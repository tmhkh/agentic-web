---
status: Draft
owner: {{担当者名}}
reviewers: [{{レビュアー}}]
last_updated: YYYY-MM-DD
related: [../../02_architecture/06_data-partitioning.md, ./01_table-index.md, ./templates/table-template.md]
---

# DB設計規約（PostgreSQL）

> テーブル100超でも一貫性を保つための共通規約。全テーブルがこれに従う。CIで一部を自動チェックすることを推奨。

## 1. 命名規約
| 対象 | 規約 | 例 |
|------|------|-----|
| テーブル | `{ドメイン接頭辞}_{複数形}`・snake_case | `auth_users`, `billing_invoices` |
| カラム | snake_case | `email_address` |
| 主キー | `id`（`bigint` or `uuid`、プロジェクトで統一） | `id` |
| 外部キー | `{参照先単数}_id` | `user_id` |
| インデックス | `idx_{table}_{columns}` | `idx_auth_users_email` |
| ユニーク制約 | `uq_{table}_{columns}` | `uq_auth_users_email` |
| 外部キー制約 | `fk_{table}_{ref}` | `fk_orders_user` |
| チェック制約 | `ck_{table}_{rule}` | `ck_orders_amount_positive` |

- 予約語・大文字始まりの識別子は使わない。

## 2. 共通カラム（全テーブル必須）
| カラム | 型 | 説明 |
|--------|-----|------|
| `id` | `bigint` GENERATED ALWAYS AS IDENTITY / `uuid` | 主キー |
| `created_at` | `timestamptz` NOT NULL DEFAULT now() | 作成日時 |
| `updated_at` | `timestamptz` NOT NULL DEFAULT now() | 更新日時（トリガ or アプリで更新） |
| `deleted_at` | `timestamptz` NULL | 論理削除する場合のみ |

- ユーザー所有データには `user_id`（or `tenant_id`）を**必須**とし、インデックスを張る。

## 3. 型の標準
| 用途 | 推奨型 |
|------|--------|
| 文字列（可変） | `varchar(n)` / `text` |
| 真偽 | `boolean` |
| 金額 | `numeric(precision, scale)`（`float` 禁止） |
| 日時 | `timestamptz`（UTC保存） |
| 日付 | `date` |
| 列挙 | {{`varchar` + CHECK or PostgreSQL `enum` のどちらかに統一}} |
| JSON | `jsonb` |

## 4. 制約・整合性
- NOT NULL をデフォルトとし、NULL許可は意図を明記。
- 外部キー制約を張る（ドメイン境界をまたぐ場合の方針は `data-partitioning` 参照）。
- 一意性はDB制約で担保（アプリのみに頼らない）。
- 削除方針（物理 / 論理）をテーブルごとに明記。

## 5. インデックス方針
- 外部キー、頻出の検索・絞り込み・ソート条件に張る。
- 大規模テーブルは複合インデックス・部分インデックスを検討。
- 不要なインデックスは書き込み負荷になるため作らない。

## 6. マイグレーション
- ツール: {{Flyway / Laravel Migration / sqldef など}}
- 1マイグレーション1目的。ロールバック可能性を考慮。
- 破壊的変更（列削除・型変更）は段階適用（追加→移行→削除）を検討。
- スキーマ変更PRには影響テーブル・影響範囲・ロールバック手順を記載。

## 7. パフォーマンス・大規模対応
- {{宣言的パーティショニングの適用基準（例: 1000万行超 or 時系列）}}
- {{アーカイブ方針（古いデータの退避）}}
- 重いクエリは `EXPLAIN ANALYZE` で検証し、設計に根拠を残す。
