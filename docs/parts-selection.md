# パーツ選定メモ

## 目的

キャンプ場向けオフグリッド LTE カメラ端末に必要な主要パーツを比較・選定するためのメモ。ここに記載する候補は初期調査用であり、最終採用前にデータシート、国内認証、入手性、キャリア対応、実測消費電力を確認する。

## 選定対象

- マイコン / SBC
- LTE/4G 通信モジュール
- SIM スロット / eSIM
- カメラモジュール
- ローカル保存用ストレージ
- 電源制御基板
- バッテリー
- ソーラーパネル / 充電回路
- 防水筐体 / アンテナ / ケーブルグランド
- センサー類（電池電圧、温度、照度など）

## 評価基準

| 項目 | 重み | 確認内容 |
| --- | --- | --- |
| 低消費電力 | 高 | deep sleep 電流、通信時ピーク電流、電源断制御のしやすさ |
| LTE 対応 | 高 | 国内キャリア対応バンド、LTE-M/Cat 1/Cat 4、技適/認証、アンテナ |
| 画像品質 | 中 | 解像度、画角、HDR、暗所性能、固定焦点/AF |
| 実装容易性 | 中 | SDK、サンプル、Linux 利用可否、TLS/S3 対応 |
| 屋外耐性 | 高 | 温度範囲、防水筐体との相性、結露対策 |
| 入手性 | 高 | 国内購入可否、長期供給、代替品 |
| コスト | 中 | 端末単価、通信費、電池/保守費 |

## 用語メモ

- **SBC**: Single Board Computer の略で、Raspberry Pi のように CPU、メモリ、ストレージ接続、OS 実行環境を 1 枚の基板にまとめた小型コンピュータ。
- **BOM**: Bill of Materials の略で、製作に必要な部品名、数量、型番、単価、購入先をまとめた部品表。

## マイコン / SBC 候補

| 候補 | 特徴 | 懸念 | 初期評価 |
| --- | --- | --- | --- |
| ESP32-S3 系カメラボード | 低消費電力寄りでカメラ接続例が多い。小型で電源制御しやすい。 | S3 直送や HTTPS/TLS、画像バッファ、LTE 制御の実装難度を確認する必要がある。 | 低消費電力重視の第一候補 |
| Raspberry Pi Zero 2 W + カメラ | Linux が使え、S3/Slack 連携や画像処理が容易。公式カメラ資産が豊富。 | 待機電力と起動時間がマイコンより不利。Wi-Fi は使わないため不要機能が多い。 | PoC しやすいが長期電池運用は要実測 |
| STM32 / nRF52 + 外付けカメラ | 低消費電力設計に強い。 | カメラ、LTE、TLS、S3 連携の実装負荷が高い。 | 量産・専用設計向け |

## LTE/4G 通信モジュール候補

| 候補 | 特徴 | 懸念 | 確認タスク |
| --- | --- | --- | --- |
| Quectel BG95 系 | LTE Cat M1 / Cat NB2 / EGPRS、GNSS 統合、低消費電力用途向け。 | 画像アップロードに十分な実効速度か確認が必要。キャリア対応とファーム入手性を確認する。 | キャリアバンド、PSM/eDRX、HTTPS/MQTT/TLS、国内認証 |
| SIMCom SIM7600 系 | LTE Cat 1 系で画像アップロードに余裕を持ちやすい。 | LTE-M より消費電力が大きくなる可能性。ピーク電流と対応バンドを確認する。 | 消費電流、S3 送信方式、対応キャリア、技適 |
| LTE USB ドングル / HAT | Raspberry Pi PoC で使いやすい。 | 屋外・低消費電力・長期供給には不向きな場合がある。 | 起動時間、再接続性、Linux ドライバ |

## カメラ候補

| 候補 | 特徴 | 懸念 | 初期評価 |
| --- | --- | --- | --- |
| OV2640/OV5640 系 | ESP32 系で利用例が多く、低解像度〜中解像度の JPEG 撮影に向く。 | 画質、暗所性能、屋外の逆光耐性を確認する。 | 低消費電力 PoC 向け |
| Raspberry Pi Camera Module 3 | 12MP クラス、AF/HDR などを活用しやすい。 | Raspberry Pi 前提になり、電力面で不利な可能性。 | 画質検証・PoC 向け |
| Arducam 系 CSI/USB カメラ | センサーやレンズの選択肢が多い。 | ドライバ、互換性、消費電力、筐体固定方法を確認する。 | 画角・画質要件が固まった後に比較 |

## ローカル保存用ストレージ候補

