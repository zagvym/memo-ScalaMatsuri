Scalaz人間
==========

@xuwei-k

Scalazとは
----------

https://github.com/scalaz/scalaz

- ScalaでHaskel likeなプログラミングをするためのライブラリ
- 型クラスを徹底的に利用
    - 継承関係が重要

- scalazは道具
    - これを使うことが問題ではない
    - FPが必要になったときに使う
- まずはFunctional Programming in Scalaを読むべし
    - 訳: Scala関数型デザイン&プログラミング ―Scalazコントリビューターによる関数型徹底ガイド
    - Scalazの前に心の準備

### 型クラス

- gakuzzzさん資料: https://gist.github.com/gakuzzzz/8d497609012863b3ea50

異なる型に共通のインターフェースを付けたい

- trait のmixinではダメなのか
    - mixinはクラスの定義時にしないといけない。既存のライブラリとうまく連携できないことがある（immutable class, final class）

#### implicit conversion

※ implicit conversionは型クラスの特殊形


I/O まわり
----------

副作用の扱い

- DBコネクションとかどう扱うべきなのか
    - A: なんでもかんでもI/Oモナドにくるんだだけでは、見た目だけ関数型っぽくなるだけ意味が薄い
    - 難しいのは、手続き型でも問題になるデータサイズや効率性の部分をどううまく扱えるようにするのか
    ｰ コネクション部分が型クラス？ それを構成していって、副作用のある部分を最後に


いつScalazが欲しくなるのか
--------------------------

- Foldable + Monoid
    - 標準だと、Monoidがないので複数の巨大な値を取り出したいときに難しい。
    - 複数のMonoidにしておくと、あとで合成できる
- Validation

- [独習Scalaz](http://eed3si9n.com/learning-scalaz/ja/) 0日目が、中級ScalaプログラマとScalazの橋渡し

### Monoid

foldLeft でたたみ込める中身がMonoid

※ Scalaz 使っている人のスライドとかで、Scalazのコードを普通のScalaコードに逆変換している場合があるので、それを参考にするとか

### Contravariant Fanctor

Playの中に入っている？ 意識せずに使っているかも

Playの中にもScalaz/Catsと同じものが入っていることがある (play.functional パッケージ)

map の逆バージョン、`onWrite()`が定義されているクラスであれば、JSON変換時によしなにやってくれる？

Q. 継続モナドでいいのでは？


なぜ仕事でScalaz/Catsを使わないのか
------------------------------

- Teamの事情
- モジュール化されていれば入れられる？
    - Monoid/Foldable とか単体なら、全体のプログラミングスタイルを大きく崩さない
- イメージの問題
    - 知らないから怖くて入れたくない？
- OOP との兼ね合い
    - ScalazがSubtyping Ploymorphismと相性が悪い


まとめ
------

「Scalazがわからない」ではなく、「Scalazのここがわからない」と具体的に発信する。
勉強会を促す