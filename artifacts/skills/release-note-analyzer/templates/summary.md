# 変更サマリテンプレート

## 対象
- **ライブラリ**: {{library_name}}
- **バージョン**: {{current_version}} → {{target_version}}
- **リリース数**: {{release_count}}件
- **期間**: {{date_range}}

---

## Breaking Changes 🔴

| 変更内容 | 影響度 | 該当バージョン | 備考 |
|----------|--------|---------------|------|
| {{change_description}} | {{severity}} | {{version}} | {{note}} |

---

## Deprecations 🟡

| 変更内容 | 代替API | 該当バージョン | 削除予定 |
|----------|---------|---------------|----------|
| {{deprecated_api}} | {{replacement_api}} | {{version}} | {{removal_version}} |

---

## Security Fixes 🔒

| CVE/Issue | 深刻度 | 該当バージョン | 説明 |
|-----------|--------|---------------|------|
| {{cve_id}} | {{severity}} | {{version}} | {{description}} |

---

## Bug Fixes 🐛

| 修正内容 | 該当バージョン | Issue |
|----------|---------------|-------|
| {{fix_description}} | {{version}} | {{issue_link}} |

---

## New Features ✨

| 機能 | 説明 | 該当バージョン | 活用可能性 |
|------|------|---------------|-----------|
| {{feature_name}} | {{description}} | {{version}} | {{applicability}} |

---

## Performance Improvements ⚡

| 改善内容 | ベンチマーク | 該当バージョン |
|----------|-------------|---------------|
| {{improvement}} | {{benchmark}} | {{version}} |

---

## 参照リンク

- リリースノート: {{release_notes_url}}
- マイグレーションガイド: {{migration_guide_url}}
- CHANGELOG: {{changelog_url}}
