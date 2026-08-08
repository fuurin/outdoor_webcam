# Step 7: DeepSleepと1日周期の自動実行

状態: **未実施**

## 目的

起動、撮影、保存、通知の一連の処理後にDeepSleepへ入り、約24時間後に再起動する。

## 実施予定

1. Step 6までの処理を `setup()` 内で一度だけ実行する。
2. カメラ、microSD、通信モジュールを安全に停止する。
3. 24時間のタイマーウェイクアップを設定する。
4. `esp_deep_sleep_start()` を呼ぶ。
5. 再起動後にwake-up reasonを記録し、同じ処理が再実行されることを確認する。

```cpp
constexpr uint64_t SLEEP_SECONDS = 24ULL * 60ULL * 60ULL;
esp_sleep_enable_timer_wakeup(SLEEP_SECONDS * 1000000ULL);
esp_deep_sleep_start();
```

## 評価

- 起動からSleep移行までの時間
- 撮影・保存・送信の成功率
- DeepSleep中と処理中の電流
- 一周期の消費電力量

DeepSleepだけで周辺回路の待機電流が十分下がらない場合は、MOSFETやTPL5110による電源制御を次段階で検討する。
