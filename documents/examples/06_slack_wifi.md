# Step 6: Wi-Fi経由のSlack画像送信

状態: **未実施**

## 目的

Step 4で保存したJPEGをWi-Fi経由でSlackへ送信し、LTE導入前にアプリケーション層を検証する。

## 準備

1. Slack Appを作成する。
2. Bot Token Scopesへ `files:write` を追加する。
3. Appを対象ワークスペースへインストールし、Botトークン（`xoxb-`）を取得する。
4. 送信先チャンネルへBotを参加させ、チャンネルIDを確認する。
5. Botトークン、チャンネルID、Wi-Fi認証情報をGit管理外の設定ファイルへ保存する。

BotトークンやWi-Fiパスワードはソースコード、`platformio.ini`、シリアルログへ記載しない。

## Slack Files APIの送信フロー

1. `files.getUploadURLExternal` へファイル名とファイルサイズを送り、`upload_url` と `file_id` を取得する。
2. 取得した `upload_url` へmicroSD上のJPEGデータをアップロードする。
3. `files.completeUploadExternal` へ `file_id` と送信先チャンネルIDを送り、共有を確定する。

`files.upload` は2025-11-12に廃止されたため使用しない。

- https://docs.slack.dev/reference/scopes/files.write/
- https://docs.slack.dev/reference/methods/files.getUploadURLExternal/
- https://docs.slack.dev/reference/methods/files.completeUploadExternal/

## 実施予定

1. Git管理外の設定からSlackとWi-Fiの認証情報を読み込む。
2. Wi-Fiへ接続する。
3. 撮影画像、端末ID、撮影情報をSlackへ送る。
4. 各APIのHTTPステータスとJSONの `ok` を確認する。
5. 失敗時もmicroSD上の画像を保持し、再送可能な状態にする。
6. タイムアウトと最大再試行回数を設け、永久待ちを防ぐ。

## 完了条件

- SlackチャンネルへJPEGと端末識別情報を送信できる。
- トークンがファームウェアやログへ露出しない。
- 途中のAPI呼び出しに失敗した場合、原因を記録して画像を保持できる。
