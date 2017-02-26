モノリシックからリアクティブへ - 大切なのはアーキテクチャ
=========================================================

James Roper @jroper

introduction
------------

Lagom + reactive

Lagomを使ったebay cloneのデモ

Live coding: オークション

もともとモノリシックに作られたものをMicroserviceへ

全てをサービスに分割して、ゲートウェイを置く

障害があってもUXを低下させないようにする

### マイクロサービスの弊害

- 可動部分が増える
- 一貫性を保つのが難しくなる
    - 同期性
- request/responseがそれぞれ即応性を保つ必要がある


遅延の伝播への対処
------------------

- 例: 入札サービスが遅延すると、商品サービスが正常でも遅延しているように見える

一部サービスの遅延が全体の遅延につながる => UXの悪化

### 対策1: circuit breaker

Gatewayが遅いサービスへのブレーカを上げる => タイムアウトになる

ユーザから見るとエラーが返ってきて、結局UXが悪い

### 対策2: Failure Recovery

事前にワークアラウンドを設定して、障害時にリカバーできるようにする。

（成城可動部分だけでできる限りの結果を貸す）


一貫性喪失への対処
------------------

想定外の挙動、矛盾のある結果、ダブルブッキングの発生など、UXに問題が起きる

サービス境界を超える（またがる）トランザクションを同期処理でやってはいけない

すべてのメッセージに対して即応性を担保する必要性はない
=> eventual consistencyなど、制約を緩める（イベントソースから状況を再現可能であればよし、とするなど）

### 対策3: EventStream

LagomではKafkaのTopicでEventStreamを実現

順序、一貫性を保証

非同期EventLogベースにすることで、あとから古い壊れたイベントを回復できる？

### 対策4: Denormalize（非正規化）

RDBの正規化では、同じレコードを重複させない。

逆に非正規化では、【必要に応じて】レコードの重複保存を許す

EventData Persistence

重要な情報は他のサービスにpushして重複保存

システムにとって重要なのかビジネスにとって重要なのかの見極め

サービスをまたいでデータを複製しておくことで、一部のサービスが停止した際にもサービス全体の停止を避けられる

まとめ
------

- Monolithicからマイクロサービスへの移行は、データフローの再設計が必要
- 障害(failure)と一貫性(consistency)に対する気配り
- Use
    - Circuit breaker
    - avoid Failure degradation （障害の影響範囲の最小化）
    - Asynchronous messagging
    - data Denormalization（データ重複保持）

lagomframework.com

[demo on github](https://github.com/jroper/microservices-architecture-scala-jp)

Q&A
---

- Q: scala userがlagomを使うモチベーションは？
    - A. Microserviceを使うガイドラインを提供
        - hot reload, deployなどもサポート
- Q: loagomとPlayFrameworkの関係
    - A. Web通信のところはPlay, 後ろはLagom
        - LagomはマイクロサービスのREST基盤
        - lagomはPlayの上に載っている
    - Typed Persistent, [Swagger](http://swagger.io)サポート予定