| 候補 | 特徴 | 懸念 | 確認タスク |
| --- | --- | --- | --- |
| microSD カード | 入手しやすく容量が大きい。現地回収して PC で読み出しやすい。 | 書き込み中の電源断や低温環境で破損リスクがある。産業用グレードの検討が必要。 | ファイルシステム、電源断耐性、低温動作、容量上限 |
| SPI NOR Flash | 小容量だがマイコンから扱いやすく、設定や小さい画像の一時保存に向く。 | 写真を複数枚残すには容量不足になりやすい。 | 必要容量、書き換え寿命、画像サイズ |
| eMMC | microSD より実装固定しやすく、SBC で使いやすい場合がある。 | 部品選定と基板実装の難度が上がる。 | 採用ボード対応、耐久性、交換可否 |

## 電源・バッテリー候補

| 候補 | 特徴 | 懸念 | 確認タスク |
| --- | --- | --- | --- |
| Li-ion / LiPo + 保護回路 | 高エネルギー密度で入手しやすい。 | 低温性能、膨張、充放電保護、防水筐体内の熱対策。 | 温度範囲、保護 IC、充電方式 |
| LiFePO4 | 安全性とサイクル寿命に優れる傾向。 | 電圧レンジ、充電 IC、容量あたりサイズ。 | 電源レギュレータ選定、低温特性 |
| 一次リチウム電池 | 長期保管・低自己放電に向く。 | 充電不可、ピーク電流対策、廃棄/交換運用。 | LTE ピーク電流を支えるコンデンサ/電源設計 |
| ソーラー + 充電コントローラ | 電池交換頻度を減らせる。 | 林間サイトの日照不足、積雪、汚れ、盗難、設置方向。 | 日照見積もり、曇天連続日数、過充電保護 |

## SIM / 通信契約の確認事項

- 通信キャリアの設置場所カバレッジ
- LTE-M、Cat 1、Cat 4 のどれを使うか
- 1 日 1 枚の画像サイズと月間通信量
- グローバル IP の要否（通常は不要）
- 閉域網や固定 IP の要否
- SIM サイズ、SIM スロット、eSIM 可否
- 低温・屋外での SIM 接触不良対策

## 初期 BOM ラフ案

### 低消費電力優先 PoC

| 分類 | 候補 |
| --- | --- |
| 制御 | ESP32-S3 カメラ開発ボード |
| 通信 | Quectel BG95 系 LTE-M モジュール評価ボード |
| カメラ | OV2640/OV5640 系 |
| ストレージ | microSD カードまたは SPI Flash |
| 電源 | Li-ion/LiFePO4 + 電源制御 + 電圧測定 |
| クラウド | S3 + Lambda + Slack Webhook |

### 実装容易性優先 PoC

| 分類 | 候補 |
| --- | --- |
| 制御 | Raspberry Pi Zero 2 W |
| 通信 | LTE HAT / USB LTE モデム |
| カメラ | Raspberry Pi Camera Module 3 |
| ストレージ | microSD カード |
| 電源 | 大容量バッテリー + 外部 RTC 電源制御 |
| クラウド | Python + boto3 + S3 + Lambda/Slack |

## 低消費電力優先 PoC 購入リンク候補（Amazon）

2026-06-20 時点で Amazon.co.jp から購入候補を探した初期リスト。価格、在庫、販売元、技適/認証、付属品は変わるため、購入直前に商品ページとデータシートを再確認する。特に LTE モジュール、アンテナ、SIM、Li-ion 充電池は国内利用条件と安全性を必ず確認する。

### 要件に対応する主要パーツ

