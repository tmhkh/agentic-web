---
status: Draft
owner: {{担当者名}}
reviewers: [{{レビュアー}}]
last_updated: YYYY-MM-DD
related: [../00_db-conventions.md, ../01_table-index.md]
---

# テーブル定義: `{{table_name}}`

> このテンプレートをコピーして1テーブル1ファイルで作成する。`db/{domain}/{table_name}.md`。
> 記入例は `../00_db-conventions.md` 配下、または `01_table-index.md` の `auth_users` エントリ参照。

## 1. 概要
| 項目 | 内容 |
|------|------|
| 物理名 | `{{table_name}}` |
| 論理名 | {{日本語名}} |
| ドメイン | {{接頭辞と所属ドメイン}} |
| 概要 | {{このテーブルが保持するもの}} |
| 所有者 | {{user_id / tenant_id / なし}} |
| 削除方針 | {{物理 / 論理（deleted_at）}} |
| 想定行数 | {{初期 / ◯年後}} |
| 関連FR | FR-{{xxx}} |

## 2. カラム定義
| # | カラム | 型 | NULL | デフォルト | 一意 | 説明 / 制約 |
|---|--------|-----|:----:|-----------|:----:|------------|
| 1 | `id` | `bigint` IDENTITY | NO | — | PK | 主キー |
| 2 | `user_id` | `bigint` | NO | — | — | 所有者（FK: auth_users.id） |
| 3 | `{{column}}` | `{{type}}` | {{YES/NO}} | {{}} | {{}} | {{説明・CHECK制約など}} |
| .. | `created_at` | `timestamptz` | NO | `now()` | — | 作成日時 |
| .. | `updated_at` | `timestamptz` | NO | `now()` | — | 更新日時 |

## 3. 制約・インデックス
| 種別 | 名前 | 対象カラム | 備考 |
|------|------|-----------|------|
| PRIMARY KEY | `pk_{{table}}` | `id` | |
| FOREIGN KEY | `fk_{{table}}_user` | `user_id` → `auth_users.id` | ON DELETE {{}} |
| UNIQUE | `uq_{{table}}_{{col}}` | {{}} | |
| INDEX | `idx_{{table}}_{{col}}` | {{}} | 検索条件 |
| CHECK | `ck_{{table}}_{{rule}}` | {{}} | {{}} |

## 4. リレーション
```mermaid
erDiagram
  auth_users ||--o{ {{table_name}} : owns
```
| 関連テーブル | カーディナリティ | 説明 |
|------------|----------------|------|
| `auth_users` | 1 : N | 所有者 |

## 5. ビジネスルール・不変条件
| ID | ルール |
|----|--------|
| BR-{{}} | {{例: amount は 0 以上}} |

## 6. アクセス制御
- このテーブルを参照・更新できるロール: {{}}
- データ所有権: 必ず `user_id`（or `tenant_id`）でスコープする。

## 7. DDL（参考）
```sql
CREATE TABLE {{table_name}} (
  id          bigint GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
  user_id     bigint NOT NULL REFERENCES auth_users(id),
  -- {{columns}}
  created_at  timestamptz NOT NULL DEFAULT now(),
  updated_at  timestamptz NOT NULL DEFAULT now()
);
CREATE INDEX idx_{{table_name}}_user_id ON {{table_name}} (user_id);
```

## 8. テスト観点
- [ ] 一意制約・外部キー制約が機能する
- [ ] 所有者以外からアクセスできない（認可）
- [ ] 境界値（最大長・最小/最大数値）
- [ ] NULL可否・デフォルト
