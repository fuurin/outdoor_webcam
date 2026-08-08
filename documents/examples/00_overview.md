# 実験ステップ概要

最終システムを一度に組み上げず、各機能を独立して確認してから統合する。完了したステップの結果を次のステップの前提とする。

## 進捗

| Step | 内容 | 完了条件 | 状態 |
|---:|---|---|---|
| 1 | USB接続・書き込み・シリアル出力 | スケッチを書き込み、Serial Monitorで出力を確認できる | 完了 |
| 2 | Wi-Fi接続 | IPアドレスを取得し、RSSIを確認できる | 完了 |
| 3 | microSD読み書き | ファイルの作成・読み込み・削除ができる | 完了 |
| 4 | OV2640撮影・microSD保存 | JPEGを撮影してmicroSDへ保存できる | 完了 |
| 5 | VS Code + PlatformIO環境の導入 | 既存相当のスケッチをビルド・書き込み・監視できる | 未実施 |
| 6 | Wi-Fi経由のSlack画像送信 | 保存したJPEGをSlackチャンネルへ送信できる | 未実施 |
| 7 | DeepSleepと1日周期の自動実行 | 一連の処理後にSleepし、タイマーで再起動できる | 未実施 |
| 8 | SORACOM SIMによるLTE通信 | Wi-FiなしでSlackへの画像送信に成功する | 未実施 |

## 実施順序

1. [USB接続・書き込み・シリアル出力](01_usb_serial.md)
2. [Wi-Fi接続](02_wifi.md)
3. [microSD読み書き](03_microsd.md)
4. [OV2640撮影・microSD保存](04_camera_sd.md)
5. [VS Code + PlatformIO環境の導入](05_platformio.md)
6. [Wi-Fi経由のSlack画像送信](06_slack_wifi.md)
7. [DeepSleepと1日周期の自動実行](07_deep_sleep.md)
8. [SORACOM SIMによるLTE通信](08_lte_soracom.md)

各ステップでは、使用した設定とコード、確認結果、次のステップへ持ち越す課題を記録する。
