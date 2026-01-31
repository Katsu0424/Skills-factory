---
name: release-note-analyzer
description: |
  依存ライブラリのバージョンアップ時に、リリースノートを収集・分析し、プロジェクトへの影響を特定する。
  「〇〇をアップデートしたい」「バージョン差分を教えて」「アップグレードの影響を分析して」などと言われた時に使用。
  Breaking Changes、Deprecations、Security Fixesを抽出し、影響箇所とマイグレーション計画を提示。
allowed-tools: Read, Grep, Glob, WebFetch, Bash(gh api *), Bash(gh release *)
---

# リリースノート検証スキル

## 呼び出し方
会話で「〇〇をアップデートしたい」「バージョン差分を教えて」「アップグレードの影響を分析して」等

## 目的
依存ライブラリのバージョンアップ時に、リリースノートを収集・分析し、プロジェクトへの影響を特定する

---

## フロー

```
依存関係検出 → リリースノート取得 → 変更分類 → 影響箇所特定 → レポート生成
```

---

## 入力

| 項目 | 必須 | 説明 |
|------|------|------|
| ライブラリ名 | ○ | GitHubリポジトリURL、パッケージ名など |
| 現在バージョン | △ | 省略時は依存ファイルから自動検出 |
| 目標バージョン | ○ | アップグレード先のバージョン |

---

## 処理ステップ

### 1. 依存関係ファイルの検出と解析

対応する依存ファイルを自動検出し、現行バージョンを取得する。

| 言語 | 依存ファイル | 検出パターン |
|------|-------------|-------------|
| Rust | Cargo.toml, Cargo.lock | `<package> = "x.y.z"` |
| Node | package.json, package-lock.json | `"<package>": "^x.y.z"` |
| Python | requirements.txt, pyproject.toml, Pipfile | `<package>==x.y.z` |
| Go | go.mod, go.sum | `<package> vx.y.z` |
| Java | pom.xml, build.gradle | `<version>x.y.z</version>` |

### 2. リリースノートの取得

以下の優先順位で取得を試行する：

1. **GitHub Releases API** (`gh api repos/{owner}/{repo}/releases`)
2. **CHANGELOG.md / HISTORY.md** （リポジトリ内）
3. **パッケージレジストリ**
   - crates.io (Rust)
   - npm (Node)
   - PyPI (Python)
   - pkg.go.dev (Go)
   - Maven Central (Java)
4. **公式マイグレーションガイド**（Spring Boot等）

### 3. 変更の分類

取得したリリースノートを以下のカテゴリに分類する：

| カテゴリ | アイコン | 対応必須度 |
|----------|----------|-----------|
| Breaking Changes | 🔴 | 必須 |
| Deprecations | 🟡 | 推奨 |
| Security Fixes | 🔒 | 必須 |
| Bug Fixes | 🐛 | 情報 |
| New Features | ✨ | 任意 |
| Performance | ⚡ | 情報 |

### 4. プロジェクト影響分析

変更内容に基づき、プロジェクト内の影響箇所を特定する：

- **API変更**: `grep` で該当関数・メソッドの使用箇所を検索
- **型変更**: インポート文、型定義の使用箇所を検索
- **設定変更**: 設定ファイル内のプロパティ名を検索
- **振る舞い変更**: テストコードを中心に影響範囲を推定

### 5. マイグレーション計画の生成

影響度と修正量に基づき、優先度付きのマイグレーション計画を出力する。

---

## 出力形式

```markdown
## 変更サマリ

### 対象
- **ライブラリ**: tokio
- **バージョン**: 1.30.0 → 1.35.0
- **リリース数**: 5件

### Breaking Changes 🔴
| 変更内容 | 影響度 | 該当バージョン |
|----------|--------|---------------|
| `runtime::Handle::block_on` が削除 | 高 | 1.32.0 |

### Deprecations 🟡
| 変更内容 | 代替API | 該当バージョン |
|----------|---------|---------------|
| `time::delay_for` | `time::sleep` | 1.31.0 |

### Security Fixes 🔒
| CVE | 深刻度 | 該当バージョン |
|-----|--------|---------------|
| CVE-2024-XXXX | High | 1.33.0 |

### New Features ✨
| 機能 | 活用可能性 | 該当バージョン |
|------|-----------|---------------|
| `sync::watch` の改善 | ○ | 1.34.0 |

---

## 影響箇所

### 🔴 要修正（Breaking Changes）

#### `runtime::Handle::block_on` の削除

**影響ファイル**:
- `src/main.rs:42` - `handle.block_on(async_task())`
- `src/service.rs:78` - `handle.block_on(db.query())`

**修正方針**:
```rust
// Before
handle.block_on(async_task());

// After
tokio::runtime::Runtime::new().unwrap().block_on(async_task());
// または async fn 内で直接 await
```

### 🟡 要確認（Deprecations）

#### `time::delay_for` → `time::sleep`

**影響ファイル**:
- `src/utils/retry.rs:15`

**修正方針**:
```rust
// Before
tokio::time::delay_for(Duration::from_secs(1)).await;

// After
tokio::time::sleep(Duration::from_secs(1)).await;
```

---

## マイグレーション計画

### 優先度: 高
1. [ ] `runtime::Handle::block_on` の置き換え（2箇所）
2. [ ] セキュリティ修正の確認

### 優先度: 中
3. [ ] `delay_for` → `sleep` の置き換え（1箇所）

### 優先度: 低（任意）
4. [ ] `sync::watch` の新機能活用を検討

---

## 活用できそうな新機能

| 機能 | 説明 | 現在の実装との比較 |
|------|------|-------------------|
| `sync::watch` 改善 | 初期値なしで作成可能に | 現在の初期値ダミー設定が不要に |
```

---

## Java / Spring Boot 固有の考慮事項

Spring Bootプロジェクトでは以下を追加で確認する：

### BOM（Bill of Materials）の追跡
- `spring-boot-dependencies` のバージョン管理
- 推移的依存関係の変更

### Spring公式リソース
- [Spring Blog](https://spring.io/blog) のマイグレーションガイド
- [Spring Boot Release Notes](https://github.com/spring-projects/spring-boot/wiki)

### 設定プロパティの変更検出
```bash
# application.yml / application.properties 内のプロパティ変更を検索
grep -r "spring\." --include="*.yml" --include="*.properties"
```

### 検出対象
- プロパティ名の変更（例: `server.servlet.context-path` → `server.servlet.context-path`）
- デフォルト値の変更
- 新しい必須設定

---

## 情報取得コマンド例

### GitHub Releases
```bash
gh api repos/{owner}/{repo}/releases --jq '.[].tag_name'
gh release view v1.35.0 --repo {owner}/{repo}
```

### crates.io (Rust)
```bash
cargo search {package} --limit 1
```

### npm (Node)
```bash
npm view {package} versions --json
npm view {package}@1.35.0 --json
```

### PyPI (Python)
```bash
pip index versions {package}
```

---

## 情報が不足している場合

以下を追加で依頼する：

1. 対象ライブラリのGitHubリポジトリURL
2. 目標バージョン（latest以外の場合）
3. マイグレーションの制約（段階的に行うか一括か）
4. 特に気にしている変更点や機能

---

## 注意事項

- リリースノートが不完全な場合、CHANGELOGやコミットログも参照する
- 推移的依存関係の変更も可能な限り追跡する
- 複数ライブラリの同時アップデート時は依存関係の整合性を確認する
