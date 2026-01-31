# マイグレーションガイドテンプレート

## 概要

| 項目 | 内容 |
|------|------|
| ライブラリ | {{library_name}} |
| 現在バージョン | {{current_version}} |
| 目標バージョン | {{target_version}} |
| 影響ファイル数 | {{affected_file_count}} |
| 推定修正箇所 | {{estimated_changes}} |

---

## マイグレーション計画

### Phase 1: 必須対応（Breaking Changes）

#### 1.1 {{change_title}}

**変更内容**:
{{change_description}}

**影響ファイル**:
- `{{file_path}}:{{line_number}}` - {{context}}

**修正方針**:
```{{language}}
// Before
{{before_code}}

// After
{{after_code}}
```

**テスト確認**:
- [ ] ユニットテスト
- [ ] 統合テスト
- [ ] 動作確認

---

### Phase 2: 推奨対応（Deprecations）

#### 2.1 {{deprecation_title}}

**非推奨API**: `{{deprecated_api}}`
**代替API**: `{{replacement_api}}`
**削除予定**: {{removal_version}}

**影響ファイル**:
- `{{file_path}}:{{line_number}}`

**修正方針**:
```{{language}}
// Before
{{before_code}}

// After
{{after_code}}
```

---

### Phase 3: セキュリティ対応

#### 3.1 {{security_issue}}

**CVE**: {{cve_id}}
**深刻度**: {{severity}}
**修正バージョン**: {{fixed_version}}

**対応**:
- [ ] バージョンアップで自動修正
- [ ] 追加の設定変更が必要: {{config_change}}

---

### Phase 4: 任意対応（新機能活用）

#### 4.1 {{feature_title}}

**新機能**: {{feature_description}}
**活用メリット**: {{benefit}}

**現在の実装**:
```{{language}}
{{current_implementation}}
```

**新機能を使った実装**:
```{{language}}
{{new_implementation}}
```

---

## チェックリスト

### 事前確認
- [ ] 依存関係の整合性確認
- [ ] CI/CDパイプラインの動作確認
- [ ] ロールバック手順の準備

### 実施
- [ ] Phase 1: Breaking Changes対応
- [ ] Phase 2: Deprecations対応
- [ ] Phase 3: セキュリティ対応
- [ ] Phase 4: 新機能活用（任意）

### 事後確認
- [ ] 全テストがパス
- [ ] ステージング環境での動作確認
- [ ] パフォーマンス劣化がないこと
- [ ] ログ・モニタリングの確認

---

## 依存関係の影響

### 推移的依存関係の変更

| 依存ライブラリ | 変更前 | 変更後 | 影響 |
|---------------|--------|--------|------|
| {{dep_name}} | {{old_version}} | {{new_version}} | {{impact}} |

### 競合の可能性

| ライブラリA | ライブラリB | 競合内容 | 解決方法 |
|------------|------------|----------|----------|
| {{lib_a}} | {{lib_b}} | {{conflict}} | {{resolution}} |

---

## ロールバック手順

### 依存ファイルの復元
```bash
# Git で復元
git checkout {{commit_hash}} -- {{dependency_file}}
```

### 確認事項
- [ ] アプリケーションが正常に起動すること
- [ ] 主要機能が動作すること
- [ ] エラーログが出ていないこと

---

## 参考リンク

- 公式マイグレーションガイド: {{official_guide_url}}
- リリースノート: {{release_notes_url}}
- Issue/Discussion: {{issue_url}}
