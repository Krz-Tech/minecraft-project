---
作成日時: 2025-12-24
更新日時: 2025-12-24
作成者: Antigravity (AIエージェント)
---

# リソースパック設計仕様書 (Resource Pack Architecture)

## 1. コンセプト (Concept)
- **Vanilla-First**: 外部MOD (Optifine, Iris等) を前提とせず、1.21+ の標準パケット・リソースパック機能のみで完結。
- **Modular & Collision-Free**: 名前空間 (`krz:`) を徹底し、バニラや他配布物との競合を防止。
- **Maintainability**: ID・パス・CMD（CustomModelData）の体系化により、複数人・複数AIでの開発を容易にする。

---

## 2. 推奨技術スタック (Technical Stack)

| 技術要素 | 用途 | 備考 |
| :--- | :--- | :--- |
| **CustomModelData (CMD)** | 武器・ツール・UIアイコンの定義 | 10001〜 形式のID体系で管理。 |
| **Font Provider** | UI/HUDのアイコン、負のスペース (Negative Space) | `assets/minecraft/font/default.json` で定義。 |
| **Atlases** | カスタムテクスチャの認識・最適化 | `assets/minecraft/atlases/gui.json` 等で管理。 |
| **Core Shaders** | 透過HUD、発光エフェクト、画面効果 | Vanilla組み込みシェーダー。外部MOD不要。 |
| **Negative Space Font** | チャット・タイトル・アクションバーでのレイアウト調整 | `\uF801` 〜 `\uF80A` 等の余白用グリフ。 |

---

## 3. ディレクトリ構成 (Folder Structure)

`assets/` 以下の構成案です。独自の `krz` 名前空間と、バニラ機能を呼び出す `minecraft` を分離します。

```text
resourcepack/
├── pack.mcmeta             # バージョン情報 (1.21=34+)
├── pack.png                # パックアイコン
└── assets/
    ├── minecraft/
    │   ├── atlases/        # バニラに認識させるテクスチャのリスト
    │   │   └── gui.json    # カスタムUIテクスチャを統合
    │   ├── font/
    │   │   └── default.json # HUDアイコン、ネガティブスペースのプロバイダ集約
    │   ├── models/
    │   │   └── item/       # バニラアイテムへのCMD紐付け (json)
    │   └── shaders/
    │       └── core/       # Vanilla Core Shaders (特殊演出用)
    └── krz/                # プロジェクト専用名前空間 (最重要)
        ├── font/           # krz専用フォント定義
        ├── models/
        │   ├── ui/         # UI部品としてのモデル
        │   ├── weapon/     # 各武器カテゴリ別のモデル定義
        │   └── equipment/  # 防具・Tech Suitのモデル
        └── textures/
            ├── ui/         # HUD、ゲージ、アイコンのPNG
            ├── item/       # 武器・ツールのPNG
            └── effect/     # パーティクル、シェーダー用テクスチャ
```

---

## 4. 命名・ID体系 (Naming & ID Standards)

### CustomModelData (CMD)
`1XXYY` 形式の5桁（またはそれ以上）を推奨します。
- `1`: カテゴリ (1=武器, 2=道具, 3=UI, 4=装備)
- `XX`: サブカテゴリ (01=物理ブレード, 02=ENブレード ...)
- `YY`: 個別ID

### フォント・ネガティブスペース
- `\uE000` 〜 `\uF7FF`: ユーザー定義領域（Private Use Area）を使用し、アイコンを登録。
- `assets/krz/textures/ui/negative_space.png`: ドット単位での空白（-1px, -2px...）を定義し、UIの微調整に使用。

---

## 5. 将来の機能拡張への想定 (Future-Proofing)

1.  **動的HUD (HappyHUD/MythicHUD互換)**: 
    - `atlases/gui.json` にパスを通しておくことで、将来的にこれらのプラグインを導入してもそのままテクスチャを参照可能。
2.  **3D装備 (Tech Suit)**:
    - 1.21.2+ で導入された `equipment` コンポーネントへの対応を想定し、防具のテクスチャも `krz` 名前空間で管理。
3.  **シネマティック演出**:
    - Core Shaders を使用して、画面全体へのノイズ、色収差、デジタルノイズ等のエフェクトを追加可能にする。

---
### 付録: 編集者情報
**エージェント名**: Antigravity (Google Deepmind)
**実施作業**: 1.21+ Vanilla準拠のリソースパック設計・ディレクトリ構成の提案。
**コンテキスト**: MOD非依存・保守性重視の「Tech」世界観を表現する基盤構築。
