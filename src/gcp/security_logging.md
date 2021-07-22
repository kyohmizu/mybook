# Logging


### クエリ

https://cloud.google.com/logging/docs/view/query-library
### Access Transparency Logs (アクセスの透明性ログ)

[https://cloud.google.com/logging/docs/audit/access-transparency-overview](https://cloud.google.com/logging/docs/audit/access-transparency-overview)

- 「アクセスの透明性ログには、Google の担当者がお客様のコンテンツにアクセスした際に行った操作が記録されます。」
- 一定のサポートレベルが必要

### Audit Logs (監査ログ)

[https://cloud.google.com/logging/docs/audit](https://cloud.google.com/logging/docs/audit)

- 監査ログの種類
    - 管理アクティビティ (activity)
    - データアクセス (data_access)
    - システムイベント (system_event)
    - ポリシー拒否 (policy)
- 自動的に _Required バケットに保存される
    - activity と system_event のみ
- IAM & Admin - Audit Logs でデータアクセス監査ログの種類を設定
    - Admin Write は必須
    - ユーザーごとにログを出力しないようにもできる
- 除外設定
  - ログクエリを使用する
  - https://cloud.google.com/logging/docs/exclusions
- ログのコピー
  - https://cloud.google.com/logging/docs/routing/copy-logs

### ログ転送

[https://cloud.google.com/logging/docs/routing/overview](https://cloud.google.com/logging/docs/routing/overview)

[https://cloud.google.com/logging/docs/export/configure_export_v2](https://cloud.google.com/logging/docs/export/configure_export_v2)

- 転送先の種類
    - ログバケット → リアルタイム
    - GCS → 1時間ごと
    - Pub/Sub → ほぼリアルタイム
    - BigQuery → 〃
    - Splunk → 〃
- ログルーターを作成
    - 監査ログの場合、_Required の条件を参考にする
    - 転送先の書き込み権限が必要
    - GCS の場合、転送先の project 名は指定不要（project を通して名前が一意になっている）
- terraform 実装例:

```t
resource "google_logging_project_sink" "audit" {
  project     = "project_id"
  name        = "audit-log-sink"
  destination = "logging.googleapis.com/projects/audit_project_id/locations/global/buckets/audit"
  filter      = "LOG_ID(\"cloudaudit.googleapis.com/activity\") OR LOG_ID(\"externalaudit.googleapis.com/activity\") OR LOG_ID(\"cloudaudit.googleapis.com/system_event\") OR LOG_ID(\"externalaudit.googleapis.com/system_event\") OR LOG_ID(\"cloudaudit.googleapis.com/access_transparency\") OR LOG_ID(\"externalaudit.googleapis.com/access_transparency\")"

  unique_writer_identity = true
}

resource "google_project_iam_member" "log-writer" {
  project = "audit_project_id"
  role    = "roles/logging.bucketWriter"
  member  = google_logging_project_sink.audit[0].writer_identity

  condition {
    title       = "audit-log"
    expression  = "resource.name.endsWith('locations/global/buckets/audit')"
  }
}
```
