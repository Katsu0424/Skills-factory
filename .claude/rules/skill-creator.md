# Skill/Rule作成ルール

## 概要
このルールは、Claude Code用のSkill、Rule、Agentを作成する際に適用される。

**重要: Skill作成時は必ずYAML frontmatterを含めること**

---

## 種類の判定

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

| 種類 | 特徴 | 例 |
|------|------|-----|
| **Rule** | 特定トリガーで自動適用 | コミットメッセージ形式、コーディング規約 |
| **Skill** | 会話で呼び出し、単一タスク実行 | コードレビュー、バグ調査、SQL最適化 |
| **Agent** | 複数ステップを自律実行 | テスト実行→修正→再実行 |
| **Bundle** | 複数ツールのセット | 開発ワークフロー |

---

## Skillの正式フォーマット（必須）

### YAML Frontmatter

全てのSKILL.mdは以下のYAML frontmatterで始めること：

```yaml
---
name: skill-name
description: |
  このスキルの説明。Claudeが自動読み込み時に参照する。
  いつ使うべきかを明記（例：「レビューして」と言われた時）。
allowed-tools: Read, Grep, Glob
---
```

### 必須フィールド

| フィールド | 必須 | 説明 |
|-----------|------|------|
| `name` | ○ | スキル名（小文字、ハイフン区切り、最大64文字） |
| `description` | ○ | スキルの説明（最大1024文字）。Claudeが自動実行の判断に使用 |

### 任意フィールド

| フィールド | 型 | 説明 |
|-----------|-----|------|
| `allowed-tools` | string | 許可なく使用できるツール（例：`Read, Grep, Bash(git *)`） |
| `disable-model-invocation` | boolean | `true`: `/skill-name`でのみ実行（Claudeが自動実行しない） |
| `user-invocable` | boolean | `false`: Claudeのみ実行可（ユーザーが`/`で呼び出せない） |
| `context` | string | `fork`: 隔離されたサブエージェントで実行 |
| `agent` | string | `context: fork`時のエージェント型（`Explore`, `Plan`等） |
| `model` | string | このSkill実行時に使用するモデル |
| `argument-hint` | string | `/`メニューに表示されるヒント（例：`[issue-number]`） |

### allowed-toolsの書き方

**基本ツール（ほぼ全てのSkillで必要）:**
- `Read, Write, Edit` - ファイル読み書き・編集
- `Grep, Glob` - 検索
- `AskUserQuestion` - ユーザーへの質問（対話型Skillでは必須）

**タスク管理が必要な場合:**
- `TodoRead, TodoWrite` - TODOリスト管理

**外部連携が必要な場合:**
- `WebFetch` - Web取得
- `Bash(git *)` - Gitコマンド
- `Bash(gh *)` - GitHub CLI

```yaml
# 基本的なSkill（対話・ファイル操作）
allowed-tools: Read, Write, Edit, Grep, Glob, AskUserQuestion

# タスク管理を含むSkill
allowed-tools: Read, Write, Edit, Grep, Glob, TodoRead, TodoWrite, AskUserQuestion

# 外部リソースを参照するSkill
allowed-tools: Read, Write, Edit, Grep, Glob, WebFetch, Bash(gh api *), AskUserQuestion

# Bashは引数パターンで制限可能
allowed-tools: Bash(npm test), Bash(git diff), Bash(git status)
```

### context / agent の設定

大量の情報を探索・分析するSkillでは、隔離されたサブエージェントで実行すると効率的：

```yaml
context: fork
agent: Explore
```

| agent | 用途 |
|-------|------|
| `Explore` | コードベース探索、情報収集 |
| `Plan` | 実装計画の策定 |

---

## Skillテンプレート

```yaml
---
name: my-skill
description: |
  このスキルの目的と機能の説明。
  「〇〇して」「△△を教えて」などと言われた時に使用。
allowed-tools: Read, Write, Edit, Grep, Glob, AskUserQuestion
---

# スキル名

## 目的
（このスキルが達成すること）

---

## 処理フロー
（実行時のステップ）

## 出力形式
（結果のフォーマット）
```

---

## 動的コンテンツ機能

### 変数置換

| 変数 | 説明 |
|-----|------|
| `$ARGUMENTS` | 全ての引数 |
| `$0`, `$1`, `$2` | 第1, 第2, 第3引数 |

### コマンド実行結果の埋め込み

```markdown
## 現在の状態
!`git status`

## 差分
!`git diff`
```

`` !`command` `` 構文でコマンド実行結果をコンテキストに挿入可能。

---

## 成果物の配置

```
artifacts/
├── rules/           # 単体Rule
│   └── <name>.md
├── skills/          # 単体Skill
│   └── <name>/
│       ├── SKILL.md      # 必須（frontmatter含む）
│       ├── README.md     # 任意（使い方説明）
│       └── templates/    # 任意（出力テンプレート等）
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

- **Skill**: `<対象>-<動作>` または `<目的>`
  - 例: `code-review`, `bug-investigation`, `sql-optimization`
- **Rule**: `<動詞>-<対象>` または `<対象>-<目的>`
  - 例: `commit-message`, `pr-template`
- **Agent**: `<役割>-agent` または `<目的>-agent`
  - 例: `test-runner-agent`, `deploy-agent`

---

## 品質チェックリスト

- [ ] YAML frontmatterが存在する
- [ ] `name`フィールドが正しい形式（小文字、ハイフン区切り）
- [ ] `description`がいつ使うべきかを明記している
- [ ] 必要な`allowed-tools`が設定されている
- [ ] 処理フローが明確
- [ ] 出力形式が定義されている
