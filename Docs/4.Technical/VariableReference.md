# 変数定義一覧 (Variable Reference)

## 概要
ゲーム全体で使用する変数の一覧です。
全てのSkriptファイルはこの定義に従って変数を使用してください。

---

## 命名規則 (Naming Convention)

| プレフィックス | 用途 | 例 |
|--------------|------|-----|
| `{-kp::%uuid%::*}` | **プレイヤーデータ** (永続) | `{-kp::%uuid%::rank}` |
| `{-krz::*}` | **サーバー設定/定数** | `{-krz::config::max_level}` |
| `{-sys::*}` | **システム状態** (非永続) | `{-sys::pg::status}` |
| `{_*}` | **一時変数** (関数/イベント内) | `{_damage}` |

---

## プレイヤー変数 (Player Data)

### 基本情報 `{-kp::%uuid%::...}`

| 変数名 | 型 | 説明 | 初期値 |
|-------|-----|------|-------|
| `rank` | text | ランク (F〜SSS) | `"F"` |
| `rank::exp` | number | ランク経験値 | `0` |
| `coins` | number | Δコイン残高 | `1000` |

### ステータス `{-kp::%uuid%::stat::...}`

| 変数名 | 型 | 説明 | 初期値 |
|-------|-----|------|-------|
| `stat::ap` | number | 現在AP | `100` |
| `stat::ap::max` | number | 最大AP | `100` |
| `stat::en` | number | 現在EN | `100` |
| `stat::en::max` | number | 最大EN | `100` |
| `stat::atk` | number | 攻撃力 | `10` |
| `stat::def` | number | 防御力 | `5` |
| `stat::fcs` | number | FCS (ロック性能) | `50` |

### スキルレベル `{-kp::%uuid%::skill::...}`

| 変数名 | 型 | 説明 | 初期値 |
|-------|-----|------|-------|
| `skill::cqc` | number | 近接戦闘 | `1` |
| `skill::ballistics` | number | 射撃 | `1` |
| `skill::energy` | number | EN制御 | `1` |
| `skill::athletics` | number | 体術 | `1` |
| `skill::stealth` | number | 隠密 | `1` |
| `skill::medic` | number | 衛生 | `1` |
| `skill::crafting` | number | 加工 | `1` |
| `skill::trading` | number | 取引 | `1` |

### 装備 `{-kp::%uuid%::equip::...}`

| 変数名 | 型 | 説明 | 初期値 |
|-------|-----|------|-------|
| `equip::suit` | item | Tech Suit | `air` |
| `equip::main` | item | メイン武器 | `air` |
| `equip::sub` | item | サブ武器 | `air` |
| `equip::acc::1〜5` | item | アクセサリ 1〜5 | `air` |

### 設定 `{-kp::%uuid%::settings::...}`

| 変数名 | 型 | 説明 | 初期値 |
|-------|-----|------|-------|
| `settings::pvp` | boolean | PvPマーク有効 | `false` |
| `settings::chat` | text | チャットモード | `"global"` |
| `settings::sound` | number | サウンド音量 (0-100) | `100` |

### 戦績 `{-kp::%uuid%::record::...}`

| 変数名 | 型 | 説明 | 初期値 |
|-------|-----|------|-------|
| `record::deploy` | number | 総出撃回数 | `0` |
| `record::vanish` | number | 帰還成功回数 | `0` |
| `record::death` | number | 死亡回数 | `0` |
| `record::kills` | number | キル数 | `0` |
| `record::coins::earned` | number | 総獲得Δコイン | `0` |

### 拠点 `{-kp::%uuid%::base::...}`

| 変数名 | 型 | 説明 | 初期値 |
|-------|-----|------|-------|
| `base::location` | location | 拠点座標 | `none` |
| `base::owned` | boolean | 区画所有有無 | `false` |

---

## サーバー設定 (Server Config)

### グローバル設定 `{-krz::config::...}`

| 変数名 | 型 | 説明 | 値 |
|-------|-----|------|-----|
| `config::max_rank` | text | 最大ランク | `"SSS"` |
| `config::starting_coins` | number | 初期コイン | `1000` |
| `config::vanish_distance` | number | 帰還条件 (距離m) | `500` |
| `config::vanish_time` | number | 帰還条件 (秒) | `300` |
| `config::assault_dash_delay` | number | AD発動までの秒数 | `3` |

### ワールド情報 `{-krz::world::...}`

| 変数名 | 型 | 説明 |
|-------|-----|------|
| `world::lobby` | text | ロビーワールド名 |
| `world::life` | text | 生活ワールド名 |
| `world::pg` | text | プレイグラウンドワールド名 |

---

## システム状態 (System State)

### プレイグラウンド `{-sys::pg::...}`

| 変数名 | 型 | 説明 |
|-------|-----|------|
| `pg::status` | text | PG状態 (open/closed/maintenance) |
| `pg::players` | number | PG内プレイヤー数 |
| `pg::weather` | text | 現在の天候 (clear/rain/thunder) |

### プレイヤーセッション `{-sys::player::%uuid%::...}`

| 変数名 | 型 | 説明 |
|-------|-----|------|
| `player::%uuid%::in_pg` | boolean | PG内かどうか |
| `player::%uuid%::spawn_loc` | location | PGスポーン地点 |
| `player::%uuid%::distance` | number | スポーンからの移動距離 |
| `player::%uuid%::time` | number | PG滞在時間 (秒) |
| `player::%uuid%::can_vanish` | boolean | 帰還可能か |

---

## 使用例

```skript
# プレイヤーのランクを取得
set {_rank} to {-kp::%player's uuid%::rank}

# コインを加算
add 100 to {-kp::%player's uuid%::coins}

# PG内かチェック
if {-sys::player::%player's uuid%::in_pg} is true:
    send "プレイグラウンド内です"
```

---

*最終更新: 2025-12-23*
