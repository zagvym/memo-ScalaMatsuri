新サービスをゼロから開発してローンチするのに大切だった3つのこと
===============================================================

C会場

Yoshimasa Niwa @niw

[資料](https://speakerdeck.com/niw/lessons-learned)

introduction
------------

Lessons Learned in launching product in Twitter

3つとは言っていない

Product: 1つの機能、製品

twitterさん参照

- GIF Search
- 画像ステッカー

### At glance

- History
- Arch
- Implement
- Executions


Twitter History
----------------

### 当初のアーキテクチャ

- 巨大な Ruby on Rails, Monorail
- grep(1) 頼み
- deployがつらい

Monolithic to MicroService
    -> Reweite each modules in Scala
        - システムを止めない
    -> Compose each small service: 分解と再構成

### 複雑さとどう向き合うか

- 分割統治: devide-and-rule
    - railsのService (request => response) -> Scala function
    - 現実セカイでは厳しい
        - 時間軸が入ってくる
        - e.x. search: load ... success/failed
- Functional Archtecture
    - `Future[+A]`
        - 当初はScala本体にはなくtwitterのutil
    - `Service[-A, +A]`

### `Future[+A]`

- 結果があることを期待する
- Composable
    - join, map, for, ... Monad like? 
    - 流れるようにコードを書ける


Implementation
--------------

- define -> resolve -> implement -> instanciate -> discover

### Define

- Thrift IDL
    - Scrooge: Thrift code generator in Scala: (IDL -> Scala)

### resolve

- Mono repo (Git改)
    - サービス/チームごとに細かくレポジトリを分けると破綻につながる
        - ダイヤモンド依存になりがち
- Git改造
    - 巨大なレポジトリを扱いやすい
    - バイナリ化してMySQLに突っ込む? 高速clone
- Pants (Python ants): build system
    - 大量のビルドタスクを

### implement

- Finagle, Finatra
- Mesos, Aurora DSL(Apache Aurora)
    - Pythonでみんなが好き勝手書きすぎてしまう
        - 複数の
- Zookeeper
    - Announce=Aurora
    - Resolve: Finagle

### Execute

- Team
    - (P)rojectManager, (D)esigner, (E)nginerr
- Plan: P & Define
- Design: P & E
- Implement: D & E

#### P v.s. E

- E: Minimize tech debt: 技術的負債の最小化
    - まずは上から下まで1回作る
        - いい設計をのこして悪い設計を除外
    - Run devel instance
        - Finagle Dtab 
            - port, addressを実行時差し替え
                - /s/gifsearch : エンドポイント
                - -> /s/prod/gifsearch : 本番系
                - -> /s/devel/gifsearch : 開発環境用
                - -> ...
            - サービスへのDNS的な動き

#### D v.s. E

- 以下に例外を減らすか
    - ユーザの予想外の動きへの対処
- Manage Internal State
    - User -> Action -> State Cahnge -> Update -> User ...
    - 逆方向に行かないように

#### E v.s. E

- 別チームの人とのやりとり
- コードを読め、コード=唯一の真実
    - ドキュメントはきっと古い
    - Mono repoが効く
- 名前をちゃんとつければそれがドキュメント/コメント代わりになる
- 正しいインタフェース設計
- Downstream の時間の管理
    - タイムアウトとリトライの調整
    - QPSとLatency
    - QPSとLoadBalancer
- Upstrean の時間の管理
    - CancelledRequestException
        - 上（前）から下（後）処理からの通知
        - 例外というよりイベント
        - 上側の設定ミス


lessons learned
---------------

1. まず素早く全体をつくる
2. 内部状態を意識する
3. コードを読む