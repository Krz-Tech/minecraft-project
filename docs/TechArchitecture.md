# 技術構成ドキュメント / Technical Architecture Document

**Version:** 1.0
**Last Updated:** 2025-12-23
**Status:** Draft

---

## 1. 概要 / Overview

本ドキュメントは、Krz-Tech Minecraft Server Project の技術構成を定義します。
インフラストラクチャ、ネットワーク、サービス構成、開発環境の全体像を記載しています。

---

## 2. インフラストラクチャ全体構成 / Infrastructure Overview

```mermaid
flowchart TB
    subgraph EXTERNAL["External Network"]
        PLAYER["🎮 プレイヤー"]
        DEVELOPER["💻 開発者 (外部)"]
        VPS["VPS<br/>(Entry Point)"]
    end

    subgraph CLOUDFLARE["Cloudflare"]
        CF_CDN["Cloudflare CDN"]
        CF_TUNNEL["Cloudflare Tunnel"]
    end

    subgraph HOME["自宅サーバー (Home Server)"]
        subgraph PROXMOX["Proxmox VE"]
            subgraph K8S["Kubernetes Cluster"]
                ARGOCD["ArgoCD<br/>(GitOps)"]
                
                subgraph MC_NS["Namespace: minecraft"]
                    VELOCITY["Velocity Proxy"]
                    MC_MAIN["Main Server<br/>(Paper)"]
                    MC_PLAYGROUND["Playground Server<br/>(Paper)"]
                end
                
                subgraph SVC_NS["Namespace: services"]
                    WEBAPP["Web App +<br/>Discord Bot"]
                    DB["Database<br/>(PostgreSQL/MariaDB)"]
                end
            end
            
            subgraph CODER_VM["Coder VM / Container"]
                CODER["Coder Server"]
                DEV_WS["Dev Workspaces"]
            end
        end
    end

    %% External connections
    PLAYER -->|"MC Protocol<br/>25565"| VPS
    DEVELOPER -->|"HTTPS"| VPS
    VPS --> CF_CDN
    CF_CDN --> CF_TUNNEL
    CF_TUNNEL -->|"Tunnel"| VELOCITY
    CF_TUNNEL -->|"HTTPS"| WEBAPP

    %% Internal connections
    VELOCITY --> MC_MAIN
    VELOCITY --> MC_PLAYGROUND
    MC_MAIN --> DB
    MC_PLAYGROUND --> DB
    WEBAPP --> DB
    ARGOCD -.->|"Deploy"| MC_NS
    ARGOCD -.->|"Deploy"| SVC_NS
    
    %% Dev connections
    DEV_WS -->|"Internal IP"| MC_MAIN
    DEV_WS -->|"Internal IP"| MC_PLAYGROUND

    classDef external fill:#ff6b6b,stroke:#333,color:#fff
    classDef cloudflare fill:#f5a623,stroke:#333,color:#fff
    classDef k8s fill:#326ce5,stroke:#333,color:#fff
    classDef minecraft fill:#62b047,stroke:#333,color:#fff
    classDef service fill:#9b59b6,stroke:#333,color:#fff
    classDef dev fill:#3498db,stroke:#333,color:#fff

    class PLAYER,DEVELOPER,VPS external
    class CF_CDN,CF_TUNNEL cloudflare
    class ARGOCD,K8S k8s
    class VELOCITY,MC_MAIN,MC_PLAYGROUND minecraft
    class WEBAPP,DB service
    class CODER,DEV_WS dev
```

---

## 3. ネットワーク構成 / Network Architecture

### 3.1 外部アクセスフロー

```mermaid
sequenceDiagram
    participant P as プレイヤー
    participant V as VPS (Proxy)
    participant CF as Cloudflare CDN
    participant CT as Cloudflare Tunnel
    participant VL as Velocity (MC Proxy)
    participant MC as Minecraft Server

    P->>V: MC接続要求 (TCP:25565)
    V->>CF: Forward
    CF->>CT: Tunnel経由
    CT->>VL: 自宅サーバーへ
    VL->>MC: バックエンドサーバーへルーティング
    MC-->>P: ゲームセッション確立
```

### 3.2 アクセス経路まとめ

