# Webアプリ 仕様駆動開発ドキュメント体系

このリポジトリは **大規模（DBテーブル100超）・長期（開発期間1年以上）のWebアプリケーション** を、
**仕様駆動開発（Spec-Driven Development）× プロトタイピング** で進めるためのドキュメント一式です。

- 対象: インターネットに公開するWebアプリ。**管理者が発行したユーザーのみ**がログインしてアクセスできるシステム
- サーバーサイド: **AWS EC2（PHP）+ RDS（PostgreSQL）**
- 開発体制: 人間 + AIエージェント（コード生成・レビュー補助）
- 前提課題: 規模が大きく **人間が全コードを把握しきれない**。そのため「ドキュメントを埋めれば要件が確定し、認識齟齬なく実装できる」状態を目指す

> このテンプレートは **汎用的なWebアプリ向け**です。各ドキュメントの `{{ }}` や `TODO:` を埋めると、
> プロジェクト固有の仕様書になります。空欄のまま実装に進んではいけません（空欄＝未決定事項）。

---

## 0. このドキュメント群の使い方（最重要）

1. **埋める順番は上から下**。上流（要件）が未確定のまま下流（設計・実装）に進まない。
2. 各ドキュメントの先頭に **ステータス**（`Draft` / `Reviewing` / `Agreed` / `Frozen`）と **最終更新日・担当** を書く。
3. **`Agreed`（顧客合意済み）になっていない要件は実装に着手しない**。合意の取り方は [`08_process/02_agreement-flow.md`](./08_process/02_agreement-flow.md) 参照。
4. 決定事項は必ず **ADR**（[`08_process/adr/`](./08_process/adr/)）に残す。「なぜそうしたか」を後から追えるようにする。
5. AIエージェントに作業を依頼するときは **該当ドキュメントのパスを指定** し、「ここに書かれていないことは勝手に決めず質問する」ことを徹底させる（[`rules.md`](./rules.md) 参照）。

---

## 1. ドキュメント体系（全体像）

