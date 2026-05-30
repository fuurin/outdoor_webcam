# firmware

マイコンまたは SBC 上で動作するプログラムを配置するディレクトリです。

## 想定する責務

- RTC/タイマー起床
- カメラ初期化と静止画撮影
- LTE/4G モジュール制御
- S3 への画像アップロード
- Slack 通知、または AWS 側通知トリガー
- 電池電圧・電波状態などのメタデータ取得
- deep sleep / 電源断

## 初期候補

- ESP32-S3 系: ESP-IDF または Arduino core
- Raspberry Pi Zero 2 W: Python または shell script

具体的な実装はパーツ選定後に追加します。
