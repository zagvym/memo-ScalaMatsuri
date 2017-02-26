なぜReactiveは重要か
====================

@okapies さん

slide:https://t.co/roLTH7K9aT

現状
----

- 何でもかんでもRaeactive
    - frontend or backend or both?
    - * programming, * Stream, ...

Context
-------

### Frontend

- GUIの非同期イベント処理
- GUI + Network

### MicroService

### BigData

### Java9

Reactive Streams


What Reactive?
---------------

- Programming Model
- RuntimeEngine
- Architechture

↑現在のプログラミング課題を解決する手段の提案

Reactive component, Reactive data-flow

- データフローと変更の伝搬
    - 命令型との対比 -> データフローへの置き換え可能
    - データのフローと実行モデルの分離
        - 命令型
        - Reactive

データ駆動、イベント駆動

### Reactive component

- Only react to input
- 自己完結

### Reactice Data Flow

Reactive component を組み合わせた処理の全体像


Motivation
----------

### 非同期処理

- 従来
    - callback だとモジュラビリティが低い
        - 前後のcallbackとの暗黙の依存関係
    - callback hell
        - 深い入れ子

時間と入出力に対する依存を取り除きたい

### 並行・並列処理


Reacitive is Functional Programming?
------------------------------------

John Hughes, QuviQ, "Why Functional Programming Matters", 1984

- モジュール化のための道具（glue）
    - 遅延評価
        - コードを生成器(generator)と選択器(selector)に分割
    - 高階関数
        - ビジネスロジックとデータ型を分離
- "問題を解決する方法は解の貼り合わせに方法に依存"

非同期な文脈とビジネスロジックを分割

### FP glues to RP

- 宣言的な記法
    - What(データフロー記述) と How(Runtime)を噴火津
- Runtimeを置き換えることで1つのデータフロー（=ビジネスロジック）を使い回せる
    - 単体のサーバで動かす or 分散環境で動かす

- 例:
    - Akka
    - Science: TensorFlow
    - BigData: Hadoop like プラットホーム


Architecture
------------

分散コンピューティングが不可欠になりつつある、その複雑さに立ち向かう手段

Traits
-------

即応性
    - Elasticity
    - 耐障害性
=> データ駆動

非同期バイナリ境界


実現方法
---------

具体例: マイクロサービス、Spark

### Apache DataFlow

- データフロー記述を標準化
    - 準拠したランタイムで実行可能
        -

WebService Oechestration
-------------------------

DevOps


まとめ
------

プログラミングモデルはデータフローで記載、ランタイム（データフローをどう実行するか）の重要性が高まっていくのではないか。