# Skill/Rule作成ルール

## 概要
このルールは、Claude Code用のSkill、Rule、Agentを作成する際に適用される。

**重要: Skill作成時は `/skill-creator` コマンドを使用すること**

---

## 種類の判定

作成したいものがどの種類に該当するか、以下のフローで判定する。

```
目的を達成したい
    │
    ├─ 特定のコマンド/タイミングで自動適用させたい？
    │   └─ Yes → Rule
    │
    ├─ 会話で呼び出して単一のタスクを実行したい？
    │   └─ Yes → Skill
    │
    ├─ 複数ステップを自律的に実行させたい？
    │   └─ Yes → Agent
    │
    └─ 複数のRule/Skill/Agentをセットで使いたい？
        └─ Yes → Bundle
```

### 判定基準

| 種類 | 特徴 | 例 |
|------|------|-----|
| **Rule** | 特定トリガーで自動適用、制約・規約を定義 | コミットメッセージ形式、コーディング規約 |
| **Skill** | 会話で呼び出し、単一タスクを実行 | コードレビュー、バグ調査、SQL最適化 |
| **Agent** | 複数ステップを自律実行、判断を伴う | テスト実行→修正→再実行、デプロイ自動化 |
| **Bundle** | 関連する複数ツールをまとめたセット | 「開発ワークフロー」「レビュープロセス」 |

### 具体例

| やりたいこと | 種類 | 理由 |
|--------------|------|------|
| コミット時にメッセージ形式を強制 | Rule | 特定タイミングで自動適用 |
| 「レビューして」でコードチェック | Skill | 会話で呼び出し、単一タスク |
| テスト失敗→原因特定→修正を自動で | Agent | 複数ステップを自律実行 |
| PR作成に必要なツール一式 | Bundle | Rule+Skillの組み合わせ |

---

## 作成方法

### Skillを作成する場合
1. `/skill-creator` コマンドを実行
2. 対話形式でSkillを定義
3. 生成されたファイルを `artifacts/skills/<skill-name>/` に配置

### Ruleを作成する場合
1. `artifacts/rules/<rule-name>.md` を作成
2. 以下のテンプレートに従って記述

---

## 成果物の配置

すべての成果物は `artifacts/` ディレクトリに配置する。

```
artifacts/
├── rules/           # 単体Rule
│   └── <name>.md
├── skills/          # 単体Skill
│   └── <name>/
│       └── SKILL.md
├── agents/          # Agent
│   └── <name>/
│       └── AGENT.md
└── bundles/         # 用途別ツールセット
    └── <bundle-name>/
        ├── README.md
        ├── rules/
        ├── skills/
        └── agents/
```

### Bundle（ツールセット）
複数のRule/Skillを用途別にまとめる場合に使用。

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

---

## Agentを作成する場合
1. `artifacts/agents/<agent-name>/` ディレクトリを作成
2. `AGENT.md` にAgent定義を記述

---

## Ruleのテンプレート

```markdown
# <Rule名>

## トリガー
（いつこのルールが適用されるか）

## 目的
（このルールが達成すること）

---

## 詳細
（ルールの詳細な定義）

## 適用手順
（実行時の具体的なステップ）
```

---

## Agentのテンプレート

```markdown
# <Agent名>

## 概要
（このAgentの目的と役割）

## 使用するツール
（このAgentが使用するツール一覧）

## 動作フロー
（処理の流れ）

## 設定
（必要な設定・環境変数等）

## 使用例
（実行例）
```

---

## 命名規則

- Rule: `<動詞>-<対象>.md` または `<対象>-<目的>.md`
  - 例: `commit-message.md`, `pr-template.md`

- Skill: `<対象>-<動作>/` または `<目的>/`
  - 例: `code-review/`, `bug-investigation/`, `sql-optimization/`

- Agent: `<役割>-agent/` または `<目的>-agent/`
  - 例: `test-runner-agent/`, `deploy-agent/`

---

## 品質基準

1. **明確性**: 曖昧な表現を避け、具体的に記述
2. **完全性**: 必要な情報がすべて含まれている
3. **実用性**: 実際のワークフローで使える
4. **一貫性**: 他のSkill/Ruleと形式が統一されている
