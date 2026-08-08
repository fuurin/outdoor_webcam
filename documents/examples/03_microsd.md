# Step 3: microSD読み書き

状態: **完了**

## 目的

AXP2101からmicroSDへ給電し、マウント、作成、読み込み、削除を確認する。

## 準備

Arduino IDEのLibrary Managerで `XPowersLib` をインストールする。

## スケッチ

```cpp
#include <Arduino.h>
#include <Wire.h>
#include <FS.h>
#include <SD_MMC.h>
#define XPOWERS_CHIP_AXP2101
#include <XPowersLib.h>

#define I2C_SDA 15
#define I2C_SCL 7
#define SDMMC_CLK 38
#define SDMMC_CMD 39
#define SDMMC_DATA 40

XPowersPMU PMU;

void setup() {
  Serial.begin(115200);
  while (!Serial) delay(10);
  Wire.begin(I2C_SDA, I2C_SCL);

  if (!PMU.begin(Wire, AXP2101_SLAVE_ADDRESS, I2C_SDA, I2C_SCL)) {
    Serial.println("ERROR: PMU initialization failed.");
    while (true) delay(1000);
  }
  PMU.setALDO3Voltage(3300);
  PMU.enableALDO3();
  PMU.disableTSPinMeasure();
  delay(100);

  SD_MMC.setPins(SDMMC_CLK, SDMMC_CMD, SDMMC_DATA);
  if (!SD_MMC.begin("/sdcard", true) || SD_MMC.cardType() == CARD_NONE) {
    Serial.println("ERROR: SD card mount failed.");
    while (true) delay(1000);
  }

  File file = SD_MMC.open("/hello.txt", FILE_WRITE);
  file.println("Hello LILYGO!");
  file.close();

  file = SD_MMC.open("/hello.txt");
  while (file.available()) Serial.write(file.read());
  file.close();
  SD_MMC.remove("/hello.txt");
  Serial.println("\nAll tests completed.");
}

void loop() {}
```

## 結果

- カード容量: 1,876MB
- 合計容量: 1,875MB
- 使用量: 1,636MB
- `hello.txt` の作成、読み込み、削除に成功

参考: https://github.com/Xinyuan-LilyGO/LilyGo-T-SIM7080G/tree/master/examples/MinimalSDCardExample
