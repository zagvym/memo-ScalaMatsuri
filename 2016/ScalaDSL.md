DSL
===


External or Internal DSL
------------------------

- External
    - 自分で全部用意する
    - 全部コントロールできる代わりに大変
- Internal
    - Scalaなどの言語上に定義
    - Externalに比べて簡単、ただし制約多い
    ｰ 例: Java Annotation
    - ホスト言語の機能セットをより簡単に記載する方法を提供する拡張

※ トレードオフの関係

### in Scala DSL

- Internal
    - implicit conversion
    - ドット、各個の省略
...

### Parser/Parser Generator

- External: Parser generator tools
    - 形式言語で文法を定義して、Parserプログラムを生成
- Internal: Parser Library
    - ホスト言語のコードで文法を定義
    - ホスト言語のライブラリを呼び出し可能

### WorkFlow(External)

### WorkFlow(Internal)

### Shallow vs Deep embedding

- Shallow: ホスト言語のコンパイラから直接コンパイル
    - 例: myWhile() {}
- Deep: ASTを自前で作成or改変
    - 例: オペレータの追加
        - scala ではimplicit conversion

### Ease of use

depends on...

- Falimiliarity with host language
- need to access host language library

Internal DSLをScalaで書くのは現実的

Scala Macro
------------

Model -> Test Cases（Executable）

Demo: Java Collections (Iterator)


Q&A
----

- Internal DSL in Scala でやる場合、Implicit conversionを多用するとAutoCompletionが効かない場合がある。見落とされがちだ。
    - A: Syntaxの記述をしっかりと整備する必要がある