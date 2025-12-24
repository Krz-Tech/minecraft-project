---
作成日時: 2025-12-23
更新日時: 2025-12-24
作成者: Antigravity (AIエージェント)
最終更新者: Antigravity (AIエージェント)
---

# Skript Coding Standard
# Skriptコーディング規約

> [!IMPORTANT]
> Skriptを作成・修正する際は必ずこの規約に従ってください。保守性とパフォーマンスを最優先します。
> 構文の最新仕様については **[Skript公式ドキュメント](https://docs.skriptlang.org/)** および **[変数定義一覧](../4.Technical/VariableReference.md)** を常に参照してください。

---

## 1. 基本ルール
*   **言語**: 変数名・オプション名は英語 (English)。メッセージ・Loreは日本語 (Japanese)。
*   **インデント**: Tab (4 spaces width)。
*   **バージョン**: Skript 2.9+ (最新構文を使用)。

## 2. 命名規則 (Naming Convention)

### 変数 (Variables)
変数の命名およびスコープについては **[変数定義一覧 (VariableReference.md)](../4.Technical/VariableReference.md)** に完全に準拠してください。

*   リスト変数は `::` で区切る (Kebab-case推奨)。
*   `{variable}` の `{}` 囲みはSkript 2.9以降の推奨構文に従う。

### 関数 (Functions)
`module_action_detail` の形式でスネークケース。

*   `core_get_player_level(p: player)`
*   `pg_spawn_enemy(loc: location, type: text)`

## 3. 推奨アドオン (Addons)
*   **SkBee**: NBT操作、UI (GUI)、レシピ、構造物。
*   **skQuery**: GUI作成 (`format slot`)、MIDI再生、YAML操作、動的な評価 (Lambda)。
*   **Skellett**: BungeeCord連携、MySQL、エンティティの高度な操作、高度なチャット演出。
*   **skript-reflect**: Java APIの直接呼び出し（高度な機能のみ）。
    *   `import "org.bukkit.entity.Player"`

## 4. パフォーマンス対策
*   **Loopの最適化**: `leaf` エイリアスを使用せず、可能な限りフィルタリングしてからループする。
*   **定期実行の抑制**: `every 1 second` などの乱用禁止。可能な限りイベント駆動 (`on damage`, `on move`等) にする。
*   **NBT利用**: 独自アイテムの判定は名前(Name)ではなく **NBTタグ (CustomModelData等)** で行うこと。

## 5. コードテンプレート

```skript
# -------------------------------------------------------------------------
# Module: Krz-Playground Core
# Author: AI Agent
# Since: 2025-12-23
# -------------------------------------------------------------------------

options:
    PREFIX: "&b[Playground] &r"

function pg_init(p: player):
    send "%{@PREFIX}% 初期化を開始します..." to {_p}
    # 処理...

on join:
    pg_init(player)
```

## 6. 開発ワークフロー (Development Workflow)

作業の確実性と即時性を担保するため、エージェントは以下の手順を厳守すること。

1.  **コンソール・アタッチ**: 作業（コーディング・修正）を行なう際は、常に `./mc attach s` を使用してサーバーコンソールにアタッチした状態を維持すること。これにより、リロード時のエラーやシステムメッセージを即座に検知できるようにする。
2.  **即時リロードと検証**: ファイルの保存・変更完了時には、必ず当該スクリプトのリロードコマンドを実行し、エラーが発生していないことを確認すること。
    *   リロードコマンド: `sk reload <script_name>` または `sk reload all`
3.  **エラー解決の優先**: エラーが検出された場合は、他の作業を進める前に最優先で修正・解決すること。

---
### 付録: 編集者情報
**エージェント名**: Antigravity (Google Deepmind)
**実施作業**: 開発ワークフロー（コンソール接続およびリロード検証の義務化）の追加、およびドキュメント管理規約に基づくメタデータの整備。
**コンテキスト**: サーバー開発における変更の即時反映と、デバッグ効率の最大化を目的とした規約更新。
