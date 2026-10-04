---
status: Draft
owner: {{担当者名}}
reviewers: [{{レビュアー}}]
last_updated: YYYY-MM-DD
related: [../02_architecture/00_system-overview.md, ../02_architecture/07_security-design.md]
---

# 非機能要件（Non-Functional Requirements）

> 非機能要件は「測定可能な数値」で書く。曖昧な表現（速い・安定）は禁止。各項目に ID（NFR-xxx）を付与。

## 1. 性能（Performance）
| ID | 項目 | 要件（測定可能な数値） |
|----|------|----------------------|
| NFR-PERF-001 | 画面応答時間 | {{例: 95%タイルで 1.5秒以内（通常操作）}} |
| NFR-PERF-002 | API応答時間 | {{例: p95 500ms以内}} |
| NFR-PERF-003 | 同時接続 | {{例: ピーク同時 ◯ユーザー}} |
| NFR-PERF-004 | データ量 | {{例: ◯年で ◯件、最大テーブル ◯万行}} |

## 2. 可用性・信頼性（Availability）
| ID | 項目 | 要件 |
|----|------|------|
| NFR-AVL-001 | 稼働率（SLO） | {{例: 99.5% / 月}} |
| NFR-AVL-002 | 計画メンテナンス | {{頻度・時間帯}} |
| NFR-AVL-003 | RPO / RTO | {{バックアップからの復旧目標}} |
| NFR-AVL-004 | 冗長構成 | {{RDSマルチAZ / EC2複数台など}} |

## 3. セキュリティ（Security）
詳細は [`../02_architecture/07_security-design.md`](../02_architecture/07_security-design.md)。
| ID | 項目 | 要件 |
|----|------|------|
| NFR-SEC-001 | 通信 | 全通信 HTTPS（TLS {{1.2+}}）、HSTS |
| NFR-SEC-002 | 認証 | 管理者発行ユーザーのみ。{{多要素/パスキー採否}} |
| NFR-SEC-003 | 認可 | RBAC。水平・垂直権限昇格の防止 |
| NFR-SEC-004 | 機密情報管理 | SSM Parameter Store / Secrets Manager。ハードコード禁止 |
| NFR-SEC-005 | 脆弱性対策 | OWASP Top 10 対応、WAF {{採否}} |
| NFR-SEC-006 | 監査ログ | 認証・重要操作を記録、保持期間 {{}} |
| NFR-SEC-007 | PII保護 | {{暗号化・マスキング方針}} |

## 4. 保守性・拡張性（Maintainability）
| ID | 項目 | 要件 |
|----|------|------|
| NFR-MNT-001 | コード品質 | 静的解析パス（`rules.md`）、カバレッジ基準（`../04_quality`） |
| NFR-MNT-002 | ドキュメント | 本仕様体系の維持・更新 |
| NFR-MNT-003 | 拡張性 | {{テーブル100超を見据えたモジュール分割（`../02_architecture/06_data-partitioning.md`）}} |

## 5. 運用性（Operability）
| ID | 項目 | 要件 |
|----|------|------|
| NFR-OPS-001 | 監視 | {{CloudWatch メトリクス・アラート}} |
| NFR-OPS-002 | ログ | {{集約先・形式・相関ID}} |
| NFR-OPS-003 | デプロイ | {{CI/CD、ロールバック手順}} |
| NFR-OPS-004 | バックアップ | {{RDS自動バックアップ・保持日数}} |

## 6. ユーザビリティ・アクセシビリティ
| ID | 項目 | 要件 |
|----|------|------|
| NFR-USA-001 | 対応ブラウザ | {{}} |
| NFR-USA-002 | レスポンシブ | {{PC/モバイル対応範囲}} |
| NFR-USA-003 | アクセシビリティ | {{準拠レベル。WCAG等。全検証には手動テスト要}} |

## 7. 法令・コンプライアンス
| ID | 項目 | 要件 |
|----|------|------|
| NFR-CMP-001 | {{個人情報保護法 等}} | {{}} |
| NFR-CMP-002 | データ保管地域 | {{ap-northeast-1 等}} |
