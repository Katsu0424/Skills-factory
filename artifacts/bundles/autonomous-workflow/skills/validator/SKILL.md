# validator

検証者スキル - 実装されたコードをテスト・lint・ビルドで検証する

## トリガー

- `conductor` から検証指示を受けた時
- `implementer` 完了後
- `/validator` コマンド実行時

## 目的

実装されたコードが品質基準を満たしているかを客観的に検証する

---

## 入力

- 実装完了報告（変更ファイル一覧）
- タスク定義（Done when）

## 出力

- 検証結果レポート（成功/失敗）
- （失敗時）失敗詳細と修正ヒント

---

## 検証項目

### 1. 構文チェック

- TypeScript/JavaScript: 型エラーなし
- その他言語: コンパイルエラーなし

### 2. Lint

- ESLint / Prettier エラーなし
- プロジェクト固有のルール遵守

### 3. テスト

- 既存テストがすべてパス
- （新規テストがある場合）新規テストもパス
- カバレッジ基準を満たす（設定されている場合）

### 4. ビルド

- ビルドが成功する
- ワーニングが増加していない

---

## 処理フロー

### 1. 変更ファイルの特定

実装報告から変更されたファイルを確認

### 2. 検証コマンドの実行

```bash
# 型チェック
npm run type-check

# Lint
npm run lint

# テスト
npm run test

# ビルド
npm run build
```

### 3. 結果の収集

各検証の結果を収集・整理

### 4. 判定

すべて成功 → 成功
1つでも失敗 → 失敗

---

## 出力テンプレート

### 成功時

```markdown
## 検証結果: 成功 ✓

### サマリー

| 項目 | 結果 |
|------|------|
| 型チェック | ✓ パス |
| Lint | ✓ パス |
| テスト | ✓ 12/12 パス |
| ビルド | ✓ 成功 |

### 次のステップ

reviewer によるレビューへ
```

### 失敗時

```markdown
## 検証結果: 失敗 ✗

### サマリー

| 項目 | 結果 |
|------|------|
| 型チェック | ✓ パス |
| Lint | ✗ 2エラー |
| テスト | ✓ パス |
| ビルド | - スキップ |

### 失敗詳細

#### Lint エラー

```
src/api/auth.ts:15:3
  error  Unexpected console statement  no-console

src/api/auth.ts:42:10
  error  'result' is defined but never used  @typescript-eslint/no-unused-vars
```

### 修正ヒント

1. console.log を削除または logger に置き換え
2. 未使用変数 `result` を削除または使用

### 次のステップ

implementer による修正へ
```

---

## Done when の確認

タスクの完了条件を確認：

```markdown
### Done when チェック

タスク完了条件: 各エンドポイントが正常動作

- [x] /login が200を返す
- [x] /logout が200を返す
- [x] /refresh が新トークンを返す
```

---

## 検証コマンドの検出

プロジェクトの package.json や設定ファイルから適切なコマンドを検出：

```json
{
  "scripts": {
    "type-check": "tsc --noEmit",
    "lint": "eslint src/",
    "test": "jest",
    "build": "tsc"
  }
}
```

コマンドが存在しない項目はスキップ。

---

## 制約

- 検証は自動化されたツールに依存
- 手動テストが必要な場合は reviewer に委譲
- 検証に時間がかかる場合はタイムアウト設定を考慮

---

## 次のスキル

- 成功 → `conductor` へ報告 → `reviewer` へ
- 失敗 → `conductor` へ報告 → `implementer` へ
