# AGENT.md - AI Agent Context Index

> [!IMPORTANT]
> **このファイルはAIエージェント用のコンテキストインデックスです。**
> 新しいチャットや作業開始時に、必ずこのファイルと参照先を読み込んでください。

---

## プロジェクト概要
*   **プロジェクト名**: Tech::Playground
*   **ジャンル**: Tech Extraction RPG (Tarkov × Armored Core × Minecraft)
*   **世界観**: SF/Tech。魔法禁止。
*   **技術スタック**: Skript (メイン), Paper/Velocity, Kubernetes, ArgoCD

---

## 必須参照ドキュメント

### 1. エージェントルール
| ファイル | 内容 |
|---------|------|
| [docs/agent/GEMINI.md](docs/agent/GEMINI.md) | 全体ルール、言語設定、禁止事項 |
| [docs/agent/MINECRAFT.md](docs/agent/MINECRAFT.md) | ゲーム仕様サマリー、サーバー構成 |
| [docs/agent/SKRIPT.md](docs/agent/SKRIPT.md) | Skriptコーディング規約、命名規則 |

### 2. ゲーム仕様
| ファイル | 内容 |
|---------|------|
| [Docs/GameSystem/0.GameSystem_General.md](Docs/GameSystem/0.GameSystem_General.md) | コアゲームループ、経済、ランク |
| [Docs/GameSystem/1.GameSystem_BattleStyle.md](Docs/GameSystem/1.GameSystem_BattleStyle.md) | 戦闘システム、Tech Suit、スマートエイム |
| [Docs/GameSystem/1.1.Combat_Dash.md](Docs/GameSystem/1.1.Combat_Dash.md) | ダッシュシステム (AD/QB) 詳細 |
| [Docs/GameSystem/1.2.Combat_SmartAim.md](Docs/GameSystem/1.2.Combat_SmartAim.md) | スマートエイム (FCS) 詳細 |
| [Docs/GameSystem/1.3.Combat_Melee.md](Docs/GameSystem/1.3.Combat_Melee.md) | 近接武器システム詳細 |
| [Docs/GameSystem/1.4.Combat_WeaponList.md](Docs/GameSystem/1.4.Combat_WeaponList.md) | 武器マスターリスト、強化変換仕様 |
| [Docs/GameSystem/1.GameSystem_LifeStyle.md](Docs/GameSystem/1.GameSystem_LifeStyle.md) | 生活システム、区画、家具 |
| [Docs/GameSystem/1.GameSystem_MMORPG.md](Docs/GameSystem/1.GameSystem_MMORPG.md) | スキルシステム、パッシブ効果 |
| [Docs/GameSystem/2.GameSystem_PlayGround.md](Docs/GameSystem/2.GameSystem_PlayGround.md) | プレイグラウンド詳細、PvP、帰還 |
| [Docs/GameSystem/3.GameSystem_UX.md](Docs/GameSystem/3.GameSystem_UX.md) | UXガイドライン、カラー、サウンド |
| [Docs/GameSystem/4.GameSystem_Menu.md](Docs/GameSystem/4.GameSystem_Menu.md) | メインメニュー仕様 |

### 3. 開発リファレンス
| ファイル | 内容 |
|---------|------|
| [Docs/Development/VariableReference.md](Docs/Development/VariableReference.md) | 変数定義一覧 (命名規則、初期値) |
| [Docs/ItemDatabase/0.ItemDatabase_Index.md](Docs/ItemDatabase/0.ItemDatabase_Index.md) | **アイテムデータベース (CMD/ID/NBT管理)** |
| [Docs/Development/DataSynchronization.md](Docs/Development/DataSynchronization.md) | サーバー間データ同期仕様 (SQL/シリアライズ) |
| [Docs/Development/PerformanceAnalysis.md](Docs/Development/PerformanceAnalysis.md) | パフォーマンス負荷シミュレーション (最適化指針) |
| [Docs/Development/TechStack_Detailed.md](Docs/Development/TechStack_Detailed.md) | 技術スタック・プラグイン・依存関係詳細 |
| [Docs/TechArchitecture.md](Docs/TechArchitecture.md) | インフラ構成、ネットワーク図 |
| [Docs/PluginArchitecture.md](Docs/PluginArchitecture.md) | プラグイン構成、カスタムモジュール |
| [Docs/WebDocsList.md](Docs/WebDocsList.md) | 参照資料インデックス |

---

## クイックリファレンス

### 変数プレフィックス
```
{-kp::%uuid%::*}  → プレイヤーデータ
{-krz::*}         → サーバー設定
{-sys::*}         → システム状態
{_*}              → 一時変数
```

### カラーパレット
```
&b (Cyan)    → システムメッセージ
&a (Green)   → 成功
&c (Red)     → 警告/エラー
&6 (Gold)    → 通貨/重要
&7 (Gray)    → 補足
```

### 禁止事項
*   魔法・ファンタジー用語の使用
*   Skriptの非推奨構文
*   未確認の破壊的変更

---

*最終更新: 2025-12-23*
