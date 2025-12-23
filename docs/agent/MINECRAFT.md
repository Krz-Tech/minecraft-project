# Minecraft Development Agent Prompt
# Minecraft開発エージェント 補足プロンプト

> [!NOTE]
> このプロンプトは`Agent/GEMINI.md`と併用してください。
> Minecraftサーバー開発タスク時に追加で参照されます。

---

## 対象範囲 | Scope

このプロンプトは以下のタスクに適用されます：
- Minecraftサーバーの設定・構築
- プラグイン開発（Java、Kotlin）
- Skriptスクリプト開発
- サーバー運用・最適化

---

## サーバーソフトウェア | Server Software

### 推奨サーバー

| ソフトウェア | 用途 | 備考 |
|-------------|------|------|
| **PaperMC** | メイン本番サーバー | パフォーマンス最適化済み |
| **Purpur** | 高カスタマイズ環境 | Paper互換、追加機能あり |
| **Spigot** | 互換性重視 | プラグイン互換性が最も高い |

### プロキシサーバー

| ソフトウェア | 用途 | 備考 |
|-------------|------|------|
| **Velocity** | 推奨プロキシ | モダン、高性能 |
| **BungeeCord** | 従来プロキシ | 広い互換性 |

### バージョン方針
- 最新の安定版を使用
- 開発時はテストサーバーで動作確認を行うこと

---

## プラグイン開発 | Plugin Development

### 言語選択

| 言語 | 推奨用途 | 理由 |
|------|----------|------|
| **Skript** | 簡易機能、プロトタイプ | 動的開発可能、再起動不要 |
| **Kotlin** | 中〜大規模プラグイン | モダン構文、Null安全性 |
| **Java** | 互換性重視、既存改修 | 広い情報源、高互換性 |

### 開発環境
- **IDE**: IntelliJ IDEA推奨（Kotlin/Java）
- **ビルドツール**: Gradle（Kotlin DSL推奨）
- **テスト**: 開発用Dockerコンテナを使用

### API参照

| API | URL | 用途 |
|-----|-----|------|
| Paper API | https://jd.papermc.io/paper/ | Paper固有機能 |
| Spigot API | https://hub.spigotmc.org/javadocs/spigot/ | 基本API |
| Bukkit API | https://hub.spigotmc.org/javadocs/bukkit/ | レガシー互換 |

---

## ゲームシステム参照 | Game System Reference

開発時は必ず以下のドキュメントを参照してください：

| ドキュメント | 内容 |
|-------------|------|
| `Docs/GameSystem/0.GameSystem_General.md` | 全体概要 |
| `Docs/GameSystem/1.GameSystem_BattleStyle.md` | 戦闘システム |
| `Docs/GameSystem/1.GameSystem_LifeStyle.md` | 生活システム |
| `Docs/GameSystem/2.GameSystem_PlayGround.md` | プレイグラウンド |

---

## コーディング規約 | Coding Standards

### Java/Kotlin共通

```java
// パッケージ命名
tech.krz.minecraft.<機能名>

// クラス命名: PascalCase
public class DamageCalculator { }

// メソッド命名: camelCase
public void calculateDamage() { }

// 定数命名: SCREAMING_SNAKE_CASE
public static final int MAX_HEALTH = 20;
```

### コメント規則

```java
/**
 * ダメージ計算を行う
 * 
 * @param attacker 攻撃者
 * @param victim 被攻撃者
 * @return 最終ダメージ値
 */
public double calculateDamage(Player attacker, Entity victim) {
    // クリティカル判定を行う
    boolean isCritical = checkCritical(attacker);
    
    // 基本ダメージを取得
    double baseDamage = getBaseDamage(attacker);
    
    return isCritical ? baseDamage * 1.5 : baseDamage;
}
```

---

## テスト環境 | Testing Environment

### ローカルテスト

```bash
# Docker Composeでテストサーバー起動
docker-compose up -d minecraft-test

# ログ確認
docker-compose logs -f minecraft-test

# サーバー停止
docker-compose down
```

### テスト項目チェックリスト
- [ ] プラグイン/スクリプトがエラーなしでロードされる
- [ ] 想定した機能が正常に動作する
- [ ] 他のプラグインとの競合がない
- [ ] サーバーパフォーマンスに悪影響がない

---

## 参考リソース | Resources

- [PaperMC Documentation](https://docs.papermc.io/)
- [SpigotMC Resources](https://www.spigotmc.org/resources/)
- [Minecraft Wiki](https://minecraft.wiki/)

> [!TIP]
> 最新情報は`Docs/WebDocsList.md`のSKRIPT, MINECRAFT関連エントリを参照してください。

---

## 更新履歴 | Changelog

| バージョン | 日付 | 変更内容 |
|-----------|------|----------|
| 1.0.0 | 2025-12-23 | 初版作成 |
