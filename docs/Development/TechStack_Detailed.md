# 技術スタック・依存関係詳細 (Detailed Tech Stack & Dependencies)

## 概要
本プロジェクトで使用する全ての技術、プラグイン、ライブラリのマスターリストです。
環境構築時のダウンロードや、技術仕様の調査に活用してください。

---

## 1. サーバープラットフォーム (Server Platforms)

| 技術 | バージョン (推奨) | 役割 | 公式サイト/リファレンス |
|------|-----------------|------|----------------------|
| **Paper** | 1.21.10 | メイン/PG用ゲームサーバーエンジン。最新のSkript (2.13.2+) に対応。 | [PaperMC](https://papermc.io/) |
| **Velocity** | 最新安定版 | 高性能プロキシ。サーバー間転送（Main ↔ PG）を統括。 | [Velocity](https://velocitypowered.com/) |
| **PostgreSQL** | 15+ | 全てのプレイヤーデータ、トランザクション、区画情報を一元管理。 | [PostgreSQL](https://www.postgresql.org/) |
| **Redis** | 最新安定版 | (任意) リアルタイムチャット同期やステータスキャッシュ。 | [Redis](https://redis.io/) |

---

## 2. 必須プラグイン (Required Plugins)

### 2.1 開発基盤 (Development Base)
| プラグイン | 役割 | 必須アドオン/前提 | リファレンス |
|-----------|------|-----------------|-------------|
| **Skript** | 2.13.2+ | メインの開発環境。ほぼ全てのゲームロジックを記述。 | なし | [Skript GitHub](https://github.com/SkriptLang/Skript) |
| **SkBee** | NBT操作、Displayエンティティ、カスタムレシピ、構造物操作。 | Skript | [SkBee Wiki](https://github.com/ShaneBeee/SkBee/wiki) |
| **skript-reflect** | Java/Minecraft内部APIをSkriptから直接呼び出す。高性能処理に必須。 | Skript | [Docs](https://tpgaming.github.io/skript-reflect/docs/) |
| **SkQuery** | SQLデータベース接続、HTTP要求、一部のGUI操作。 | Skript | [Docs](https://skquery.org/) |
| **ProtocolLib** | **必須**。プライベートHUD（Text Displayパケット）の送信に必要。 | なし | [Spigot](https://www.spigotmc.org/resources/protocollib.1997/) |

### 2.2 システム基盤 (System Foundation)
| プラグイン | 役割 | 連携 | リファレンス |
|-----------|------|------|-------------|
| **LuckPerms** | 権限・ランク管理。サーバー間同期対応。 | MySQL/PostgreSQL | [LuckPerms](https://luckperms.net/) |
| **Vault** | 経済APIの標準。krz-economyと他プラグインの橋渡し。 | なし | [Vault](https://github.com/MilkBowl/Vault) |
| **PlaceholderAPI** | UIやチャットへの変数表示。 | Skript, LuckPerms | [PAPI Wiki](https://github.com/PlaceholderAPI/PlaceholderAPI/wiki) |

### 2.3 演出・連携 (Visual & Integration)
| プラグイン | 役割 | 備考 | リファレンス |
|-----------|------|------|-------------|
| **DiscordSRV** | Discord ↔ Minecraft チャット同期、通知。 | 要Bot Token | [DiscordSRV](https://discordsrv.com/) |
| **BlueMap** | 生活ワールドの3D Webマップ表示。 | Mainのみ | [BlueMap](https://bluemap.bluecolored.de/) |

---

## 3. インフラ・開発ツール (Infrastructure & Dev Tools)

| ツール | 用途 | 備考 |
|-------|------|------|
| **Kubernetes (K8s)** | サーバーコンテナのオーケストレーション。 | 自宅Proxmox環境上 |
| **ArgoCD** | GitOpsによる自動デプロイ。 | GitHub連携 |
| **Garage S3** | プラグイン・スクリプトのオブジェクトストレージ管理。 | init-containerで同期 |
| **Cloudflare Tunnel** | 自宅サーバーの安全な外部公開（ポート開放不要）。 | ゼロトラスト環境 |
| **Coder** | VS Code(Web)ベースのクラウド開発環境。 | 手元のPCから接続 |

---

## 4. リソースパック (Resource Pack)

*   **Custom Model Data**: 武器やTech Suitの外見を定義。
*   **Shader / UI**: 視点固定（1人称遮蔽）やカスタムフォント、テクスチャ。
*   **Sound**: コンポジットサウンド（複数のSEを組み合わた駆動音）。

---

## 5. 調査・学習用資料リンク

*   **Skript Hub**: [https://skripthub.net/](https://skripthub.net/) (構文検索)
*   **skUnity**: [https://skunity.com/](https://skunity.com/) (アドオン/スクリプト共有)
*   **Minecraft Dev Wiki**: [https://minecraft.wiki/w/Minecraft_Wiki](https://minecraft.wiki/w/Minecraft_Wiki) (技術仕様)
*   **Paper API Docs**: [https://jd.papermc.io/paper/1.21/](https://jd.papermc.io/paper/1.21/) (reflect使用時の参照)

---

*最終更新: 2025-12-23*
