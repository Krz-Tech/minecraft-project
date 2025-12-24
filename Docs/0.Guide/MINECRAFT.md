# Minecraft Development Guidelines
# Minecraft開発ガイドライン

> [!NOTE]
> ゲーム仕様とサーバー構成に関する知識ベースです。

---

## 1. サーバー構成 (Architecture)
*   **Proxy**: Velocity (高速・セキュア)
*   **Backend**: Paper (最新安定版)
    *   **Main Server**: 生活ワールド (`lobby`, `life_world`)
    *   **Playground**: 戦闘ワールド (`pg_waiting`, `playground`)

## 2. ゲームシステム仕様 (Game Specs)
詳細は `Docs/GameSystem/*.md` を参照。ここでは要約のみ記す。

### 世界観
*   **Tech / Sci-Fi / Cyberpunk**
*   魔法なし。剣や弓も「ガジェット」「Tech武器」として扱う。

### コアゲームループ (Extraction)
1.  **生活ワールド (Safe)**: 装備購入、ハウジング、休息。
2.  **Playground (Danger)**:
    *   **目的**: アイテム収集 (Scope素材) して生きて帰る。
    *   **死**: Non-Scopeアイテム全ロスト。装備耐久減少。ランク経験値減。
    *   **帰還**: 専用ポスト (Delivery Post) にアイテムが届く。

### 戦闘 (Battle Style)
*   **スマートエイム**: 3人称視点でのロックオン・誘導射撃。
*   **Tech Suit**: 全身一体型防具。FCS（ロック性能）やEN（エネルギー）を管理。
*   **アクション**: 通常ダッシュ（無限）→ **アサルトダッシュ**（EN消費・高速巡航）。

### 経済 (Economy)
*   **通貨**: **Δ (デルタ)**
*   **Scope / Non-Scope**:
    *   Scope: 持ち帰り可能なレア素材。
    *   Non-Scope: 帰還時にシステムが自動換金する一般資材。
*   **建材**: 生活ワールドのショップで購入（安価）。

## 3. プラグイン構成
*   **Skript**: ゲームロジックの中核（`krz-*` シリーズ）。
*   **DiscordSRV**: チャット同期。
*   **LuckPerms**: 権限管理。
*   **BlueMap**: 生活ワールドの地図（Playgroundは非表示）。
