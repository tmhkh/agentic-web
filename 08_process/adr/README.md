---
status: Draft
owner: {{担当者名}}
reviewers: [{{レビュアー}}]
last_updated: YYYY-MM-DD
related: [../templates/adr-template.md]
---

# ADR（Architecture Decision Record）運用ルール

> 重要な意思決定とその理由を記録する。「なぜこうしたか」を後から追えるようにし、
> 長期・多人数・AI併用の環境で判断のブレ・蒸し返しを防ぐ。

## 1. ADRを書く対象
- アーキテクチャ・技術スタックの選定（言語/FW/DB/インフラ構成）
- 認証・認可方式の決定
- データ分割・マルチテナント方式
- 外部連携方式
- その他、後戻りコストが高い / 複数案から選んだ判断

> 「どちらでもよい些細な実装選択」はADR不要。迷ったら書く方に倒す。

## 2. 運用
- 連番で管理: `0001-xxx.md`, `0002-xxx.md`。
- テンプレート: `../templates/adr-template.md`。
- ステータス: `Proposed` → `Accepted` / `Rejected`、後に `Superseded by ADR-xxxx`。
- 一度 `Accepted` にしたADRは**書き換えず**、変える場合は新しいADRで上書き（履歴を残す）。

## 3. ADR一覧
| ADR | タイトル | ステータス | 日付 |
|-----|---------|-----------|------|
| 0001 | アーキテクチャ決定を記録する | Accepted | YYYY-MM-DD |
| {{0002}} | {{}} | {{}} | {{}} |
