Panel Session: Reactive
=======================

TypeSafeが考えるReactiveとは？

Reactive Manifest

Reactive <-> Predictive

1.TIS
------

Q. ReactiveってResilient？ 金融とTwitterでは違ってくるんじゃない？

原則としてResilient、しかしどんなアプリケーションでも原則を守りきれるわけではない。

どんなシステムもResilientであるべき。

Scale Up/Down (Out/In) を実際に動いているシステムでやると遅いかもしれない。

金融とかテレコムシステムってReactiveという言葉が出る前からReactiveだったんじゃない？


Q. 例えば金融ではどういったところがReactive？ （どういう障害をうまく押さえ込めている）

事実に関するログ作成

分散トランザクションを求められるけど、実はそんなにトランザクションって多くない？
Factを正確にログに残していき、整合性を保てるようにしている。

Process のトランザクション

Akka は魔法の杖ではなく、適切な道具を提供すること

Reactive は目的ではく、成功のための手段


Actorモデルについて
-------------------

Actor便利だけどモジュール化できないという批判もある。その解決策がAkka (Akka Stream) ?

Q. Apache DataFlow とどう戦っていくのか

Spark, DataFlow と競合しているのではなく、補完的な関係

Streamという言葉があふれている。名前が同じだけどそれぞれレイヤが違う

Akka Stream のコアのFreatureは適切な Back Pressure

ノード間でできる限り高速かつ安全に（後続が処理できる負荷の範囲で）メッセージを送れるように注力している。

送信側・受信側両方で工夫

Akka Stream を利用しているエコシステムができあがりつつある。
信頼できるデータパイプラインとして様々なプラットフォームで使用できる。

例: Intel Gearpump

#### Back Pressure

サーバの扱える負荷を超えてしまう、分散システムの問題。

リクエストされたぶんだけメッセージを送るようにする。

### 分散Akka Stream

Lifted

本当に必要かどうか確認する。実は多段のPoint to Pointなんじゃない？

5回確認して、必要だと思ったら Gearpump を使う

- Stream のパイプライン、っていすのはそれぞれ別のセマンティクス
    - ポイント間で必要となる機能が必要な機能が異なる

### Reactive Socket


Streamの動的再構成
------------------

現状、静的に決まっているStreamを後からつなぎ替えるような方法の検討があるのか。

Akka Streamの目標とは違う。BackPressureの管理に注力していく

GearpumpはDynamicトポロジーをサポート？
    MillWheel (Google, 2013)

Google CloudDataFlow の裏はGearpump