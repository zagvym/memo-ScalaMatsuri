ストリーミング・ワールドのためのサバイバル・ガイド
==================================================

会場B

Konrad Malawski @ktosopl ポーランド

introduction
------------

Akka Stream

Akka ちゃん

### history

- 2013
- v0.7 2014
- v2.0 2016
- 2017 ~
    - Reactive Stream on JDK9 (Flow)

### Akka

toolbox for concurenncy and distribution

- concurrency : Actor
- persistency : Event pile

### What's Stream

- 無限のデータセット
- procesed element by element
- Async
- Scalabiliy

### Fast Data

v.s. Batch processing arch

for BigData archtecture

### Where does akka stream fit?

- 低レイテンシを求められる ”Streaming” 処理

#### v.s. Kafka:

直接競合するものではない。
Kafka topicをAkka StreamでConsume

#### v.s. Spark

#### v.s. Java Stream

its not Stream but collection

#### v.s. Java Flow

JDK 9, Reactive Stream

### Reactive Manifest

Akka がすべてカバーする

ref: オライリー「Why Reavtive」, for free


road to Reactive
----------------

Not a quite reactice -> reactive

部分的にReactiveでも、全体がReactiveでないとサービスは "Reactive" にならない

例: Single point failue, 一部サーバへの負荷の偏り

circuit breaker だけでは解決しない問題がある。（Failureを扱うには良い手段）

バックエンドが処理可能かどうか「当てずっぽうをやめよう」

https://reactive-streams.org implements reactive stream.

API -> SPI (Service Provider Interface)

### back-pressure

- like TCP, application level flow control

story: Play, Akka-IO, RxJava

#### case: Push model

- Fast publisher and Slow subscriber
    - drop message after sub's buffer over flow ?
        - memory is not infinity
    - retry again and again?

#### pull-based-backpressure

各subscriber(consumer)が処理可能なペースでメッセージを受信可能

### Reactive goals

1. avoid infinite buffer
2. inter-op ...

### backpressure as a future

Elixirなどにportingされている


implementation
---------------

proper static typing -> どう実現するか

Source -> Flow -> Sink (terminal point like File or DB, ...)

Akkaは個別のアプリケーションの要件すべて満たすようにつくられていない。
代わりに細かいBuildingBlockを提供、それを組み合わせて個別の要件を満たす

Alp akka: コミュニティ for stream connector, Opened
    - Kafka, MySQL, ...

### type of Protocols (example)

HttpServer: Flow[HttpRequst, HttpResponse]
HttpEntity: Source[ByteString, ...]

TCP: Flow[ByteString, ByteString]

WebSocket

### Akka stream implement example: 

IoT: 工場内機械の状態監視、機械学習

座標、加速度などのデータがCSVなどで流れてくるとする

### composability

`Source.via(Flow)`
`Flow.via(Flow)`

### Ad

lightbend のmonitoringツール -> Akka Streamを可視化

### JDK9

現行のものとメソッド名は互換、importするパッケージの切り替えで対応可能


Wahts next Akka?
----------------

- Akka 2.5.x
- Alpakka connectors
- Akka HTTP/2
    - Play-2.6 Akka HTTP backend coming soon
- Akka Typed

Better AbstractActor