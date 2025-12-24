---
作成日時: 2025-12-24
更新日時: 2025-12-24
作成者: Antigravity (AIエージェント)
最終更新者: Antigravity (AIエージェント)
---
# Skript Official Documentation Master Index
# Skript公式ドキュメント・マスターインデックス

> [!IMPORTANT]
> このファイルはSkript開発における「正典（Master Reference）」への入り口です。
> 構文の曖昧な点や新しい機能を確認する際は、まず以下の公式リンクを参照してください。

---

## 1. 公式サイト・トップ
- **[Skript Documentation Home](https://docs.skriptlang.org/)**
    - 全ドキュメントの起点。最新バージョンやお知らせを確認できます。

## 2. カテゴリ別リファレンス (Syntax Reference)

| カテゴリ | 概要 | 公式リンク |
|:---|:---|:---|
| **Events** | 処理の開始タイミング（on join, on damage等）を定義します。 | [Events List](https://docs.skriptlang.org/events.html) |
| **Conditions** | 条件分岐（if, while等）で使用するチェック項目です。 | [Conditions List](https://docs.skriptlang.org/conditions.html) |
| **Effects** | 実際にサーバーやプレイヤーに影響を与える動作（send, teleport等）です。 | [Effects List](https://docs.skriptlang.org/effects.html) |
| **Expressions** | 値やオブジェクト（player's name, location等）を取得するための構文です。 | [Expressions List](https://docs.skriptlang.org/expressions.html) |
| **Types (Classes)** | Skriptで扱うデータ型（player, itemstack, number等）の定義です。 | [Types/Classes List](https://docs.skriptlang.org/classes.html) |
| **Structures** | コードの構造（command, function, options等）を定義します。 | [Structures List](https://docs.skriptlang.org/structures.html) |
| **Sections** | 特殊な範囲処理（using, do等）を定義します。 | [Sections List](https://docs.skriptlang.org/sections.html) |
| **Functions** | カスタム関数の定義と呼び出し方法です。 | [Functions List](https://docs.skriptlang.org/functions.html) |

## 3. 重要リソース (Important Resources)
- **[Tutorials](https://docs.skriptlang.org/tutorials.html)**: 公式が提供する基本・応用ガイド。
- **[Text & Coloring](https://docs.skriptlang.org/text.html)**: メッセージのマゼンタ、16進数カラー、チャットタグの仕様。
- **[Dev Tools](https://docs.skriptlang.org/index.html)**: 開発者向けツール（Javadocs等）。
- **[GitHub Repository](https://github.com/SkriptLang/Skript/)**: Skript本体のソースコードとIssueトラッカー。

## 4. アドオン・リファレンス (Addon References)

主要なアドオンの機能概要とリファレンスです。

| アドオン | 主な機能・役割 | ドキュメント/GitHub |
|:---|:---|:---|
| **skQuery** | GUI（仮想インベントリ）、MIDI再生、YAML操作、動的なコード実行（Lambda）など、歴史ある多機能アドオン。 | [GitHub](https://github.com/SkQuery/SkQuery) / [Syntax](https://docs.skunity.com/syntax/search/addon:skquery) |
| **Skellett** | 「The Beast」と呼ばれる汎用拡張。BungeeCord連携、MySQL、エンティティのヒットボックス操作、高度なチャット演出（JSON）など、バニラでは届かない機能を網羅。 | [GitHub](https://github.com/TheLimeGlass/Skellett) / [Syntax](https://docs.skunity.com/syntax/search/addon:skellett) |
| **SkBee** | NBT操作、UI (GUI)、カスタムレシピ、ストラクチャ操作など。 | [GitHub](https://github.com/SkriptHub/SkBee) / [Syntax](https://docs.skunity.com/syntax/search/addon:skbee) |
| **skript-reflect** | Java APIを直接呼び出すための高度な連携パッケージ。 | [GitHub](https://github.com/SkriptLang/skript-reflect) / [Syntax](https://docs.skunity.com/syntax/search/addon:skript-reflect) |

---

## 5. 検索のヒント (Search Tips)
- **ブラウザ内検索 (Ctrl+F)** を活用し、特定のキーワード（例: `nbt`, `vector`, `entity`）を各リストページで検索してください。
- 複数形や所有格（`player's name` vs `name of player`）に注意してください。Skriptは柔軟ですが、公式の記述が最も確実です。

---
### 付録: 編集者情報
**エージェント名**: Antigravity (Google Deepmind)
**実施作業**: Skript公式ドキュメントの構造化インデックス作成。
**コンテキスト**: ユーザーによる「docs.skriptlang.orgをマスターとする」という指示に基づき、開発効率と正確性を高めるために構築。
