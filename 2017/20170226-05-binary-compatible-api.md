Binary compatibilityを意識したAPI設計
=====================================

セバスチャンさん

いつ互換性を気にするのか
------------------------

エンドユーザにアプリケーションをまるっと提供するなら（リコンパイル権限があるなら）、気にしなくていい

ライブラリとして提供する場合に意識が必要

複数のライブラリの異存がある場合に、一部のライブラリのせいで他のバイナリが動かなくなるかも


どうやってバイナリ互換性を維持するのか
--------------------------------------


ソースコード互換性維持よりもバイナリ互換性維持の方が簡単？ in Scala

例: implict param <-> explicit paramになったら？

### 壊れた状態とは

binary シグネチャが同じなのに定義が異なるものができてしまっている状態？

### case class 

case class はバイナリ互換性の天敵

"コードのデモは実際に使われているアプリケーションでやるべき"

悪の根源は、 `object#unapply()`, product, 

field追加すると互換性壊れてします

#### when to use

pattern matchで使う分にはいい

そうでなければ、あらゆる代償を払ってでも回避すべき？

### case class altanative

1. final class
2. private constructer
3. public field, all default value must be set
4. make copy() private

```scala
final class Config private (
    val f1: ...
) {
    private def copy() = ???
}

object Config {
    def apply(): Config = new Config
}
```

### ライブラリデベロッパの心構え

ライブラリ制作者が覚悟を決めて互換性を意識した実装頑張るとあとあと効いてくる


### デフォルトパラメータ

パラメータ群の末尾に追加する分には動くかも。間に入ると互換が壊れる？

#### 対策

override

### trick

```scala
@deprecated
protected[foo] def foo() = ???
```

~~互換性維持できないものを弾く？~~

Bytecodeレベルではpublicになる？ -> https://twitter.com/eed3si9n/status/835750967949697024



Q&A
----

- Q. 互換性維持ツール/プラグインでやるのは？
    - https://github.com/sbt/contraband
    - 余計に依存性を増やすというトレードオフ
- Q. マイナーバージョンアップとかの対策は
    - あるかも。今回はcase classの落とし穴への対処の照会
- Q. Scala.jsの互換性はLightbendのマイグレーションチェックで十分なのか？
    - Scala.js のケースでは十分？
        - classファイルがバイナリ互換なら、生成されるscalajs-irファイルも互換?
        - 逆は成り立たない
- Q. unapplyは本当に無理
    - 無理
- Q. case class の `._1` とかのアクセスにすれば
    - バイナリ互換性は保てても、ソース互換性が保てない