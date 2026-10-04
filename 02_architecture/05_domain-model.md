---
status: Draft
owner: {{担当者名}}
reviewers: [{{レビュアー}}]
last_updated: YYYY-MM-DD
related: [./06_data-partitioning.md, ../03_detailed-design/db/01_table-index.md]
---

# ドメインモデル（概念設計）

> 物理テーブル（`../03_detailed-design/db`）の前に、**業務概念とその関係**を整理する。
> ここで境界（モジュール分割）を決めると、テーブル100超でも構造が破綻しにくい。

## 1. 境界づけられたコンテキスト（モジュール境界）
```mermaid
graph TB
  subgraph Auth[認証・ユーザー]
    A1[User]
    A2[Role]
  end
  subgraph DomainA[{{業務ドメインA}}]
    B1[{{Entity}}]
  end
  Auth -. 参照 .-> DomainA
```

| コンテキスト | 責務 | 主要エンティティ | 他コンテキストとの関係 |
|-------------|------|----------------|----------------------|
| 認証・ユーザー | ユーザー・権限 | User, Role | 全体から参照される |
| {{業務A}} | {{}} | {{}} | {{}} |

## 2. 概念 ER 図（主要エンティティ）
```mermaid
erDiagram
  USER ||--o{ ROLE_ASSIGNMENT : has
  ROLE ||--o{ ROLE_ASSIGNMENT : assigned
  USER ||--o{ {{ENTITY}} : owns
```

## 3. 主要エンティティ定義（概念レベル）
| エンティティ | 説明 | 主な属性（概念） | 所有者 |
|-------------|------|----------------|--------|
| User | ログインする利用者 | 識別子, 氏名, ロール, 状態 | — |
| {{Entity}} | {{}} | {{}} | User / Tenant |

## 4. 不変条件・ビジネスルール
エンティティが常に満たすべきルール。実装時のバリデーション・制約の根拠。

| ID | ルール | 対象 |
|----|--------|------|
| BR-01 | {{例: 停止ユーザーはログインできない}} | User |
| BR-02 | {{}} | {{}} |
