AkkaStreams-Batch
================

@negokaz さん

Akka Streamsによるバッチの高速化

### 元構成

Rails-ActiveRecord-DB経由でデータを取得してCSV出力

ボトルネック: メモリのスワップ多発？
-> メモリ使用量を増やして再テスト
    -> CPUを使い切れていなかった

### 改善案

- Iterate、並列実行
- 時刻順にする必要があった => Akka Streams + Slick3

#### Akka Streams

Reactive Streams の Akka実装

並列処理、処理順はそのまま

Source -> Flow -> Sink -> Materialize

