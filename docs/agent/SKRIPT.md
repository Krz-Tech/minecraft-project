# Skript Development Agent Prompt
# Skript開発エージェント 補足プロンプト

> [!NOTE]
> このプロンプトは`Agent/GEMINI.md`と併用してください。
> Skript開発タスク時に追加で参照されます。

---

## 対象範囲 | Scope

このプロンプトは以下のタスクに適用されます：
- Skriptスクリプトの作成・修正
- Skriptアドオンの活用
- Skriptコードレビュー

---

## 公式リソース | Official Resources

### ドキュメント

| リソース | URL | 用途 |
|----------|-----|------|
| **Skript公式ドキュメント** | https://docs.skriptlang.org | 構文リファレンス |
| **SkriptHub** | https://skripthub.net/docs/ | アドオン含む構文検索 |
| **skUnity** | https://docs.skunity.com | コミュニティドキュメント |

### GitHub

| リポジトリ | URL | 用途 |
|-----------|-----|------|
| SkriptLang/Skript | https://github.com/SkriptLang/Skript | メインリポジトリ |
| skript-reflect | https://github.com/SkriptLang/skript-reflect | Java連携アドオン |
| SkBee | https://github.com/SkriptHub/SkBee | 拡張機能アドオン |

---

## 推奨アドオン | Recommended Addons

| アドオン | 用途 | 備考 |
|---------|------|------|
| **SkBee** | NBT、レシピ、スコアボード等 | 必須級 |
| **skript-reflect** | Java API直接呼び出し | 高度な機能用 |
| **SkQuery** | SQL、ユーティリティ | DB連携時 |

---

## コーディング規約 | Coding Standards

### インデント・フォーマット

> [!IMPORTANT]
> インデントは**4スペース**または**1タブ**のどちらかに統一してください。混在は禁止です。

```skript
# 良い例: 一貫したインデント
on join:
    send "Welcome!" to player
    if player has permission "vip":
        send "&6VIP特典が有効です" to player

# 悪い例: 混在インデント（禁止）
on join:
	send "Welcome!" to player  # タブ
    if player has permission "vip":  # スペース
```

### 変数命名規則

| 用途 | 形式 | 例 |
|------|------|-----|
| **プレイヤー固有** | `{variable::%player's uuid%}` | `{home::%player's uuid%}` |
| **一時変数（RAM）** | ハイフン始まり `{-variable}` | `{-temp::cooldown::%player%}` |
| **階層区切り** | `::` を使用 | `{player::stats::level::%uuid%}` |
| **定数（単一スクリプト）** | `options`使用 | `options: Prefix: "&a[Server] "` |
| **共有定数** | `SCREAMING-HYPHEN-CASE` | `{GLOBAL-MAX-LEVEL}` |

```skript
# 良い例: UUID使用（名前変更に対応）
set {home::%player's uuid%} to location of player

# 悪い例: プレイヤー名使用（名前変更で消失）
set {home::%player%} to location of player
```

### 一時変数（RAM変数）

```skript
# RAM変数: サーバー再起動で自動削除
# variables.csvに保存されないため軽量
set {-cooldown::%player's uuid%} to now

# 使い終わったら明示的に削除
delete {-cooldown::%player's uuid%}
```

---

## コード構造 | Code Structure

### スパゲッティコードの回避

```skript
# 悪い例: 深いネスト
on right click:
    if player is holding diamond sword:
        if player has permission "combat.special":
            if {cooldown::%player's uuid%} is not set:
                # 処理...

# 良い例: 早期リターン
on right click:
    if player is not holding diamond sword:
        stop
    if player doesn't have permission "combat.special":
        stop
    if {cooldown::%player's uuid%} is set:
        stop
    
    # メイン処理（ネストが浅い）
    # ...
```

### 関数の活用

```skript
# 関数定義: camelCase命名
function calculateDamage(attacker: player, baseDamage: number) :: number:
    set {_critChance} to 0.1
    set {_multiplier} to 1
    
    # クリティカル判定
    if chance of {_critChance} * 100%:
        set {_multiplier} to 1.5
        send action bar "&c&lクリティカル！" to {_attacker}
    
    return {_baseDamage} * {_multiplier}

# 関数呼び出し
on damage:
    set {_finalDamage} to calculateDamage(attacker, 10)
    set damage to {_finalDamage}
```

### 効率的な処理

```skript
# setチェンジャーを優先（add/removeより高速）
# 悪い例
add "item1" to {list::*}
remove "item1" from {list::*}

# 良い例（可能な場合）
set {list::item1} to true
delete {list::item1}
```

