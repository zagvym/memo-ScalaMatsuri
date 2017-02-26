ChatWorkのScala採用プロダクト “Falcon” リリースまでの失敗と成功の歴史
=====================================================================

加藤 潤一 @j5ik2o, Backend担当エンジニア


技術的と言うよりストーリーベース

about Chatworks
---------------

- リリース後5年
- 導入: 12万社
- マルチデバイス対応
- ISO xxxx 認証

### データ規模

2015 -> 2016

- Chatroom: 2.4M -> 4.2M
- Messages: ...

### 当初のアーキテクチャ

- 社内の1プロジェクトとしてスタート
    - @2010
    - PHP framework
- 技術的負債
- データ量増加に伴い、システム構造上の問題

### なぜ "re-implementataion" したのか

- 納期の遅延など
- 構造上、言語上の限界？

### なぜ Scala にしたのか

かとじゅんさん入社前、合宿で決めた

- AWS上での運用 => JVMがいい
    - AWS SDK Java
- Chat-appに向いている？


Falconプロジェクト Phase1: Live Migration
-----------------------------------------

### 要件

- 既存コードの改修無し
- 無停止
- ...

### 戦略

- データのマイグレーションはしない
- 現行システムと並行運用

### チーム

- backend
    - Falcon: 新プロダクト(Migration)
    - Phoenix: 現行
- その他
    - Web
    - iOS

### Function Scope

- Chatroom 
    - ID Worker (新旧IDのマッピングなど)
    - ...

### Architecture

- Spray base
    - 非同期、Non-blocking
- 現行システムとの接続（腐敗防止層、Akka）
- iOSクライアントはFalconへ接続する想定
- Streaming
- Phoenixへのフィードバック

### Context Map of DDD

- Web -> phoenix -> falcon -> iOS
    - 上層からの遅延が下層（後続）に伝播
    - 後続のスケジュールがどんどんきつくなる
- 結合テストが難しい

### 発生した問題

- プロジェクト終盤で致命的な問題に気づく
    - DynamoDBのセカンダリインデックスとか
    - 結合テストが遅れてしまったのもある
- Scala以外の、性能や構成に関する問題

### Make a decision

- 2016年1月に一旦プロジェクト停止
- 振り返り
    - 良い結果をみつける
        - 自分たちの立ち向かっている問題の大きさに気づけた
        - よいチームを作れた
        - Akka, DDDの知見

### Rebooting the Project

- 人事: 開発本部長招聘
- 堅牢なアーキテクチャの選定
- POS (Proof of Concept) の義務づけ
    - 初期コスト（+時間）と将来的なクオリティ（+運用コスト、改修コスト）とのトレードオフ
    - やればいいってものではない
- 最終的な非機能要件、パフォーマンス要件の具体化
    - 目標の数値化
- Data Migration

### POCの設定

それぞれの理想のFalconを出し合って、最終的な目標を決める

- target
    - Scalabiliyy
    - Resilliency
    - 2倍のスループット
    - Low Cost
    - Functionality 

- Requirements
    - AWS
    - CQES + Event Sourcing
    - 責務分離

#### POC検証

構成:

- full Akka
    - akka-http, stream
- Infra
    - EC2, ELB
    - Lightbend ConductR
    - WriteDB: cassandra
    - ReadDB: AWS Aurora

- Akka Stream
    - cluster sharding
- Domainイベントを元に、非同期でReadDBに書き込み

C3.2xkarge

3 nodes, 2000users 

検証の結果、

- akka clusterを見送り（DataCenterが3カ所は欲しい、SplitBrain時のため）
- ClusterShardingも見送り, 運用コストとのバランスを見て、代替手段を採用
- Cassandra: レジューム/replicaが大変？ 見送り
- Aurora:
    - master ノードへの書き込み性能に難あり
    - Shardingで解決できるが、開発・運用が難しい


Falcon Phase2
-------------

### インフラ・ミドルウェア構成

- WriteDB: Kafka
    - append only でコスト比パフォーマンスが出しやすい
- ReadDB: HBase
    - master/slave構成が直感的だった
- ...

### チーム構成

- Facon: PHPフロントエンドからきたメッセージを処理するバックエンド
    - Kafka経由でSparrow forwarderにフィードバック
- Data Migration Team
- Sparrow: legacy service との連携 (PHP)

### ストレステスト

- POCから大きくズレなし
    - 現行の30~倍

### データマイグレーション

- 基本マイグレーション
    - 最終メンテナンス直前4日分
    - 3.5時間で16億メッセージ
- 差分マイグレーション
    - insert/update
- データ検証
    - ツールを利用して確認

### DevOps

開発効率向上のため

KubanetesのクラスタをAWS上に作成し管理するツールを作成 => coreos/kube-AWS

パイプラインベースCI: Csoncource

sbt-release, sbt-native-packager -> Docker Imageにしてデプロイ

### Finally Release

2016/12/29 深夜の7時間で実施、リリース完了

Conclusion
-----------

- 最終的にはリリースできた
- 問題はScalaではなく、その他の部分に由来するところが大きい
- AkkaなどScalaが成功要因になったり