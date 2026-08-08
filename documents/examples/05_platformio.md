# Step 5: VS Code + PlatformIO環境の導入

状態: **未実施**

## 目的

Arduino IDEで確認したコードを、依存ライブラリとビルド設定をプロジェクト単位で管理できるVS Code + PlatformIOへ移行する。

## 準備

- Visual Studio Codeをインストールする。
- VS CodeのExtensionsで公式の `PlatformIO IDE` を検索してインストールする。VS Code拡張にはPlatformIO Coreが含まれるため、Coreを別途インストールする必要はない。
- Git経由の依存関係を利用できるよう、ターミナルで `git --version` が成功することを確認する。

公式手順: https://docs.platformio.org/en/latest/integration/ide/vscode.html

## プロジェクト作成

1. VS Code下部のPlatformIO Homeを開く。
2. `New Project` を選ぶ。
3. Boardで `Espressif ESP32-S3-DevKitC-1-N8`、Frameworkで `Arduino` を選ぶ。
4. 作成されたプロジェクトで、実装を `src/main.cpp`、ヘッダーを `include/`、プロジェクト固有ライブラリを `lib/` に配置する。
5. Arduinoスケッチを移す場合は、先頭に `#include <Arduino.h>` があることを確認する。

ボードIDは `esp32-s3-devkitc-1`。LILYGOボード固有のFlash/PSRAM設定は汎用ボードの既定値と異なる可能性があるため、Arduino IDEで確認済みの8MB OPI PSRAM設定と一致することを実機で検証する。

## `platformio.ini` の初期設定

```ini
[env:outdoor-webcam]
platform = espressif32
board = esp32-s3-devkitc-1
framework = arduino
monitor_speed = 115200
build_flags =
    -D ARDUINO_USB_CDC_ON_BOOT=1
```

`XPowersLib`などの依存ライブラリは、採用するパッケージとバージョンを確認後、`lib_deps` に固定して再現可能にする。PSRAMやFlash関連の追加オプションは、ビルドと実機認識を確認しながら確定する。

Board設定: https://docs.platformio.org/en/latest/boards/espressif32/esp32-s3-devkitc-1.html

## 動作確認

1. Step 1のシリアル出力コードを `src/main.cpp` へ移す。
2. PlatformIO ToolbarのBuildでコンパイルする。
3. USB接続後、Uploadでボードへ書き込む。
4. Serial Monitorを開き、115200 baudで出力を確認する。
5. Step 4のコードを移し、PSRAMが8,388,608 bytesとして認識され、撮影とmicroSD保存が成功することを確認する。

CLIでは次のコマンドに相当する。

```sh
pio run
pio run --target upload
pio device monitor --baud 115200
```

## 完了条件

- VS Code上でビルド、書き込み、Serial Monitorが利用できる。
- Arduino IDEで確認したカメラとmicroSDのコードが同じように動作する。
- 必要なボード設定とライブラリ依存関係が `platformio.ini` に記録されている。

参考: https://qiita.com/tronicboy/items/fd7d5951dd49f95e1b6e
