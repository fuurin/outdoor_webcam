# cloud

AWS と Slack 連携に関する設定、Infrastructure as Code、補助スクリプトを配置するディレクトリです。

## 想定する構成要素

- S3 バケット
- IAM ロール/ポリシー
- Lambda による Slack 通知
- S3 Event Notification
- CloudWatch Logs / Metrics
- 秘密情報管理（Secrets Manager または SSM Parameter Store）

## 方針

端末側に Slack Webhook URL を持たせず、S3 への画像保存を契機に AWS Lambda から Slack へ通知する構成にします。
