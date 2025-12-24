# Skript Coding Standard
# Skriptコーディング規約

> [!IMPORTANT]
> Skriptを作成・修正する際は必ずこの規約に従ってください。保守性とパフォーマンスを最優先します。
> 構文の最新仕様については **[Skript公式ドキュメント](https://docs.skriptlang.org/)** および **[マスターインデックス](../Development/Skript_Docs_Master_Index.md)** を常に参照してください。

---

## 1. 基本ルール
*   **言語**: 変数名・オプション名は英語 (English)。メッセージ・Loreは日本語 (Japanese)。
*   **インデント**: Tab (4 spaces width)。
*   **バージョン**: Skript 2.9+ (最新構文を使用)。

## 2. 命名規則 (Naming Convention)

### 変数 (Variables)
Krz-Techプロジェクト固有のプレフィックスを使用し、スコープを明確にする。

| 種別 | プレフィックス | 例 | 備考 |
|-----|-------------|---|------|
| **一時変数** | `_{variable}` | `_damage`, `_{_target}` | 関数・イベント内のみ |
| **プレイヤー** | `{-kp::%player's uuid%::key}` | `{-kp::%player's uuid%::level}` | 永続データ(MySQL連携推奨) |
| **設定/定数** | `{-krz::config::key}` | `{-krz::config::max_level}` | サーバー共通設定 |
| **システム** | `{-sys::key}` | `{-sys::pg_status}` | システム内部状態 |

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
    send "{@PREFIX} 初期化を開始します..." to {_p}
    # 処理...

on join:
    pg_init(player)
```
