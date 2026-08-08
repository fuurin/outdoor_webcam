# outdoor webcam ドキュメント

`documents/` は、オフグリッドIoTカメラの要件、調査、仕様および段階的な実験記録を管理する。

## ドキュメント管理ルール

- ファイル名の先頭には、同じディレクトリ内で読む順番を表す2桁の連番を付ける。
- `design/` には「要件 → 調査 → 確定仕様」の順で、プロジェクト全体に関わる情報を置く。
- `examples/` には再現可能な実験手順を1ステップ1ファイルで置き、進捗は `examples/00_overview.md` で管理する。
- 実験で使用する画像は `examples/images/` に置き、対応するステップ番号から始まる名前にする。
- 確認できた事実、採用済み仕様、未実施の計画を区別して記載する。
- 外部情報には可能な限り参照URLを付け、価格や製品情報には調査時点を記載する。
- Wi-Fiパスワード、Botトークン、APN認証情報などの秘密情報は記載・コミットしない。
- 実験の追加や順序変更を行った場合は、ファイル名、見出し、相互参照、`00_overview.md` を同時に更新する。

## ファイル構成

```text
documents/
├── README.md                         # 本ディレクトリの構成と管理ルール
├── design/
│   ├── 00_requirements.md            # プロジェクトの要件
│   ├── 01_survey.md                  # 部品・通信・省電力化の調査と選定根拠
│   └── 02_specification.md           # 現時点で決定しているシステム仕様
└── examples/
    ├── 00_overview.md                # 実験全体の進め方と進捗
    ├── 01_usb_serial.md              # USB書き込みとシリアル出力
    ├── 02_wifi.md                    # Wi-Fi接続
    ├── 03_microsd.md                 # microSD読み書き
    ├── 04_camera_sd.md               # OV2640撮影とmicroSD保存
    ├── 05_platformio.md              # VS Code + PlatformIO環境の導入
    ├── 06_slack_wifi.md              # Wi-Fi経由のSlack画像送信
    ├── 07_deep_sleep.md              # DeepSleepと24時間周期の実行
    ├── 08_lte_soracom.md             # SORACOM SIMによるLTE通信
    └── images/                       # 実験手順から参照する画像
```

価格、販売状況、外部サイトの内容は2026-08-08時点のもので、今後変わる可能性がある。
