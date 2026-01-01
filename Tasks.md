# ドキュメント作成進捗 (Document Progress)

`Docs/00.Index.md` に定義された全ドキュメントの作成・整備状況一覧です。

## 0. System (システム・インフラ)
| 状況 | ファイル名 | 説明 |
|:---:|:---|:---|
| [x] | `Docs/0.System/0.Sys_Flow.md` | 開発・デプロイメントパイプライン定義 |
| [x] | `Docs/0.System/0.Sys_Deployment.md` | 本番環境デプロイメント定義 |
| [x] | `Docs/0.System/99.Review_Sys_Flow.md` | フロー技術レビュー |

## 1. Development (開発ガイド)
| 状況 | ファイル名 | 説明 |
|:---:|:---|:---|
| [x] | `Docs/1.Development/1.Dev_Minecraft.md` | Minecraftサーバー開発ガイド |
| [x] | `Docs/1.Development/1.Dev_Skript.md` | Skript開発ベストプラクティス |
| [x] | `Docs/1.Development/1.Dev_Coder.md` | Coder環境利用ガイド |

## 2. Game System (ゲームシステム詳細)

### 2.Game_Battle
| 状況 | ファイル名 | 説明 |
|:---:|:---|:---|
| [x] | `Docs/2.GameSystem/2.Game_Battle/BattleStyle.md` | 戦闘スタイル定義 |
| [=] | `Docs/2.GameSystem/2.Game_Battle/Playground.md` | プレイグラウンド詳細 |

### 2.Game_Core
| 状況 | ファイル名 | 説明 |
|:---:|:---|:---|
| [x] | `Docs/2.GameSystem/2.Game_Core/CoreContents.md` | コアコンテンツ定義 |
| [=] | `Docs/2.GameSystem/2.Game_Core/WorldView.md` | 世界観設定 |
| [=] | `Docs/2.GameSystem/2.Game_Core/MobSociety.md` | モブ社会・各ディメンションの役割 |
| [=] | `Docs/2.GameSystem/2.Game_Core/DeltaGuild.md` | Delta Guild・プレイグラウンド探索支援組織 |
| [x] | `Docs/2.GameSystem/2.Game_Core/Quest.md` | クエストシステム |
| [x] | `Docs/2.GameSystem/2.Game_Core/LifeStyle.md` | 生活スタイル定義 |
| [=] | `Docs/2.GameSystem/2.Game_Core/99.Review_WorldView.md` | 世界観レビュー |

### 2.Game_Economy
| 状況 | ファイル名 | 説明 |
|:---:|:---|:---|
| [=] | `Docs/2.GameSystem/2.Game_Economy/Currency.md` | 通貨システム |
| [x] | `Docs/2.GameSystem/2.Game_Economy/Shops.md` | ショップシステム |

### 2.Game_Items
| 状況 | ファイル名 | 説明 |
|:---:|:---|:---|
| [x] | `Docs/2.GameSystem/2.Game_Items/Accessories.md` | アクセサリ仕様 |
| [x] | `Docs/2.GameSystem/2.Game_Items/Consumables.md` | 消耗品仕様 |
| [x] | `Docs/2.GameSystem/2.Game_Items/Equipments.md` | 装備仕様 |
| [x] | `Docs/2.GameSystem/2.Game_Items/Tools.md` | ツール仕様 |
| [x] | `Docs/2.GameSystem/2.Game_Items/Weapons.md` | 武器仕様 |
| [ ] | `Docs/2.GameSystem/2.Game_Items/Accessories/*` | 個別アクセサリ定義詳細 |
| [ ] | `Docs/2.GameSystem/2.Game_Items/Consumables/*` | 個別消耗品定義詳細 |
| [ ] | `Docs/2.GameSystem/2.Game_Items/Equipments/*` | 個別装備定義詳細 |
| [ ] | `Docs/2.GameSystem/2.Game_Items/Materials/*` | 個別素材定義詳細 |
| [ ] | `Docs/2.GameSystem/2.Game_Items/Tools/*` | 個別ツール定義詳細 |
| [ ] | `Docs/2.GameSystem/2.Game_Items/Weapons/*` | 個別武器定義詳細 |

## 3. UX Optimization (UX最適化)
| 状況 | ファイル名 | 説明 |
|:---:|:---|:---|
| [=] | `Docs/3.UXOptimization/3.UX_Design.md` | UI/UXデザイン全体像 |
| [ ] | `Docs/3.UXOptimization/3.UX_GameDesign.md` | ゲームプレイ快適性設計 |
| [ ] | `Docs/3.UXOptimization/3.UX_ResourcePacks.md` | リソースパック仕様 |

## 4. DX Optimization (開発者体験)
| 状況 | ファイル名 | 説明 |
|:---:|:---|:---|
| [x] | `Docs/4.DXOptimization/4.DX_Discord.md` | Discord連携運用 |
| [x] | `Docs/4.DXOptimization/4.DX_Environment.md` | 開発環境構築 |
| [x] | `Docs/4.DXOptimization/4.DX_Github.md` | GitHub/Git運用ルール |

---

## 次回作業タスク (Next Tasks)

### 優先度: 高（ゲームプレイに直結）
| 状況 | タスク | 関連ドキュメント |
|:---:|:---|:---|
| [x] | Δコインの相場感・アイテム売却価格の設計 | `Currency.md` |
| [ ] | ランクシステム数値設計（必要経験値、ロスト割合） | `DeltaGuild.md` |
| [x] | 帰還条件の具体値（時間・移動距離） | `Playground.md` |
| [ ] | レアバイオーム追加報酬の数値設計（Δコイン量、経験値量、ドロップ率） | `Playground.md` |

### 優先度: 中（コンテンツ設計）
| 状況 | タスク | 関連ドキュメント |
|:---:|:---|:---|
| [x] | プレイグラウンド詳細設計（マップ生成、救出システム） | `Playground.md` |
| [ ] | PvPルール設計 | `Playground.md`, `BattleStyle.md` |
| [ ] | バイオーム特化素材・限定アイテム設計 | `2.Game_Items/*`, `Playground.md` |
| [ ] | 戦闘システム詳細設計 | `BattleStyle.md` |
| [ ] | ファントム救助の演出・UI設計 | `Playground.md` |

### 優先度: 低（後から詰めても可）
| 状況 | タスク | 関連ドキュメント |
|:---:|:---|:---|
| [ ] | オーバーワールド中心都市詳細 | `WorldView.md` |
| [ ] | NPCキャラクター設定 | `DeltaGuild.md` |
| [ ] | チュートリアル設計 | 新規作成 |
| [ ] | ストーリーライン・イベント設計 | 新規作成 |
| [ ] | クエストUIデザイン | `Quest.md` |
| [ ] | LLMプロンプト詳細設計 | `Quest.md` |
| [ ] | クエスト報酬の具体的数値設計 | `Quest.md` |

---

**凡例:**
* [x]: ドキュメントファイルが存在する（内容は要確認の可能性あり）
* [=]: ドキュメント内容が確定済み（2026/01/01 世界観設定完了）
* [ ]: ドキュメント自体が未作成、またはタスク未着手
