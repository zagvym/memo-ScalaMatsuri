Akka cluster with autoscaling
=============================

途中参加

introduction
------------

### 構成

Front autoscaled

backend

Redis Elasticache

S3 Snapshot 

運用上の問題
------------

### unreachable cluster node の除外

- ライフサイクル
- reader node はすべてのノードにメンバシップ情報が行き渡った後にreader actionを実施する
    - unreachableが1台でもいると、次のreader actionができない
- scale-in イベントをフックしてクラスタノード数を更新する

#### unreachable になる原因と対処

- ネットワーク分断 => 経過待ち
- machineクラッシュ => scale-in対応
- actor 停止
- 性能劣化

fallback メカニズムが重要

Split Brain Resolver (commercial), stragtegy は昨日のスライドを参照

#### reset cluster strategy

シードノードを固定して、新規クラスタを作成して、全部移す

ダウンしたノードは移らないので、クラスタ健全性が保たれる

アプリケーション再起動に伴うダウンタイム等に注意

### Journaling

Akka Persitence EventSource相当

そのkeep cleanigが結構大変

本来、イベントストアはイベントの完全な履歴を持つ、という想定

この会社の様にキャッシュ的用途では、本来用途から外れるのでサポートが弱い


Q&A
---

- Q. Unreachableメンバが増えても、Quorum数など適切に設定できればいいのでは？
    - AutoScaling環境では、staticに決めるのが難しい？
    - 割合計算するなど、作り込めば可能かも
    - twitter: https://twitter.com/TanUkkii007/status/835690211925360640
        - AutoScalingグループ以外など、ロールごとににstatic Quorumを設定可能な模様