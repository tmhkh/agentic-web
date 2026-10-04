---
status: Draft
owner: {{担当者名}}
reviewers: [{{レビュアー}}]
last_updated: YYYY-MM-DD
related: [../01_requirements/04_non-functional-req.md, ./01_infrastructure.md]
---

# システム全体構成（概要設計）

> 概要設計は「システムをどう組むか」の全体像。詳細設計（`../03_detailed-design`）の前提になる。
> アーキテクチャ上の重要判断は必ず ADR（`../08_process/adr`）に残す。

## 1. アーキテクチャ方針
- {{例: モノリス / モジュラモノリス / サービス分割}}（選定理由と ADR 番号を記載）
- レイヤ構成: {{例: プレゼンテーション / アプリケーション / ドメイン / インフラ}}
- 大規模（テーブル100超）前提のモジュール分割方針は [`06_data-partitioning.md`](./06_data-partitioning.md)

## 2. 論理構成図
```mermaid
graph TB
  subgraph Client[クライアント]
    Browser[ブラウザ]
  end
  subgraph AWS[AWS ap-northeast-1]
    WAF[AWS WAF] --> ALB[ALB / Nginx]
    ALB --> App[EC2: PHP / PHP-FPM]
    App --> RDS[(RDS PostgreSQL)]
    App --> Cache[{{キャッシュ: ElastiCache 等 任意}}]
    App --> Storage[{{S3: ファイル保存 任意}}]
  end
  Browser -->|HTTPS| WAF
```

## 3. 技術スタック詳細
| レイヤ | 技術 | バージョン | 備考 |
|-------|------|-----------|------|
| フロントエンド | {{}} | {{}} | |
| アプリケーション | PHP + {{FW}} | {{}} | |
| Webサーバ | {{Nginx}} | {{}} | |
| DB | PostgreSQL (RDS) | {{}} | |
| キャッシュ | {{}} | {{}} | 任意 |
| ジョブ / バッチ | {{}} | {{}} | `../03_detailed-design/batch` |

## 4. コンポーネント一覧（モジュール分割）
テーブル100超に耐えるため、業務ドメイン単位でモジュールを分ける。

| モジュール | 責務 | 主なテーブル群 | 依存モジュール |
|-----------|------|--------------|--------------|
| 認証・ユーザー管理 | ログイン・ユーザー発行 | user_* | — |
| {{業務ドメインA}} | {{}} | {{}} | {{}} |

## 5. 外部連携
| 連携先 | 方式 | 用途 | 認証 |
|--------|------|------|------|
| {{}} | {{REST/SFTP等}} | {{}} | {{}} |

## 6. 主要な非機能の実現方針
性能・可用性・セキュリティ（`../01_requirements/04_non-functional-req.md`）を、どの構成要素で満たすかを明記。

| NFR-ID | 実現方針 |
|--------|---------|
| NFR-PERF-* | {{インデックス設計 / キャッシュ / 読み書き分離}} |
| NFR-AVL-* | {{RDSマルチAZ / Auto Scaling / バックアップ}} |
| NFR-SEC-* | [`07_security-design.md`](./07_security-design.md) |
