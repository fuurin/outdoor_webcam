# 製作に必要な情報・パーツ選定調査

2026-08-08時点の調査記録である。価格は当時の参考値であり、購入前に再確認する。

## 開発ボード

### 採用候補: LILYGO T-SIM7080G-S3

SIMスロットとLTEモジュールをマイコンへ外付けする構成は、部品単体でも約3,000〜10,000円になり、配線ミスや破損のリスクも増える。そのため、必要な機能が一枚にまとまった `LILYGO ESP32-S3 T-SIM7080G-S3` を選定した。

- 調査時価格: 6,100円 + 配送料1,300円
- ESP32-S3搭載
- SIM7080G LTEモジュール、nano SIMスロット搭載
- 2.4GHz Wi-Fi搭載
- microSDスロット搭載
- カメラ用FPCコネクタ搭載（カメラ本体は別売）
- 1セルLi-ion/LiPo（3.7V）用コネクタ搭載
- GPS搭載（現時点では必須ではない）
- SIM7080G単体の公称値はPower Save Mode 3.2µA、スリープ時0.6mA。ただしボード全体の待機電流は実測が必要。

関連リポジトリ:

- https://github.com/Xinyuan-LilyGO/LilyGo-T-SIM7080G
- https://github.com/Xinyuan-LilyGO/LilyGo-Cam-ESP32S3

## カメラ

FPC接続のOV2640カメラを選定した。

- 200万画素
- 広角160度
- 24ピンインターフェース
- 調査時価格: 1,712円 + 配送料100円
- ケーブルに長さがあり、ケースへ収めやすいと判断

## 電池

ボードの18650バッテリーホルダーのスプリング間を実測すると約65mmだった。

- 約65mm: 保護回路なしセルが適合する可能性が高い
  - 選定例: Panasonic NCR18650B 3.7V / 3400mAh
- 約69mm: 保護回路付きセルの例
  - KEEPPOWER 18650 3.7V / 3500mAh、PCB保護回路搭載

今回は寸法に合わせ、保護回路なしのPanasonic NCR18650Bを選定した。ショート、過充電、過放電への対策はボード側の保護機能も含め、実機で確認する必要がある。

初回購入は開発ボード、OV2640カメラ、18650セルの合計で、当初11,010円、値引き後9,212円だった。microSDカードは手持ち品を使用する。

## SIM・通信

- SORACOM nano SIMを利用する計画
- 候補: 日本カバレッジ IoT SIM（plan-D）
- Wi-Fiで撮影・保存・Slack通知を先に検証し、その後LTEへ置き換える

## 省電力化

ESP32のDeepSleepだけでは周辺部品すべての電力を遮断できない可能性があるため、段階的に検討する。

1. ESP-IDF/Arduino APIによるDeepSleep
2. テスターによる実機の待機電流測定
3. MOSFETを使い、撮影時以外の周辺回路への給電を遮断
4. TPL5110などの超低消費電力タイマーでMOSFETまたはシステム全体を起動
5. 必要に応じてソーラーパネルを追加

測定器の候補としてANENG AN8009デジタルマルチメーターが挙げられている。

## 開発環境・参考資料

- Arduino IDE: https://www.arduino.cc/en/software/
- ESP32 Boards Manager URL: https://espressif.github.io/arduino-esp32/package_esp32_index.json
- microSDサンプル: https://github.com/Xinyuan-LilyGO/LilyGo-T-SIM7080G/tree/master/examples/MinimalSDCardExample
- VS Code + PlatformIOの参考記事: https://qiita.com/tronicboy/items/fd7d5951dd49f95e1b6e
