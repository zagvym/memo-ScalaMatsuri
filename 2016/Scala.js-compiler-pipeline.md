Scala.js compiler pipeline
==========================


### Scala JVM Pipeline

class ファイルをlinkしてJVM上で動作

コンパイラが標準ライブラリの場所を知っている

### Scala.js

.sjsir ファイルはclassファイル相当

動的リンクはできない。

jsinterop, jscode をコンパイラに追加

Compile Phase
-------------

code -> frontend () -> jsinterop -> backend -> jscode -> sjsir
                                             | -> jvm -> class

Scala.js Compiler Output: The IR
--------------------------------

IRはscala.jsコンパイル時の中間表現

### General

- AST form (typed)
- Complex expressions
    - 例: JavaScriptではブロックを変数に代入できない
- JavaScript operations

### Types

- No generics (like JVM, type erasure)
- Primitive types (like int)
- Class types

### Class/interfaces

- No overiding (instead name mangling)
- JavaScript Export


Scala.js Link Phase
-----------------------

Linker -> (IR Checker) -> Optimizer -> Refiner -> Emitter   -> Js code
                                                            -> (closure) -> minified js code

IR Checkerはデバッグ用（毎回オンにすると遅いため）

### Linker

不達コードを削除

### Optimizer, Refiner

複雑なコード排除して、出力コード量を削減

### Emitter

- Complex expressions: Blockスコープに注意しながら、同等のJSコードに展開
    - Q: なんで例のコードは括弧残っているの
        - プレゼンのため。外してかまわない括弧は実際には取り除かれる。
- Shadowingは？

- Q. JVMの例外とJavaScriptのエラーの違いをどう扱うか
    - A: Undefined behavior
        - JavaScriptの実行ができるようなコードになるように例外を扱っている

- Q. Scala.jsは実用的？
    - 200KBは必要になってくる
    - 実行速度に関しては生JavaScriptに近い
        - 遅くても2xぐらい
- Q. JsCDN的なもので標準ライブラリを提供する予定はあるか？ フロントエンドエンジニアだけで完結できるか？
    - 検討はされているが、まだない。当面無理そう
- Q. native呼び出し
- Q. Angular JS Digest, 双方向ダイジェスト
    - ?
- Q. 最近注力している機能
    - Scala.ks defined JS classes
    - デフォルトメソッドの追加
        - ユーザの方からはあまり見えないが、Scala-2.12移行時のバイナリ互換検査のため

Optional
--------

- Hijacked Classes
    - Stringはオブジェクトでも、javascriptではプリミティブ
- Additional Type
    - String
    - Array types()
    - Record types
- Enumeraton, Reflection, export overloading


Q&A
----

- Debug はどうするのか
    - SourceMapを使う