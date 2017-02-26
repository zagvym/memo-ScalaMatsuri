Akka Streams による Kafka の Reactive 化
========================================

B会場

Krzysztof Ciesielski @kpciesielski , SoftwareMill, ScalaTimes

introduction
------------

旧 akka-stream-Kafka

akka-streamのストレージとしてkafkaを使う

Apache Kafka
-------------

distributed message broker, message log/queue

Topic (AWS Kinesis Stream?) - partition (Kinesis Shard?) 

producer -> topic -> consumer-group

clusterがconsumerグループのインスタンスウに応じて対応パーティションをバランシング（automatic or manual）

Decoupling（分離） producers and consumers:

=> マイクロサービス間を結ぶメッセージングフレームワーク

consumerによるmessage acknowledgemnet by commit => どこまで読んだ（処理したか）、Kinesis Checkpoint


Akka Streams
------------

- DSL for データ送信パイプライン
- Actor model
- implement Reactive stream specigication
    - Reactive stream実装の１つ

### features

- backpressure awareness
- testkit
- extensible
    - connectorを実装しやすい
    - reactive-kafkaもその1つ

### Alpakka

=> rich set of connectors

- HTTP
- Streaming TCP
- Streaming FILE
- S3
- ...

reactive-kafkaでは、KafkaがAkka-StreamのSourceになる

- Sink (Producer) を接続:  kafka consumer
- 必要に応じてFlow Producerも接続

この関連づけとバックグラウンドをAkka-Streamが担当


akka-stream-kafka
------------------

in akka ecosystems, well maintained

### example: plain consumer

Kafka Java API を直接呼ぶと、辛い（ループ、例外処理、backpressure考慮、メッセージのブロック化）

性能: java API v.s. Akka plain consumer

- スループットはJava API直叩きとほぼ同等、若干オーバヘッドあり

### example: committ consumer

requires: commitableSource or Batched Committable Source

性能を気にしてバッチ化すると、書くのが大変かつコールバックやLockを書かないといけない

akka-stream では、GroupedWithinで、例えば「100個ごともしくは5秒ごと」にBatch化などの記述サポート

at-least-once delivery

性能比: スループットはJavaAPI直より30%程度劣る

### example producer

as sink or as flow

From kafka to Kafka is possible by Producer as Flow

### example: streaming streams

streamに要素としてstreamを流す

kafka partitionごとにbackpressureをかける

### error handling

run() で Future を取得してエラー処理

`run(): [StreamControl, StreamFuture]`

`streamFuture.onFailure()` でエラー処理を設定

Kafka Streams
-------------

Apache Kafka: Java API, statefl processor, windowingm joinging, aggregation operators

akka-stream-kafkaとは別物