| 通信経路 | プロトコル | ポート | 説明 |
|---------|-----------|--------|------|
| Player → VPS | TCP | 25565 | Minecraft接続 |
| VPS → Cloudflare | TCP | 443 | CDN経由 |
| Cloudflare → Home | Tunnel | - | Cloudflare Tunnel |
| Velocity → Backend | TCP | 25565+ | 内部プロキシ |
| Web Browser → VPS | HTTPS | 443 | Webアクセス |
| Coder → MC Servers | TCP | Internal | 開発者直接アクセス |

---

## 4. Minecraft サーバー構成 / Minecraft Server Architecture

### 4.1 サーバー構成図

```mermaid
flowchart LR
    subgraph PROXY["Velocity Proxy"]
        VEL["Velocity<br/>🛡️ 認証・ルーティング"]
    end

    subgraph MAIN["Main Server Pod"]
        LOBBY["lobby<br/>🏠 ロビー"]
        LIFE1["life_world_001<br/>🏘️ 生活ワールド"]
        LIFE2["life_world_002<br/>🏘️ 生活ワールド"]
        LIFEX["life_world_xxx<br/>🏘️ ..."]
        OTHER["その他ワールド"]
    end

    subgraph PG["Playground Server Pod"]
        PG_WAIT["pg_waiting<br/>⏳ 待機ワールド"]
        PLAYGROUND["playground<br/>⚔️ プレイグラウンド"]
    end

    subgraph STORAGE["Persistent Storage"]
        PV_MAIN["PV: Main Worlds"]
        DB["Database<br/>📊 共有データ"]
    end

    VEL --> LOBBY
    VEL --> PG_WAIT
    LOBBY --> LIFE1
    LOBBY --> LIFE2
    LOBBY --> LIFEX
    LOBBY --> OTHER
    PG_WAIT --> PLAYGROUND
    PLAYGROUND -->|"帰還完了"| LOBBY
    
    MAIN --> PV_MAIN
    PG --> PV_PG
    MAIN --> DB
    PG --> DB

    classDef proxy fill:#f39c12,stroke:#333,color:#fff
    classDef main fill:#27ae60,stroke:#333,color:#fff
    classDef pg fill:#e74c3c,stroke:#333,color:#fff
    classDef storage fill:#8e44ad,stroke:#333,color:#fff

    class VEL proxy
    class LOBBY,LIFE1,LIFE2,LIFEX,OTHER main
    class PG_WAIT,PLAYGROUND pg
    class PV_MAIN,PV_PG,DB storage
```

### 4.2 サーバー詳細

| サーバー | ソフトウェア | 役割 | ワールド |
|---------|-------------|------|----------|
| Velocity Proxy | Velocity | 認証・ルーティング・セキュリティ | - |
| Main Server | Paper | 生活・ロビー・その他 | lobby, life_world_xxx, etc. |
| Playground Server | Paper | 戦闘・Extraction | pg_waiting, playground |

> **Note:** Playgroundでは帰還処理完了後、プレイヤーはMain Server (lobby) に転送されます。

### 4.3 Playground 分離の理由

- **ログ分離**: 戦闘ログ・アイテムドロップログを独立管理
- **パフォーマンス**: 戦闘負荷をメインサーバーから隔離
- **メンテナンス**: Playground のみ再起動・更新が可能
- **シンプルな構成**: 待機ワールド + プレイグラウンドのみで完結

---

## 5. サービス構成 / Service Architecture

### 5.1 サービス構成図

