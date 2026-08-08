# outdoor_webcam

電源やWi-Fiのない屋外で使用する、バッテリー駆動のIoTカメラプロジェクトです。1日ごとに静止画を撮影してmicroSDカードへ保存し、SORACOM SIMによるLTE通信を通してSlackへ画像を送信した後、DeepSleepへ移行することを目指します。

## システム概要

```text
タイマー起動
    ↓
OV2640でJPEGを撮影
    ↓
microSDへ保存
    ↓
SIM7080G + SORACOM SIMでLTE接続
    ↓
Slack Files APIで画像を送信
    ↓
DeepSleep（約24時間）
```

通信に失敗した場合も、撮影画像はmicroSDカードへ残す方針です。

## 主なハードウェア

| 区分 | 採用構成 |
|---|---|
| 開発ボード | LILYGO T-SIM7080G-S3（ESP32-S3 + SIM7080G） |
| カメラ | OV2640、200万画素、広角160度、24ピンFPC |
| ストレージ | microSDカード |
| モバイル通信 | SORACOM nano SIM、LTE |
| 電源 | Panasonic NCR18650B 3.7V / 3400mAh |
| 通知先 | Slack Files API |

詳しい採用仕様は [documents/design/02_specification.md](documents/design/02_specification.md) を参照してください。

実験の順序と完了条件は [documents/examples/00_overview.md](documents/examples/00_overview.md) で管理しています。

## リポジトリ構成

```text
.
├── README.md        # プロジェクト概要
├── .gitignore       # Git管理対象外の設定
├── documents/
│   ├── README.md    # ドキュメントの構成と管理ルール
│   ├── design/      # 要件、調査、確定仕様
│   └── examples/    # 段階的な実験手順と画像
└── main/
    └── src/         # PlatformIOアプリケーションコード
```

## ドキュメント

- [要件定義](documents/design/00_requirements.md)
- [部品・方式の調査](documents/design/01_survey.md)
- [システム仕様](documents/design/02_specification.md)
- [実験ステップ一覧](documents/examples/00_overview.md)
- [ドキュメント管理ルール](documents/README.md)

## 開発環境

開発には主に VS Code と PlatformIO を使用します。導入方法と初期設定は [Step 5: VS Code + PlatformIO環境の導入](documents/examples/05_platformio.md) にまとめています。

PlatformIOプロジェクトを `main/` に構築し、アプリケーションコードを `main/src/` で管理します。

## 認証情報

次のような情報はリポジトリへコミットしないでください。

- Wi-FiのSSIDとパスワード
- Slack AppのBotトークン
- SIM/APNの認証情報

実値はGit管理外の `secrets.h` などに置きます。共有用のサンプルを作る場合は、`secrets.example.h` のような別ファイルへキー名とダミー値だけを記載してください。
