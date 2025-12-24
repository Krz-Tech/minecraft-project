---
作成日時: 2024-12-24
更新日時: 2024-12-24
作成者: Antigravity
最終更新者: Antigravity
---

# Minecraft 1.21.4 プロトコル変更点まとめ (ProtocolLib向け)

Minecraft 1.21.4 (Bundles of Bravery / The Garden Awakens) で導入されたプロトコルおよびパケット構造の変更点についてまとめます。ProtocolLib を使用してパケットを操作する際の参照用ドキュメントです。

## 1. 基本情報
- **プロトコルバージョン**: `769`
- **リリース日**: 2024年12月3日
- **Data Pack バージョン**: 61
- **Resource Pack バージョン**: 46

---

## 2. 新規追加されたパケット

### 2.1 ServerboundPlayerInputPacket (Client -> Server)
従来の「Steer Vehicle」パケットなどの一部機能を統合・置換したパケットです。プレイヤーの移動入力をサーバーに送ります。
入力状態が変化した際に送信されます。

**構造 (Input Record):**
1バイトのビットフラグで構成されます。

| フラグ名 | ビット値 | 説明 |
| :--- | :--- | :--- |
| FLAG_FORWARD | 0x01 (1) | 前進 |
| FLAG_BACKWARD | 0x02 (2) | 後退 |
| FLAG_LEFT | 0x04 (4) | 左移動 |
| FLAG_RIGHT | 0x08 (8) | 右移動 |
| FLAG_JUMP | 0x10 (16) | ジャンプ |
| FLAG_SHIFT | 0x20 (32) | スニーク |
| FLAG_SPRINT | 0x40 (64) | スプリント |

### 2.2 EntityPositionSyncPacket (Server -> Client)
`ENTITY_TELEPORT` パケットの代わり、あるいは補完として導入されました。エンティティの正確な位置同期に使用されます。
1.21.4 では `ENTITY_TELEPORT` が正常に動作しないケースがあり、このパケットへの移行が推奨されています。

**構成要素 (予測):**
- Entity ID (VarInt)
- Position X, Y, Z (Double)
- Velocity X, Y, Z (Short/Fixed)
- Rotation Yaw, Pitch (Byte)
- On Ground (Boolean)

### 2.3 PlayerLoadedPacket (Client -> Server)
地形の読み込み（Loading Terrain）画面が終了し、プレイヤーが世界に入ったとき、またはリスポーンしたときに送信されます。

---

## 3. 変更・削除されたパケット

### 3.1 EntityTeleportPacket
1.21.4 において、一部の状況でサイレントに破損する（反映されない）問題が報告されています。前述の `EntityPositionSyncPacket` がその役割を引き継いでいます。

### 3.2 Trail Particle (`minecraft:trail`)
`duration` フィールド (VarInt) が必須となりました。
- **内容**: パーティクルが開始地点から目標地点まで飛ぶ時間（ティック数）。

---

## 4. コンポーネントおよび外部データの変更

### 4.1 Custom Model Data (`minecraft:custom_model_data`)
フィールドが拡張され、数値以外のデータも保持できるようになりました。
- `floats`: float配列
- `flags`: boolean配列
- `strings`: string配列
- `colors`: color (RGB) 配列

### 4.2 Trim Material (`minecraft:trim_material`)
レジストリから `item_model_index` フィールドが削除されました。

---

## 5. ProtocolLib での実装アドバイス

### パケットタイプの指定
ProtocolLib 5.4.0 以降では、`PacketType.Play.Server.ENTITY_POSITION_SYNC` や `PacketType.Play.Client.PLAYER_INPUT` としてアクセスできる可能性があります。

### 互換性の注意
- **ENTITY_TELEPORT の置換**: 既存のプラグインでテレポートパケットを送信している場合、1.21.4 クライアントに対しては `ENTITY_POSITION_SYNC` を送信するように条件分岐が必要です。
- **Player Input の監視**: 車両の操縦だけでなく、歩行中の入力（ジャンプやスプリント）もこのパケットで取得可能になったため、移動制御ロジックの改善が期待できます。

---
### 付録: 編集者情報
**エージェント名**: Antigravity (Google Deepmind)
**実施作業**: Minecraft 1.21.4 プロトコルリサーチおよびドキュメント作成。
**コンテキスト**: 1.21.4 でのパケット構造の変更、特に移動入力と同期に関する変更を整理。