```mermaid
flowchart TB
    subgraph CONTAINER["Web + Bot Container (同一Pod)"]
        subgraph BACKEND["Backend (Python/FastAPI?)"]
            API["REST API<br/>サーバー状態取得"]
            SCHEDULER["Scheduler<br/>定期監視"]
        end
        
        subgraph WEBAPP_FE["Web Frontend"]
            WEB["Homepage<br/>🌐 プレイヤー向け"]
            STATUS["Status Page<br/>📊 稼働状況"]
        end
        
        subgraph BOT["Discord Bot (discord.py)"]
            BOT_STATUS["Status Commands<br/>📊 サーバー状態"]
            BOT_PROGRESS["Progress Tracker<br/>📈 開発進捗"]
            BOT_NOTIFY["Notifications<br/>🔔 ダウン通知"]
        end
    end

    subgraph EXTERNAL["External Services"]
        DISCORD["Discord API"]
        MC_SERVERS["Minecraft Servers"]
    end

    subgraph DATA["Data Layer"]
        DB["Database"]
        REDIS["Redis (Cache)"]
    end

    %% Connections
    WEB --> API
    STATUS --> API
    BOT_STATUS --> API
    BOT_NOTIFY --> API
    API --> DB
    API --> REDIS
    SCHEDULER --> MC_SERVERS
    SCHEDULER --> API
    BOT --> DISCORD
    
    classDef backend fill:#3498db,stroke:#333,color:#fff
    classDef frontend fill:#2ecc71,stroke:#333,color:#fff
    classDef bot fill:#9b59b6,stroke:#333,color:#fff
    classDef external fill:#e74c3c,stroke:#333,color:#fff
    classDef data fill:#f39c12,stroke:#333,color:#fff

    class API,SCHEDULER backend
    class WEB,STATUS frontend
    class BOT_STATUS,BOT_PROGRESS,BOT_NOTIFY bot
    class DISCORD,MC_SERVERS external
    class DB,REDIS data
```

### 5.2 サービス詳細

| サービス | 技術スタック | 機能 |
|---------|-------------|------|
| Web Frontend | HTML/CSS/JS (静的) | ホームページ、ステータス表示 |
| Backend API | Python (FastAPI 推奨) | サーバー状態API、DB接続 |
| Discord Bot | discord.py | 状態通知、開発進捗、コマンド |
| Database | PostgreSQL / MariaDB | 共有プレイヤーデータ |
| Cache | Redis (オプション) | 状態キャッシュ、セッション |

### 5.3 同一コンテナ構成の利点

- **内部通信**: localhost 経由で高速通信
- **デプロイ簡易化**: 単一イメージで管理
- **リソース共有**: DB接続プールの共有

---

## 6. データベース設計方針 / Database Design Principles

### 6.1 データ保存戦略

```mermaid
flowchart LR
    subgraph WORLD_DATA["ワールドデータ (Paper デフォルト)"]
        WD1["プレイヤー位置"]
        WD2["インベントリ"]
        WD3["チャンク"]
        WD4["エンティティ"]
    end

    subgraph DB_DATA["データベース (共有データ)"]
        DB1["プレイヤー統計<br/>(ランク・経験値)"]
        DB2["区画所有情報"]
        DB3["サーバー間共有<br/>アイテム・通貨"]
        DB4["トランザクション<br/>ログ"]
    end

    subgraph BACKUP["バックアップ"]
        WORLD_BK["ワールドバックアップ<br/>(rsync/rclone)"]
        DB_BK["DB差分バックアップ<br/>(pg_dump/mysqldump)"]
    end

    WORLD_DATA --> WORLD_BK
    DB_DATA --> DB_BK

    classDef world fill:#27ae60,stroke:#333,color:#fff
    classDef db fill:#3498db,stroke:#333,color:#fff
    classDef backup fill:#95a5a6,stroke:#333,color:#fff

    class WD1,WD2,WD3,WD4 world
    class DB1,DB2,DB3,DB4 db
    class WORLD_BK,DB_BK backup
```

### 6.2 推奨構成

| データ種別 | 保存先 | バックアップ方式 |
|-----------|--------|-----------------|
| ワールド・チャンク | Paper デフォルト (ファイル) | 定期 rsync + 世代管理 |
| プレイヤー統計 | Database | 日次差分バックアップ |
| 区画・所有情報 | Database | トランザクションログ |
| 経済データ | Database | リアルタイムレプリケーション (将来) |

---

## 7. 開発環境 / Development Environment

### 7.1 開発フロー

