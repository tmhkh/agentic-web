---
status: Draft
owner: {{担当者名}}
reviewers: [{{レビュアー}}]
last_updated: YYYY-MM-DD
related: [../../02_architecture/03_api-design-policy.md, ./templates/endpoint-template.md]
---

# API共通規約（詳細設計）

> 全体方針は `../../02_architecture/03_api-design-policy.md`。ここは詳細設計で守る具体ルール。

## 1. エンドポイント一覧（索引）
| API-ID | メソッド | パス | 概要 | 認可 | 関連FR | 詳細 |
|--------|---------|------|------|------|--------|------|
| AUTH-login | POST | /api/v1/auth/login | ログイン | 公開 | FR-AUTH-001 | [login.md](./auth/login.md) |
| AUTH-logout | POST | /api/v1/auth/logout | ログアウト | 認証 | — | {{}} |
| USER-create | POST | /api/v1/users | ユーザー発行 | 管理者 | FR-AUTH-002 | {{}} |
| {{API-ID}} | {{}} | {{}} | {{}} | {{}} | {{}} | {{}} |

## 2. 共通ヘッダ
| ヘッダ | 用途 |
|--------|------|
| `Authorization` | {{Bearer トークン / セッション}} |
| `X-Request-Id` | 相関ID（なければサーバ生成） |
| `Content-Type` | `application/json` |

## 3. 共通エラーコード
| code | HTTP | 意味 |
|------|------|------|
| `VALIDATION_ERROR` | 422 | 入力検証エラー |
| `UNAUTHENTICATED` | 401 | 未認証 |
| `FORBIDDEN` | 403 | 権限不足 |
| `NOT_FOUND` | 404 | リソース無し |
| `CONFLICT` | 409 | 競合（一意制約・楽観ロック） |
| `INTERNAL_ERROR` | 500 | サーバエラー |

## 4. 共通仕様
- ページング・ソート・フィルタの記法（`../../02_architecture/03_api-design-policy.md`）に従う。
- 全保護APIで認証・認可・データ所有権を検証。
- 入力は全てバリデーション。エラーは `details` に項目単位で返す。
- OpenAPI 定義を正とし、本一覧と同期する（可能なら自動生成）。
