Spark/Deep learning による侵入検知
==================================

Skymind CTO, adam さん, 日本在住 (日本office)

introduction
------------

銀行向け、既存企業向けのRawログからのネットワーク侵入検知

異常値検知

深層学習を実際の問題解決に使えるということを見せる

リアルタイム検知

Network intrusion
-----------------

- いかに侵入してくるHackerを見つけてくるか
- 望ましくないパケットを弾く
- 内部から、人間による

今回はネットワークログから検知する

Streaming Processing

Web Log: Web Serverが残している、クライアントからのリクエストログ

どうやってトラフィックをマイニングして、対策するか

Annormaly detection
-------------------

昔から行われてきた。例: 異常な銀行入金

"finding a needle from haystack"

### 適用例: Simbox fraud

フランスの通信業者@アフリカ

通信業者の持っているネットワークインフラにただ乗り、不正侵入

どうやって正常でないトラフィックを遮断するか

数10億円？規模

通信ログに対して、深層学習を適用

CDR

ベクトルのreplicationによる検知


Deep learning
-------------

似ているパターンを見つけてくる

representation learning: 自分のパターンを把握？

もともと、アドテク(Googleなど) or Amazon や Netflix のrecommendation

Deep learning 利用時は何かしらのパターン（モデル）を見いだそうとする

例: クリックトラッキング、ユーザ行動分析

Deep Learning が侵入検知、異常検知に向いている・適用できるのでは？ と考えた

同じようなデータセット・学習だが、見つけたいモデルが違う

共通するのは、タイムスタンプ


Hacker detection Example
------------------------

Akka, Play, Kafka

### processing flow (Data engineering)

入力からニューラルネットに入れられるようにデータを加工

1. Storage/DataSource
2. Data Spout: 後続達にデータを渡す, routing
    - Data queueing or Pub/Sub
    - 目的ごとに複数の後続処理がある（同じデータに複数の利用形態が考えられる）
3. Data link: データのdumping, archiving, cleaning
    - 生データはdirty, noisy
    - 例: useragentの偽装検知のため、正規化。データサイエンティストの仕事
4. Data formatting: 機械学習に利用しやすいようにデータ形式を変換
5. ベクトル化
    - aka: feature engineering
        - one hot 形式: all zero but found
        - 例: IE, Firefox, Chrome のうち、どれが来たかを01で表す
            - IEがきた => 001
            - Firefox => 010
            - Chrome => 100
            - else/nothing => 000

時間の8割はデータエンジニアリング（データ収集と整形）にかけなければならない

JVMとScalaが機械学習の8割を占める。実はPythonじゃない

### Machine Learning

ニューラルネット例: LSTM, Google Translation

入力されたtime seriesでパターン認識

Time slice: 1回の学習に使うデータの期間
attribute: データレコードカラム
 
生データを使ってそこに隠れているパターンを見つける
feature enginnerringをしなくて良い？

representation learning

backpropaggation, or ...

ニューラルネットによる自己再構成

auto encoder

再構築エラーを入力に戻す？ 継続的に監視

深層学習+統計的手法で自動的な侵入検知を実現


ニューラルネットのConfigurartionしたいなら専門用語の理解が必要

モデルを使うところはリアルタイム、事前のトレーニング（モデル作り）を時間をかけて行う


demo
----

Play, microservice, updatable

Data visualization

IP address, timestamp, geolocation, 攻撃の確率（精度？）, 重篤度

リアルタイムで判定、攻撃確率と重篤度が高いと判定されたものを優先的に対処するなど



Q&A
----

- なぜ生データのクリーニングが必要
    - 必ずしも必要ではない
    - やらなくても隠しパターンは見つかるけど、使いづらいモデルになるかも
        - 例えばIE=>1, Chrome =>2 みたいに勝手に割り振ってくれるわけではない。人手で解釈してラベル付け？
- Modelをどう作る？
    - Mini Batch learning