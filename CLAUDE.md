# CLAUDE.md

このファイルはClaude Codeがプロジェクトを理解するためのガイドです。

## プロジェクト概要

**Skills-factory** - Claude Code用のSkillおよびRuleを作成・管理するためのプロジェクト

### 目的
- 再利用可能なSkill/Ruleの作成
- 開発ワークフローの効率化
- ベストプラクティスの蓄積

---

## ディレクトリ構成

```
Skills-factory/
├── CLAUDE.md                    # 本ファイル（プロジェクトルール）
├── README.md                    # プロジェクト説明
├── .claude/
│   ├── settings.json            # プロジェクト設定
│   └── rules/
│       └── skill-creator.md     # Skill/Rule作成用ルール ★重要
├── artifacts/                   # 成果物
│   ├── rules/                   # 単体Rule
│   ├── skills/                  # 単体Skill
│   └── bundles/                 # 用途別ツールセット
└── .github/
    └── pull_request_template.md
```

---

## 開発ルール

### 1. Skill/Rule作成時
**Skill作成時は `/skill-creator` コマンドを使用すること**

- 成果物は必ず `artifacts/` に配置
- Ruleは `artifacts/rules/<name>.md`
- Skillは `artifacts/skills/<name>/SKILL.md`
- Bundleは `artifacts/bundles/<bundle-name>/`

### 2. コミット
- コミットメッセージは明確に記述する
- Conventional Commits形式を推奨

### 3. ブランチ運用
- mainブランチへの直接プッシュは禁止（権限で制限）
- 必ずfeatureブランチからPRを作成してマージする

### 4. PR作成・マージ手順
```bash
# PRを作成
gh pr create --base main --title "タイトル" --body "説明"

# PRをマージ
gh pr merge --merge
```

---

## クイックスタート

### 新しいRuleを作成する場合
```
1. .claude/rules/skill-creator.md を読む
2. artifacts/rules/<rule-name>.md を作成
3. テンプレートに従って内容を記述
```

### 新しいSkillを作成する場合
```
1. .claude/rules/skill-creator.md を読む
2. artifacts/skills/<skill-name>/ ディレクトリを作成
3. artifacts/skills/<skill-name>/SKILL.md を作成
4. テンプレートに従って内容を記述
```

---

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