```mermaid
flowchart LR
    subgraph DEV["開発者環境"]
        LOCAL["ローカルPC"]
        CODER_WS["Coder Workspace"]
    end

    subgraph SERVERS["サーバー環境"]
        DEV_MC["開発サーバー<br/>(Workspace内)"]
        STAGING["ステージング<br/>(共有)"]
        PROD["本番環境<br/>(K8s)"]
    end

    subgraph VCS["バージョン管理"]
        GITHUB["GitHub Repository"]
        ARGOCD["ArgoCD"]
    end

    LOCAL -->|"SSH/HTTPS"| CODER_WS
    CODER_WS -->|"Internal IP"| DEV_MC
    CODER_WS -->|"Internal IP"| STAGING
    CODER_WS -->|"git push"| GITHUB
    GITHUB -->|"Webhook"| ARGOCD
    ARGOCD -->|"Deploy"| PROD
    
    classDef dev fill:#3498db,stroke:#333,color:#fff
    classDef server fill:#27ae60,stroke:#333,color:#fff
    classDef vcs fill:#e74c3c,stroke:#333,color:#fff

    class LOCAL,CODER_WS dev
    class DEV_MC,STAGING,PROD server
    class GITHUB,ARGOCD vcs
```

### 7.2 環境一覧

| 環境 | アクセス方法 | 用途 |
|------|-------------|------|
| 開発サーバー | Coder Workspace 内スクリプト起動 | 個人開発・テスト |
| ステージング | 共有 K8s Pod (内部IPアクセス) | 統合テスト・QA |
| 本番 | K8s (ArgoCD 経由デプロイ) | プレイヤー向け |

### 7.3 アクセス制御

```mermaid
flowchart TB
    subgraph PUBLIC["公開アクセス"]
        PLAYER["プレイヤー"]
    end

    subgraph INTERNAL["内部アクセス (自宅NW)"]
        DEV["開発者<br/>(Coder経由)"]
        ADMIN["管理者"]
    end

    subgraph SERVERS["サーバー群"]
        PROD["本番環境"]
        STAGING["ステージング"]
        DEV_SRV["開発サーバー"]
    end

    PLAYER -->|"Cloudflare経由"| PROD
    DEV -->|"Internal IP"| DEV_SRV
    DEV -->|"Internal IP"| STAGING
    DEV -->|"Internal IP"| PROD
    ADMIN -->|"Direct"| PROD

    classDef public fill:#e74c3c,stroke:#333,color:#fff
    classDef internal fill:#27ae60,stroke:#333,color:#fff
    classDef server fill:#3498db,stroke:#333,color:#fff

    class PLAYER public
    class DEV,ADMIN internal
    class PROD,STAGING,DEV_SRV server
```

---

## 8. Kubernetes マニフェスト構成案 / K8s Manifest Structure

```
kubernetes/
├── base/
│   ├── namespace.yaml
│   ├── configmap.yaml
│   └── secrets.yaml (sealed)
├── minecraft/
│   ├── velocity/
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── configmap.yaml
│   ├── main-server/
│   │   ├── statefulset.yaml
│   │   ├── service.yaml
│   │   ├── pvc.yaml
│   │   └── configmap.yaml
│   └── playground/
│       ├── statefulset.yaml
│       ├── service.yaml
│       ├── pvc.yaml
│       └── configmap.yaml
├── services/
│   ├── webapp/
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── ingress.yaml
│   └── database/
│       ├── statefulset.yaml
│       ├── service.yaml
│       └── pvc.yaml
└── argocd/
    └── applications.yaml
```

---

## 9. 今後の検討事項 / Future Considerations

### 優先度: 高

- [ ] Velocity の詳細設定 (player limit, timeout, forwarding)
- [ ] Database スキーマ設計
- [ ] Cloudflare Tunnel 設定

### 優先度: 中

- [ ] Discord Bot コマンド仕様
- [ ] バックアップ自動化スクリプト
- [ ] 監視・アラート (Prometheus/Grafana)

### 優先度: 低

- [ ] ディザスタリカバリ計画

---

## Appendix: 技術選定理由

| 技術 | 選定理由 |
|------|----------|
| **Velocity** | 最新のMCプロキシ、modern forwarding対応、パフォーマンス |
| **Paper** | 安定性、プラグイン互換性、Skript対応 |
| **PostgreSQL** | 信頼性、JSON対応、差分バックアップ容易 |
| **ArgoCD** | GitOpsによる宣言的デプロイ、K8sネイティブ |
| **Cloudflare Tunnel** | ポート開放不要、DDoS対策、ゼロトラスト |

---

*Generated by Antigravity Agent - 2025-12-23*
