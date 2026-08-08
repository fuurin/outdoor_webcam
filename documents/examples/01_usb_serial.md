# Step 1: USB接続・書き込み・シリアル出力

状態: **完了**

## 目的

MacからESP32-S3へArduinoスケッチを書き込み、USBシリアルで実行結果を確認する。

## セットアップ

1. Arduino IDEを https://www.arduino.cc/en/software/ からインストールする。
2. PreferencesのAdditional boards manager URLsへ次を追加する。

   `https://espressif.github.io/arduino-esp32/package_esp32_index.json`

3. Boards Managerで `esp32 by Espressif Systems` をインストールする。
4. USB Type-CでボードをMacへ接続する。
5. ターミナルを開き、`ls /dev/cu.*` で `/dev/cu.usbmodem101` のようなポートを確認する。
6. Boardを `ESP32S3 Dev Module`、Portを該当デバイス、USB CDC On Bootを `Enabled` にする。

## スケッチ

```cpp
void setup() {
  Serial.begin(115200);
  while (!Serial);
  Serial.println("Hello LILYGO!");
}

void loop() {
  delay(1000);
  Serial.println("Running...");
}
```

## 結果

Upload後、115200 baudのSerial Monitorへ `Hello LILYGO!` と1秒ごとの `Running...` が表示された。ボード上のボタンは上からRST、BOOT、PWR、SIM-BOOT。RSTで再起動するとsetupのメッセージから再開する。
