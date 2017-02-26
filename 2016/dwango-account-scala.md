DwangoScala
===========

@pab_tech


継続モナド
----------

「継続」: 処理されていない処理

Webの認証処理って継続処理のシーケンス図に似てない？

Monad？

継続を受け取る関数を受け取ってMonad

### 例: 継続Monadによる認証処理

継続とFutureの合成で柔軟に処理を記述できる？

### 例: Playの場合

PlayのActionFunctionは継続だがMonadでない

合成

アカウントシステム
------------------

23万行のScala（半分はテスト）, PlayやSlickでも数万行

400以上のモジュール

### Minimal Cake Pattern DI

DI用に簡潔化したCake Pattern

MixIn Trait をうまく使う。
各クラスは他の実装を参照しつつ、自身も実装を持つ

高度なフレームワークは必要ない

#### 既存DI

Guiceなどは実績、サポートあり。実行時エラーの問題

#### Cake Pattern

もともとDIにかぎったものではない

- コンポーネント技術として利用
- Cake同士が密結合に
- 型とかクラスが複雑になりがち？

#### Reader Monad

FPの人たちの提案、ただし大規模開発でのDIに向かない？


Subtyping によるトランザクションMonad: Fujitask
===============================================

Storageアクセスの抽象化とトランザクションの合成

DBコネクションをReaderMonadに？

### DBのMaster/Slave (read replica) 構成のよくある問題

クライアント側がmaster/slaveどちらに接続するのか意識しないといけない

`trait Task` を実装して利用。数十行で実装可能？

共変・反変とimplicit conversionで工夫している

例：ScalikeJDBCを扱う例

Read TransactionとWriteTransactionを合成すると、RW Transactionになる。
Fujitask側で型毎にmaster/slaveどちらに行くか自動的に振り分けできる。

Summary
--------

Scalaの高度な言語機能をうまく使った


Q&A
----

- なんでそんなに巨大なのか
    - アカウント種別、レスポンスの組み合わせが膨大なので
- Phantom Typeを使うともっと簡単なんじゃない？
    - 現状で十分？
- 継続モナドってFutureを同期的に使えばいいんじゃない？
    - 8割ぐらいはFutureでいける
    - 2割の例外的な処理（差し戻しとか）が継続Monadだと書きやすい
    - Webコントローラだと、Futureのように前から合成ではなく後ろから合成した方が便利？