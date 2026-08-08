# Step 2: Wi-Fi接続

状態: **完了**

## 目的

ESP32-S3を2.4GHz Wi-Fiへ接続し、IPアドレスと受信強度を確認する。

## スケッチ

認証情報は実値をコミットせず、Git管理外の設定ファイルから読み込むこと。

```cpp
#include <WiFi.h>

const char* ssid = "YOUR_SSID";
const char* password = "YOUR_PASSWORD";

void setup() {
  Serial.begin(115200);
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);

  Serial.println("Connecting to WiFi...");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("\nConnected!");
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());
}

void loop() {
  delay(5000);
  Serial.printf("RSSI: %d dBm\n", WiFi.RSSI());
}
```

## 結果

- DHCPで `192.168.0.21` を取得
- RSSIはおおむね -54〜-62dBm
- Wi-Fi接続に成功
