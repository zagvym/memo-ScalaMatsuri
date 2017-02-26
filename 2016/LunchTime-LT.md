LunchTime LT
=============

## Dwango 新人研修テキスト公開

- https://github.com/dwango/scala_text


## Scala.js cross build

公式ドキュメント参照

slide: https://t.co/0XP9X18pca

Cross Type: Pure, Dummy（かぶり無し？）, Shared

## Dwango: gRPC(scalaPB)

ScalaPB: gRPCの野良実装

RESTに比べてProtocolBuffer利用のメリットを享受できる


## 障害対策: Failure wall

slide: https://t.co/C4EHGun4p0

依存システムの障害を考慮した実装を簡単に・・・

NetfrixのHystrixにインスパイアされた

エラー処理を内包したFutureを返す

- Retry, Semaphore, Circuit Breaker