| 分類 | Amazon 候補 | 用途・確認ポイント |
| --- | --- | --- |
| マイコン / SBC | [ESP32-S3 ETH Development Board + OV2640 Camera](https://www.amazon.co.jp/-/en/Waveshare-ESP32-S3-development-Ethernet-processor/dp/B0DKJ7CMNW) | ESP32-S3 系カメラ開発ボード候補。PoC では Ethernet/PoE 部分は必須ではないため、カメラ接続、消費電力、GPIO 数、microSD 追加可否を確認する。 |
| LTE/4G 通信モジュール | [Waveshare BG95-M3 Zero / BG95 EVB 開発ボード](https://www.amazon.co.jp/QuecPython-%E7%94%A8%E3%81%AB%E8%A8%AD%E8%A8%88%E3%81%95%E3%82%8C%E3%81%9F-%E9%96%8B%E7%99%BA%E3%83%9C%E3%83%BC%E3%83%89%E3%80%81%E4%BD%8E%E6%B6%88%E8%B2%BB%E9%9B%BB%E5%8A%9B%E3%80%81LTE-BG95-M3-Zero/dp/B0D4V8MSB8) | Quectel BG95 系候補。LTE Cat M1 / NB-IoT / EGPRS、対応バンド、アンテナ、UART/USB 接続、消費電流、技適/認証を確認する。 |
| SIM スロット（物理） | [SIM カードソケットブレークアウトボード](https://www.amazon.co.jp/AYASOSO-SIM%E3%82%AB%E3%83%BC%E3%83%89%E3%82%BD%E3%82%B1%E3%83%83%E3%83%88%E3%83%96%E3%83%AC%E3%83%BC%E3%82%AF%E3%82%A2%E3%82%A6%E3%83%88%E3%83%9C%E3%83%BC%E3%83%89%E3%80%81%E3%83%94%E3%83%B3%E3%83%98%E3%83%83%E3%83%80%E3%83%BC%E3%80%81SIM%E3%82%AB%E3%83%BC%E3%83%89%E3%82%A2%E3%83%80%E3%83%97%E3%82%BF%E3%83%BC%E3%83%A2%E3%82%B8%E3%83%A5%E3%83%BC%E3%83%AB%E3%80%81GSM-GPRS-Arduino%E3%80%81Raspberry-Pi%E3%80%81DIY%E9%9B%BB%E5%AD%90%E6%A9%9F%E5%99%A8%E3%83%97%E3%83%AD%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E7%94%A8%E3%80%82/dp/B0G2J4KS9F) | BG95 開発ボードに SIM スロットが搭載されている場合は不要。別基板化や延長配置を検討する場合の候補。SIM 電圧、カードサイズ、配線長を確認する。 |
| カメラモジュール | [Aideepen OV2640 Camera Module 68° Lens](https://www.amazon.co.jp/-/en/Aideepen-Megapixel-Sensors-ESP-32CAM-STM32F4/dp/B0BVHPFW7J) | OV2640 追加・交換用候補。採用する ESP32-S3 ボードのカメラコネクタ、ピン配置、FPC 向き、レンズ画角との互換性を確認する。 |
| ローカル保存用ストレージ | [Micro SD TF Card Memory Shield Module](https://www.amazon.co.jp/-/en/5-Piece-Memory-Compatible-Arduino-Adapter/dp/B078NSBDDW) | ESP32-S3 から SPI 接続で画像を保存する候補。電源電圧、レベル変換、CS ピン、書き込み中電源断への耐性を確認する。 |
| 電源制御基板 | [AOD4184 Isolation MOSFET Module](https://www.amazon.co.jp/-/en/AOD4184-Isolation-Opticoupler-Raspberry-Solenoid/dp/B0G5NNG1VZ) | カメラ、LTE モジュール、ストレージの電源オン/オフ検証用。実運用では待機時消費電流、低サイド/高サイド構成、突入電流、電圧降下を再評価する。 |
| バッテリー | [KEEPPOWER 18650 充電池セット候補](https://www.amazon.co.jp/18650-%E4%BF%9D%E8%AD%B7%E5%9B%9E%E8%B7%AF%E4%BB%98%E3%81%8D/s?k=18650+%E4%BF%9D%E8%AD%B7%E5%9B%9E%E8%B7%AF%E4%BB%98%E3%81%8D) | Li-ion 充電池候補。PSE、保護回路、容量表記の信頼性、最大放電電流、充電器対応、低温特性を確認する。 |
| ソーラーパネル / 充電回路 | プロトタイプ製作では使用しない | まずは 1 サイクルの消費電力量を測定し、必要容量が見えてから検討する。 |
| 防水筐体 / アンテナ / ケーブルグランド | プロトタイプ製作では使用しない | 室内 PoC 後、屋外試験フェーズで防水、アンテナ配置、結露対策と合わせて選定する。 |
| センサー類（電池電圧、温度、照度など） | プロトタイプ製作では使用しない | 最初の PoC では必須にせず、電池電圧測定や温度ログが必要になった段階で追加する。 |

### プロトタイプ製作で追加購入を検討するもの

| 分類 | Amazon 候補 | 用途・確認ポイント |
| --- | --- | --- |
| microSD カード本体 | [Amazon.co.jp microSD カード検索](https://www.amazon.co.jp/s?k=microSD+%E3%82%AB%E3%83%BC%E3%83%89+32GB) | 画像保存用。容量は 32GB 程度から開始し、屋外運用では高耐久/産業用グレードを検討する。 |
| 18650 電池ボックス | [2 本用 18650 電池ボックス スイッチ付き](https://www.amazon.co.jp/-/en/Azuocn-Battery-Storage-2x18650-Container/dp/B09L54TCQ6) | ベンチ検証用の電池ホルダー。直列/並列、出力電圧、コネクタ形状、逆接続リスクを確認する。 |
| Li-ion 充電モジュール | [TP4056 Type-C 充電保護モジュール](https://www.amazon.co.jp/-/en/TP4056-Type-C-Charger-Charging-Protection/dp/B0D57XDQV3) | 単セル Li-ion の充電検証用。充電電流、保護回路、発熱、電池仕様との整合を確認する。 |
| ジャンパワイヤ | [Dupont ジャンパワイヤキット](https://www.amazon.co.jp/-/en/GTIWUNG-Breadboard-Male-Female-Female-Female-Multicolor/dp/B08LD6Z84R) | ESP32-S3、BG95、microSD、MOSFET モジュールの仮配線用。オス/メス混在セットが便利。 |
| ブレッドボード | [Amazon.co.jp ブレッドボード検索](https://www.amazon.co.jp/s?k=%E3%83%96%E3%83%AC%E3%83%83%E3%83%89%E3%83%9C%E3%83%BC%E3%83%89+Arduino) | はんだ付け前の仮配線用。LTE 通信時の大電流経路には不向きなので、電源ラインは太い配線や端子台を使う。 |
| USB ケーブル | [Amazon.co.jp USB Type-C ケーブル検索](https://www.amazon.co.jp/s?k=USB+Type-C+%E3%82%B1%E3%83%BC%E3%83%96%E3%83%AB+%E7%9F%AD%E3%81%84) | ESP32-S3、BG95、充電モジュールの給電・書き込み・デバッグ用。必要なコネクタ形状を各ボードで確認する。 |
| 外部電源アダプタ | [Amazon.co.jp USB AC アダプタ検索](https://www.amazon.co.jp/s?k=USB+AC%E3%82%A2%E3%83%80%E3%83%97%E3%82%BF+5V+3A) | 開発中の安定給電用。LTE モジュールのピーク電流に合わせ、5V 2A〜3A 程度の余裕を持つものを選ぶ。 |
| テスター | [Amazon.co.jp デジタルマルチメータ検索](https://www.amazon.co.jp/s?k=%E3%83%87%E3%82%B8%E3%82%BF%E3%83%AB%E3%83%9E%E3%83%AB%E3%83%81%E3%83%A1%E3%83%BC%E3%82%BF) | 電池電圧、配線確認、消費電流の簡易測定用。後続の電力測定では USB 電力計やロガーも検討する。 |
| SIM カード | [Amazon.co.jp IoT SIM 検索](https://www.amazon.co.jp/s?k=IoT+SIM+LTE-M) | LTE-M/Cat M1 対応、通信キャリア、月額費用、データ容量、APN、SMS 要否を確認する。 |

## 参考リンク（初期調査）

- Espressif ESP32-S3-EYE: https://www.espressif.com/en/products/devkits/esp32-s3-eye/overview
- Espressif ESP32-S3-EYE User Guide: https://documentation.espressif.com/esp-who/master/docs/en/get-started/ESP32-S3-EYE_Getting_Started_Guide.md
- Raspberry Pi Camera Module 3: https://www.raspberrypi.com/products/camera-module-3/
- Raspberry Pi Camera Module 3 Product Brief: https://datasheets.raspberrypi.com/camera/camera-module-3-product-brief.pdf
- Quectel BG95: https://www.quectel.com/product/lpwa-bg95-cat-m1-cat-nb2-egprs-series/
- SIMCom SIM7600NA: https://www.simcom.com/product/SIM7600NAG.html
- Particle Boron Datasheet: https://docs.particle.io/reference/datasheets/b-series/boron-datasheet/

## その他確認が必要な事項

### 電力設計の考え方

電池寿命は次の要素で大きく変わる。

- 待機時電流
- 起動時間
- LTE 接続確立時間
- アップロードに必要な送信時間
- 画像サイズ
- 電波状況
- 低温時のバッテリー性能低下
- ソーラー充電の有無と日照条件

初期段階では以下の測定を必須にする。

| 測定項目 | 目的 |
| --- | --- |
| deep sleep 電流 | 待機期間の消費電力を見積もる |
| 起動から撮影完了までの電流・時間 | カメラ処理の電力を見積もる |
| LTE 接続からアップロード完了までの電流・時間 | 通信処理の電力を見積もる |
| 低温時の起動可否 | 冬季キャンプ場での運用リスクを確認する |
| 電波弱環境での再送回数 | 山間部や林間サイトでの電力悪化を確認する |

### 未決定事項

- 設置場所の LTE キャリア別電波品質
- 必要な画角、解像度、夜間撮影要否
- ソーラーパネルを使うか、一次電池/二次電池のみで運用するか
- 運用目標期間（例: 1 か月、3 か月、1 年）
- 防水等級、耐候性、盗難/いたずら対策

## 次に決めること

1. 設置予定地で主要キャリアの LTE 電波を測定する。
2. 必要画質と画像サイズを決める。
3. 目標電池交換間隔を決める。
4. ソーラー利用の有無を決める。
5. PoC を「低消費電力優先」か「実装容易性優先」どちらで開始するか決める。