---

## 命名規則 | Naming Conventions

| 要素 | 形式 | 例 |
|------|------|-----|
| **関数** | `camelCase` | `calculateDamage`, `sendWelcome` |
| **オプション** | `PascalCase` | `Prefix`, `MaxLevel`, `WelcomeMessage` |
| **変数パーツ** | `hyphen-case` | `{player-data::current-hp}` |
| **ローカル変数** | `{_variableName}` | `{_damage}`, `{_targetPlayer}` |

---

## コメント規則 | Comment Rules

```skript
#  セクションコメント（2スペース後にテキスト）
#  ==============================
#  戦闘システム
#  ==============================

on damage:
    #  ダメージ計算を開始
    set {_baseDamage} to damage
    
    #  防御力による軽減を適用
    #  防御力1ポイントにつき5%軽減
    set {_reduction} to {defense::%victim's uuid%} * 0.05
    set damage to {_baseDamage} * (1 - {_reduction})
```

### コメント必須の場面
- 複雑な計算ロジック
- 他プラグイン/アドオンとの連携部分
- ワークアラウンド（回避策）を使用している箇所
- マジックナンバー（意味のある数値）の説明

---

## オプション（定数）の使用 | Using Options

```skript
options:
    #  サーバー設定
    Prefix: "&a[KrzServer] &r"
    MaxLevel: 100
    
    #  戦闘設定
    CriticalChance: 0.1
    CriticalMultiplier: 1.5

on join:
    send "{@Prefix}ようこそ！" to player

on damage:
    if chance of {@CriticalChance} * 100%:
        set damage to damage * {@CriticalMultiplier}
```

---

## パフォーマンス最適化 | Performance Tips

> [!WARNING]
> 以下の処理はサーバー負荷が高くなる可能性があります。

### 避けるべきパターン

```skript
# 毎tick処理は極力避ける
every tick:
    loop all players:
        # 重い処理...

# variables.csvへの頻繁なアクセス
every second:
    set {counter} to {counter} + 1  # 毎秒書き込み発生
```

### 推奨パターン

```skript
# 必要な時だけ処理
on player move:
    # 距離チェックで処理頻度を削減
    if distance between player's location and {-lastCheck::%player's uuid%} < 5:
        stop
    set {-lastCheck::%player's uuid%} to player's location
    # メイン処理...

# RAM変数で頻繁な更新を処理
every 5 seconds:
    set {-tempCounter} to {-tempCounter} + 1
    # 一定間隔でのみ永続化
    if {-tempCounter} >= 12:  # 1分ごと
        set {counter} to {counter} + {-tempCounter}
        set {-tempCounter} to 0
```

---

## 禁止事項 | Prohibited Practices

> [!CAUTION]
> 以下の実装は禁止です。

1. **プレイヤー名での変数保存**
   - 必ず`%player's uuid%`を使用

2. **無限ループの可能性があるコード**
   - ループには必ず終了条件を設定

3. **未テストのスクリプトの本番投入**
   - テストサーバーで動作確認必須

4. **他プラグインの内部実装に依存したコード**
   - 公開APIのみを使用

---

## デバッグ | Debugging

```skript
# デバッグ用ブロードキャスト
options:
    Debug: true

function debugLog(message: text):
    if {@Debug} is true:
        broadcast "&7[DEBUG] %{_message}%"

# 使用例
on damage:
    debugLog("Damage event triggered: %damage%")
```

---

## スクリプトテンプレート | Script Template

```skript
#  ==============================
#  スクリプト名: example.sk
#  説明: このスクリプトの概要
#  作成者: 名前
#  作成日: 2025-12-23
#  ==============================

options:
    Prefix: "&a[Example] &r"
    Version: "1.0.0"

#  ==============================
#  関数定義
#  ==============================

function exampleFunction(player: player) :: text:
    return "Hello, %{_player}%!"

#  ==============================
#  イベントハンドラ
#  ==============================

on join:
    send "{@Prefix}%exampleFunction(player)%" to player

#  ==============================
#  コマンド
#  ==============================

command /example:
    permission: example.use
    description: サンプルコマンド
    trigger:
        send "{@Prefix}バージョン: {@Version}" to player
```

---

## 更新履歴 | Changelog

| バージョン | 日付 | 変更内容 |
|-----------|------|----------|
| 1.0.0 | 2025-12-23 | 初版作成（ベストプラクティス含む） |
