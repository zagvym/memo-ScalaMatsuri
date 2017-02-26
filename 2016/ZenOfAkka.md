The Zen of Akka
===============

@ktosopl

Akka の Tips, アプリケーション作成時に基本とらる考え方

Tao（道）、Zen（禅）


Actor
-----

- Actorは受信と送信を担当
- 処理はワーカが担当、ワーカプールに流す

### Naming

機能に紐付いた名前を付ける。
ログに含めて特定できるような名前をつける。

Tips: Loggingの際は、プレースホルダ(ex. `"{} is {}, yah"`)にしておくと、出力条件を満たしたときだけ文字列作成でパフォーマンスに貢献

### Immutable

### Blocking

ブロッキングは避ける。

例: Future内でThread.sleepするなど
- スレッドプールDispacherを分けて、Dispacherごとにブロックできるように
    - 必要なブロック操作は、隔離して実行

タイムアウト付きNonBlockingFuture操作
    akka.afterでタイムアウト時間とそのときの動作をFutureオブジェクトにセット

### Serialize

Java Serializeは遅いので、できるだけ避けるべき。でもデフォルトはこれ

- 単に遅い
- XMLよりもでかい

ｰ Kryo, ProtocolBuffer などを使うべき

※ 推測せずに自分で計測せよ。プロファイリングツールを使え

### Let it Crash

ｰ Error: business logicの問題
    - なにかができていない
ｰ Failure（障害）: インフラの問題
    - 動作していない

### BackoffSupervision

例: DBのコネクションエラーの後に、各スレッドがそれぞれ再コネクト=>リクエスト地獄
    - 代表がそのあたりをコントロール

### Design using State Machine

FSM(Finite State Machine) trait or become メソッドで有限機械を作る

### Cluster convergence and joining

Cluster Gosip Convergence

Convergence: 各ノードがクラスタの状態について合意をとっている状態

Join -> weak-up -> UP

#### Weakly-up

Consistencyが損なわれる。(Eventually Consistencyになる？)

- Seed Nodes: 全員が知っているノード
    - Leader Electonで選出
    - リーダノードが参加を管理
        - Updateを行うSupervisor

もし、誰かに連絡が取れないとき、新規に追加ができない（全員の合意がとれないため）

そのとき、Weakly-upにしてとりあえず追加。（一時的に不整合。）
連絡が取れなかった人が戻ってきて、合意がとれたらUpになる

### Cluster Partitions and Down

Unreachableになったノードは、合意をとってDownさせる。

Unreacheableからは復帰できるが、Downと判定されたノードの復帰は許さない

single writer principle では必須。

#### Quarantined

Akka では不使用

### Akka is a toolkit

フレームワークではない。
便利なツール群を最小限提供する。抽象化を助ける。
    - LocalのFutureを分散環境で抽象化

用途に合わない道具は使うべきではない。Akkaに合う用途かどうかを見極める。