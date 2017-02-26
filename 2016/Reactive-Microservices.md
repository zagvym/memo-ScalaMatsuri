Reactive Microservices
======================

@huntchr (Tyesafe)

http://www.reactivemanifesto.org/ja

- Reactive must have
    - Resiliency
        - 障害に直面しても即応性を保ち続ける
    - Elasticity
        - ワークロードが変動しても即応性を保ち続ける
- Cache management accross multiple nodes

Example Program
----------------

Customer Service

Solution to make service Reactive
---------------------------------

- Cluster our DataStore
    - Postgres BDR
- Run Multiple nodes
    - Typesafe ConductR
        - リアクティブアプリケーションのクラスタ上でのデプロイ・管理
        - Akka ベースのマイクロサービスsupervisor
- Signal for cahce update
    - Reactive


[Typsafe ConductR](https://www.typesafe.com/products/conductr)
-----------------

- MicroService の各コンポーネントのデプロイ、起動終了、レプリケーションなどを管理
    - コンポーネント=Bundle (.zip)
    - Naming Service
        - AWSのサービスエンドポイントのように自動的に各Bundleへのアクセスポイントを提供

Play, Akka Cluster

Eventually consistency / Strong consistency, NoSQL or RDB


Q&A
----

- What is the difference ConductR vs Mesos
    - 裏側がMesos