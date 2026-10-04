---
status: Draft
owner: {{担当者名}}
reviewers: [{{レビュアー}}]
last_updated: YYYY-MM-DD
related: [../README.md, ../02_architecture/01_infrastructure.md, ../02_architecture/07_security-design.md]
---

# 06 インフラ / デプロイ

> このカテゴリは「インフラの実体（IaC）とデプロイ・CI/CD の手順」を置く場所です。
> 設計意図は `02_architecture/01_infrastructure.md` に、ここは**具体的な構築・デプロイ運用**を書きます。
> インフラは IaC でコード化し、手動構築は禁止（`rules.md`）。本カテゴリは手順と運用ルールを記録します。

## このフォルダに置くドキュメント（想定）
| ファイル（案） | 内容 |
|---------------|------|
| `00_iac-overview.md` | IaC（Terraform / CloudFormation）の構成・リポジトリ・運用方針 |
| `01_environments.md` | dev / stg / prod の構成差分・パラメータ管理（SSM / Secrets Manager） |
| `02_deploy-procedure.md` | デプロイ手順・ロールバック手順（EC2/PHP への反映方法） |
| `03_cicd-pipeline.md` | CI/CD（ビルド・テスト・デプロイの自動化、認証方式） |
| `04_secrets-management.md` | 機密情報の扱い（`.env` 生成、ハードコード禁止の実務手順） |

## 作成の進め方
- 環境は IaC を正とし、本カテゴリのドキュメントは「意図・手順・運用ルール」を補完する位置づけ。
- 機密情報・本番エンドポイントはドキュメントにも直接書かない（キー名のみ）。
- 追加課金・月額コストが発生する構成変更は、事前に顧客・PM へ提示する（`rules.md` コスト判断）。

> このフォルダはまだ中身が未作成の予約カテゴリです。番号 06 は「インフラ / デプロイ」の定位置として確保しています。
