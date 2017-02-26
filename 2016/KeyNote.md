基調講演: Refactoring in Scala
==============================

@gakuzzzz さん

Professional Null Cleaner

slide: https://t.co/MtTi7m7DE5

### Type of Value

#### TaggedType(scalaz), NewType

```scala
type UserId = Long
type Tagged[A, T] = {type Tag = T; type self = A}
type @@[UserId]
```

コンパイル時にinline展開、wrap/unwrap実行時コスト削減。

#### Value Classes

```scala
class UserId(val value: Long) extends AnyVal
```

#### Tagged Type or Value Classes?

前者はType Container有利？ valueTypeは型拡張性（メソッド追加など）。状況に合わせて使い分け。

https://twitter.com/xuwei_k/status/693243886316093440

#### Phantom Type

```scala
trait Id[A]
```

型検査だけ用にGenericsを使用して区別

http://akabe.github.io/2015/12/PhantomTypeTricks/


### レイヤー境界での型変換

WebフレームワークやRDBとの接続など

- ライブラリのサポート
    - Play2 のFormatterクラスなど
        - 自動的に変換できないものについては、ルールを記述して型変換を実施

=> 別レイヤーの機能へ二依存

#### Iso and Prisn

```scala
trait Iso[A, B] {
    def to(a: A): B
    def from(b: B): A
    // AからB, BからAは1:1のマッピング
}

trait Prism[A, B] {
    def to(a: A): Option[B]
    def from(b: B): A
    // A -> B は全射（例: 色名:A->RGB:Bコードマッピング）
}
```

IsoはPrismの特殊系（サブクラス）

レイヤ（フレームワーク）に依存しない変換ロジック -> ドメインロジックの独立性確保


#### Implicit Macro

- 公式のサンプルはIsoを定義
- MacroはExperimentalなので要注意

Q&A
----

- Abstract Type Member v.s. Type Alias
    - アクセスがPublicかどうか？

Memo
----

- https://t.co/WF9IruxzdQ