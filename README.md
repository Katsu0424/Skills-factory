# Skills-factory

Claude Code用のSkill / Rule / Agent / Bundleを作成・管理するリポジトリ。

## Skills

| 名前 | 説明 | 呼び出し方 |
|------|------|------------|
| [code-review](./artifacts/skills/code-review/) | コードレビュー | 「レビューして」等 |
| [bug-investigation](./artifacts/skills/bug-investigation/) | バグ調査・原因特定 | 「調査して」「原因は？」等 |
| [sql-optimization](./artifacts/skills/sql-optimization/) | SQLクエリ最適化 | 「最適化して」等 |
| [ask-workflow](./artifacts/skills/ask-workflow/) | 曖昧な依頼の明確化 | 曖昧な依頼時に自動 |
| [release-note-analyzer](./artifacts/skills/release-note-analyzer/) | 依存ライブラリのアップデート影響分析 | 「アップデートしたい」「差分を教えて」等 |

## Bundles

| 名前 | 説明 |
|------|------|
| [autonomous-workflow](./artifacts/bundles/autonomous-workflow/) | 自律エージェントワークフロー（Plan & Dig → Do Phase） |

## ディレクトリ構成

```
artifacts/
├── rules/      # 単体Rule
├── skills/     # 単体Skill
├── agents/     # 単体Agent
└── bundles/    # 用途別ツールセット
```

## 使い方

作成したSkill/Ruleを他プロジェクトで使用する場合、対象プロジェクトの `.claude/` にコピー。

## ライセンス

MIT
