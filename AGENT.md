---
作成日時: 2025-12-23
更新日時: 2025-12-23
作成者: ユーザー
最終更新者: Antigravity (AIエージェント)
---
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
| [Docs/GameSystem/0.GameSystem_Index.md](Docs/GameSystem/0.GameSystem_Index.md) | **ゲームシステム・マスターインデックス (全仕様の入り口)** |
| [Docs/GameSystem/1.4.Combat_WeaponList.md](Docs/GameSystem/1.4.Combat_WeaponList.md) | 武器の実装・強化仕様 |
| [Docs/GameSystem/1.5.Combat_DamageStagger.md](Docs/GameSystem/1.5.Combat_DamageStagger.md) | ダメージ計算・スタッガーシステム詳細 |
| [Docs/GameSystem/3.1.Scoreboard_Sidebar_Spec.md](Docs/GameSystem/3.1.Scoreboard_Sidebar_Spec.md) | スコアボード・サイドバー詳細仕様 |

### 3. 開発リファレンス
| ファイル | 内容 |
|---------|------|
| [Docs/Development/VariableReference.md](Docs/Development/VariableReference.md) | 変数定義一覧 (命名規則、初期値) |
| [Docs/ItemDatabase/0.ItemDatabase_Index.md](Docs/ItemDatabase/0.ItemDatabase_Index.md) | **アイテムデータベース (CMD/ID/NBT管理)** |
| [Docs/Development/DataSynchronization.md](Docs/Development/DataSynchronization.md) | サーバー間データ同期仕様 (SQL/シリアライズ) |
| [Docs/Development/PerformanceAnalysis.md](Docs/Development/PerformanceAnalysis.md) | パフォーマンス負荷シミュレーション (最適化指針) |
| [Docs/Development/TechStack_Detailed.md](Docs/Development/TechStack_Detailed.md) | 技術スタック・プラグイン・依存関係詳細 |
| [Docs/Progress/2025-12-23.md](Docs/Progress/2025-12-23.md) | **最新の進捗レポート** |
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

## 戦闘バランス (Stagger)
**「スタッガー（衝撃）」システム** は、戦闘における「攻守の入れ替わり」を生み出す重要な要素です。

*   **衝撃蓄積**: 攻撃によりスタミナとは別の「体勢ゲージ」が蓄積。
*   **スタッガー状態**: ゲージが最大になると、一時的に動作不能＆被ダメージ増大。
*   **直撃のチャンス**: スタッガー中の敵には高威力武器による「直撃（Direct Hit）」が狙えます。

詳細は [1.5.Combat_DamageStagger.md](1.5.Combat_DamageStagger.md) を参照。

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

---
### 付録: 編集者情報
**エージェント名**: Antigravity (Google Deepmind)
**実施作業**: プロジェクト全体のインデックス管理、ドキュメント管理規約の導入、最新インデックスへのリンク整理。
**コンテキスト**: Tech::Playground プロジェクトの品質向上と情報の透明性を確保するための規約整備。
