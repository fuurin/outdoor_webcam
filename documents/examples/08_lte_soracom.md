# Step 8: SORACOM SIMによるLTE通信

状態: **未実施**

## 目的

Wi-Fi接続をSIM7080GとSORACOM nano SIMによるLTE通信へ置き換え、屋外でSlack通知を行う。

## 実施予定

1. SORACOM IoT SIM（plan-D候補）を開通し、nano SIMを装着する。
2. アンテナ、SIM向き、対応周波数帯を確認する。
3. APNを設定し、ネットワーク登録状態を確認する。
4. LTE経由でHTTPS通信を確認する。
5. Step 6のSlack画像送信をLTE経由で実行する。
6. 圏外、タイムアウト、再接続、送信失敗時の挙動を確認する。
7. 通信後にSIM7080GをPower Save Modeへ移行し、消費電流を測定する。

## 完了条件

- Wi-Fiなしで画像通知に成功する。
- 通信失敗時にも画像がmicroSDへ残る。
- 通信処理に上限時間と再試行回数が設定され、永久待ちにならない。
- DeepSleepを含む24時間周期で安定動作する。

参考: https://github.com/Xinyuan-LilyGO/LilyGo-T-SIM7080G
