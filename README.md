# Skills-factory

Claude Code用のSkillおよびRuleを作成・管理するためのプロジェクト

## 目的

- 再利用可能なSkill/Ruleの作成
- 開発ワークフローの効率化
- ベストプラクティスの蓄積

## ディレクトリ構成

```
Skills-factory/
├── CLAUDE.md                    # プロジェクトルール
├── README.md                    # 本ファイル
├── .claude/
│   ├── settings.json            # プロジェクト設定
│   └── rules/
│       └── skill-creator.md     # Skill/Rule作成ルール
├── artifacts/                   # 成果物
│   ├── rules/                   # 単体Rule
│   ├── skills/                  # 単体Skill
│   └── bundles/                 # 用途別ツールセット
└── .github/
    └── pull_request_template.md
```

## 成果物の種類

### Rules（単体ルール）
特定のトリガーで適用されるルール。

配置: `artifacts/rules/<name>.md`

### Skills（単体スキル）
会話で呼び出されるスキル。

配置: `artifacts/skills/<name>/SKILL.md`

### Bundles（ツールセット）
用途別に複数のRule/Skillをまとめたセット。

配置: `artifacts/bundles/<bundle-name>/`

```
artifacts/bundles/<bundle-name>/
├── README.md           # バンドルの説明・使い方
├── rules/              # このバンドルに含まれるルール
│   └── <name>.md
└── skills/             # このバンドルに含まれるスキル
    └── <name>/
        └── SKILL.md
```

## 成果物一覧

### Rules
| 名前 | 説明 | トリガー |
|------|------|----------|
| commit-message | コミットメッセージ + 粒度分割 | `claude commit` |
| pr-template | PR説明生成 | `claude pr` |

### Skills
| 名前 | 説明 | 呼び出し方 |
|------|------|------------|
| code-review | コードレビュー | 「レビューして」等 |
| ask-workflow | 汎用ask | 曖昧な依頼時 |
| bug-investigation | バグ調査 | 「調査して」等 |
| sql-optimization | SQLクエリ最適化 | 「最適化して」等 |

### Bundles
（今後追加予定）

## 使い方

### Skill/Ruleの作成

1. `/skill-creator` コマンドを実行（Skillの場合）
2. 生成されたファイルを `artifacts/` 配下に配置

詳細は `.claude/rules/skill-creator.md` を参照。

### 成果物の利用

作成したSkill/Ruleを他のプロジェクトで使用する場合：

1. 必要なファイルを対象プロジェクトの `.claude/` にコピー
2. または、bundleごとコピーして利用

## 開発ルール

- コミット: Conventional Commits形式を推奨
- ブランチ: mainへの直接プッシュ可（設定による）
- Skill作成時は `/skill-creator` コマンドを使用

## ライセンス

MIT