```
webapp-spec/
├─ README.md                      ← 本ファイル（入口・読む順番）
├─ rules.md                       ← チーム共有ルール（人間 & AI 共通の憲法）
├─ glossary.md                    ← 用語集（ユビキタス言語）
│
├─ 01_requirements/               ← ① 要件定義（WHAT / WHY）
│   ├─ 00_project-overview.md         プロジェクト概要・背景・ゴール
│   ├─ 01_scope.md                    スコープ（やること／やらないこと）
│   ├─ 02_stakeholders-roles.md       ステークホルダー・ユーザー種別・ロール
│   ├─ 03_functional-requirements.md  機能要件（FRリスト）
│   ├─ 04_non-functional-req.md       非機能要件（性能/可用性/セキュリティ等）
│   ├─ 05_use-cases.md                ユースケース／ユーザーストーリー
│   └─ 06_constraints-assumptions.md  制約・前提・リスク
│
├─ 02_architecture/               ← ② 概要設計（基本設計 / HOW・全体）
│   ├─ 00_system-overview.md          システム全体構成
│   ├─ 01_infrastructure.md           インフラ構成（EC2/RDS/ネットワーク）
│   ├─ 02_auth-and-access.md          認証・認可・アクセス制御方式
│   ├─ 03_api-design-policy.md        API設計方針（共通仕様）
│   ├─ 04_screen-list.md              画面一覧・画面遷移
│   ├─ 05_domain-model.md             ドメインモデル（概念）/ 境界づけ
│   ├─ 06_data-partitioning.md        データ分割・テーブル群の管理方針（100超対策）
│   └─ 07_security-design.md          セキュリティ設計（全体方針）
│
├─ 03_detailed-design/            ← ③ 詳細設計（実装仕様）
│   ├─ 00_how-to-write.md             詳細設計の書き方・粒度ルール
│   ├─ db/
│   │   ├─ 00_db-conventions.md       DB設計規約（命名・型・共通カラム）
│   │   ├─ 01_table-index.md          テーブル一覧（ドメイン別インデックス）
│   │   └─ templates/
│   │       └─ table-template.md      テーブル定義テンプレート
│   ├─ api/
│   │   ├─ 00_api-conventions.md      API共通規約
│   │   └─ templates/
│   │       └─ endpoint-template.md   エンドポイント定義テンプレート
│   ├─ screen/
│   │   └─ templates/
│   │       └─ screen-template.md     画面詳細設計テンプレート
│   └─ batch/
│       └─ templates/
│           └─ batch-template.md      バッチ／非同期処理テンプレート
│
├─ 04_quality/                    ← ④ 品質担保（人間が全コードを追えない前提）
│   ├─ 00_quality-strategy.md         品質戦略の全体像
│   ├─ 01_test-strategy.md            テスト戦略（層別・カバレッジ）
│   ├─ 02_code-review-policy.md       レビュー方針（人間 & AI）
│   ├─ 03_ai-generated-code.md        AI生成コードの検証ルール
│   ├─ 04_traceability.md             トレーサビリティ（要件↔設計↔実装↔テスト）
│   └─ 05_acceptance-criteria.md      受け入れ基準（Doneの定義）
│
├─ 05_implementation/             ← 実装ガイド（予約カテゴリ / 現状 README のみ）
│   └─ README.md                      環境構築・実装手順の置き場
│
├─ 06_infrastructure-deployment/  ← インフラ / デプロイ（予約カテゴリ / 現状 README のみ）
│   └─ README.md                      IaC・CI/CD・デプロイ手順の置き場
│
├─ 07_operations/                 ← 運用・保守（予約カテゴリ / 現状 README のみ）
│   └─ README.md                      監視・障害対応・ランブックの置き場
│
└─ 08_process/                    ← ⑤ 進め方・顧客合意
    ├─ 00_dev-process.md              開発プロセス（仕様駆動×プロトタイピング）
    ├─ 01_milestones.md               マイルストーン・フェーズ計画
    ├─ 02_agreement-flow.md           顧客との合意の取り方
    ├─ 03_change-management.md        変更管理フロー
    ├─ 04_issue-management.md         課題・リスク管理
    ├─ adr/
    │   ├─ README.md                  ADRの運用ルール
    │   └─ 0001-record-architecture-decisions.md
    └─ templates/
        ├─ adr-template.md
        ├─ change-request-template.md
        └─ meeting-minutes-template.md
```

---

## 2. 読む順番（役割別）

| 役割 | 最初に読むべきドキュメント |
|------|---------------------------|
| 全員 | `rules.md` → `glossary.md` → `01_requirements/00_project-overview.md` |
| PM / PdM | `08_process/*`、`01_requirements/01_scope.md` |
| アーキテクト | `02_architecture/*` |
| 開発者（人間） | `rules.md` → `03_detailed-design/*` → `04_quality/*` |
| AIエージェント | 作業指示で指定された設計ドキュメント + `rules.md` + `04_quality/03_ai-generated-code.md` |
| 顧客 / ステークホルダー | `01_requirements/*`、`02_architecture/04_screen-list.md` |

> 番号は「開発工程の順番」を表します。`05`〜`07`（実装ガイド / インフラ・デプロイ / 運用・保守）は
> 工程カテゴリの定位置として確保した**予約カテゴリ**で、現状は各フォルダに `README.md`（想定ラインナップ）だけを置いています。
> 必要になったフェーズで中身を作成してください。欠番ではありません。

---

## 3. ステータス定義

| ステータス | 意味 | 下流作業の可否 |
|-----------|------|---------------|
| `Draft` | 作成中。内容は未確定 | 実装不可 |
| `Reviewing` | チーム内レビュー中 | 実装不可 |
| `Agreed` | 顧客合意済み | **実装可** |
| `Frozen` | 当該フェーズで変更凍結 | 実装可・変更は変更管理フロー経由 |

各ドキュメント冒頭のメタ情報ブロックで必ず明示すること。

---

## 4. メタ情報ブロック（全ドキュメント共通ヘッダ）

各ドキュメントの先頭に以下を記載する:

```md
---
status: Draft            # Draft | Reviewing | Agreed | Frozen
owner: {{担当者名}}
reviewers: [{{レビュアー}}]
last_updated: YYYY-MM-DD
related: [関連ドキュメントのパス]
---
```
