# Release Note Analyzer

依存ライブラリのバージョンアップ時に、リリースノートを収集・分析し、プロジェクトへの影響を特定するClaude Code用スキル。

## 機能

- 指定バージョン間の全リリースノートを収集
- Breaking changes / Deprecations / Security Fixes の抽出
- プロジェクト内で使用している機能への影響分析
- 必要な修正箇所のリストアップ（ファイル:行番号付き）
- 新機能で活用できそうなものの提案
- 優先度付きマイグレーション計画の生成

## 対応エコシステム

| 言語 | 依存ファイル | リリースノート取得元 |
|------|-------------|---------------------|
| Rust | Cargo.toml | GitHub Releases / crates.io |
| Node | package.json | GitHub Releases / npm |
| Python | requirements.txt / pyproject.toml | GitHub Releases / PyPI |
| Go | go.mod | GitHub Releases |
| Java | pom.xml / build.gradle | GitHub Releases / Maven Central |

## 使い方

### 呼び出し例

```
tokio 1.30 → 1.35にアップデートしたい
```

```
spring-boot 2.7 から 3.0 へのアップグレード影響を分析して
```

```
reactのバージョン差分を教えて（17 → 18）
```

### 出力内容

1. **変更サマリ**: Breaking Changes / Deprecations / Security Fixes / New Features
2. **影響箇所**: 修正が必要なファイルと行番号
3. **修正コード例**: Before / After形式での具体的な修正案
4. **マイグレーション計画**: 優先度付きチェックリスト

## ファイル構成

```
release-note-analyzer/
├── README.md           # 本ファイル
├── SKILL.md            # スキル定義（Claude Codeが参照）
└── templates/
    ├── summary.md      # 変更サマリのテンプレート
    └── migration.md    # マイグレーションガイドのテンプレート
```

## Java / Spring Boot 固有機能

Spring Bootプロジェクトでは以下を追加でサポート:

- BOM（Bill of Materials）のバージョン追跡
- Spring公式マイグレーションガイドの参照
- `application.yml` / `application.properties` のプロパティ変更検出

## 制限事項

- リリースノートが公開されていないライブラリは分析不可
- プライベートリポジトリのリリースノート取得には認証が必要
- AST解析は行わないため、動的な呼び出しは検出できない場合がある
