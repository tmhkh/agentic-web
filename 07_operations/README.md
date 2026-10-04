---
status: Draft
owner: {{担当者名}}
reviewers: [{{レビュアー}}]
last_updated: YYYY-MM-DD
related: [../README.md, ../01_requirements/04_non-functional-req.md, ../02_architecture/07_security-design.md]
---

# 07 運用・保守

> このカテゴリは「リリース後の運用・監視・障害対応」の手順を置く場所です。
> 運用要件（NFR-OPS-*）は `01_requirements/04_non-functional-req.md` に、ここは**実際の運用手順（ランブック）**を書きます。
> 長期運用のプロジェクトでは、ここが整っているかどうかが保守性を大きく左右します。

## このフォルダに置くドキュメント（想定）
| ファイル（案） | 内容 |
|---------------|------|
| `00_monitoring.md` | 監視項目・メトリクス・アラート（CloudWatch 等）・通知先 |
| `01_logging.md` | ログの集約先・形式・相関ID・保持期間 |
| `02_backup-restore.md` | バックアップ方針と復旧手順（RPO/RTO の実現手順） |
| `03_incident-response.md` | 障害対応フロー・エスカレーション・連絡体制 |
| `04_runbook.md` | 定常運用・よくある対応のランブック（手順書） |
| `05_maintenance.md` | パッチ適用・計画メンテナンス・依存更新の手順 |

## 作成の進め方
- リリース前に最低限 `02_backup-restore.md` と `03_incident-response.md` を用意する（事故対応の備え）。
- 障害やトラブル対応の経験は、都度ランブックに追記してナレッジ化する。
- セキュリティインシデント対応は `02_architecture/07_security-design.md` と連携する。

> このフォルダはまだ中身が未作成の予約カテゴリです。番号 07 は「運用・保守」の定位置として確保しています。
