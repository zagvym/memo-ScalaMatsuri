Dotty と新しい Scala 開発エクスペリエンス
==========================================

- dotty: http://dotty.epfl.ch
- dotty-repos: https://github.com/lampepfl/dotty
- 資料: http://felixmulder.com/talks/the-new-scala-dev-exp/#/

introduction
-------------

> A languate is only as good as its tooling

###  scala rough edges (こなれてないところ)

- 落とし穴: code example
- newbie solutions
    - immutablity損失
    - lazy hell
    - delete trait
    - ...
- type inference (型推論)
    - わかりにくいエラーメッセージ
- 長いコンパイル時間
- ScalaDocのメンテナンスが大変
    
Dotty REPL =>  色分け、エラー分割、もしかして、エラー箇所、

`-explain` オプションで、ロングエラーメッセージ

コンパイルエラーメッセージに関してIDEがやってくれるようなことを、Dottyがやってくれる

### Compile

scalaでは型検査に時間がかかるが、さらに一番時間がかかるのは簡略化=simplifyの部分（24フェイズ）

#### Fused Phases

抽象構文木全体に対してphaseを適用するとキャッシュがでかくなりすぎてメモリに載らない

構文木を分割して各ノードに対して、一斉にphaseを適用？
マクロphaseに戻しながら？


### DottyDoc

HTMLジェネレータ [Jekyll](https://github.com/jekyll/jekyll)

dottydoc で Jekyll を出力して API ドキュメントを作成できる

Markdown形式

レポート生成: ドキュメントのカバレッジを出して褒めてくれる

テンプレートを自作可能

[Dottyのサイト](http://dotty.epfl.ch)はDottyDocで作っている

### Language Server

visualstudio code プロトコル（Opened） 

準拠することで、いろんなIDEとやりとりできる

### TASTY

Typed AST

バイナリ互換性への問題への解決策

コンパイラがTASTYがサポートしていれば、バイナリ互換性を担保できる

### developer usability

github issueの `help wanted` のものは、コントリビュート募集中, mentorがつく？


Q&A
---

- Q. TASTYができるとscala-native/scala-jsのコンパイラもTASTYに対応する？
    - native: 実装して欲しい
    - scalajs: 
- Q. Jekyllはrubyが必要では？ 今どうやっている？
    - ruby使っていない。
    - Scalaでtype safeなテンプレートを
- Q. DottyDocs で markdownになったけど、パラメータや関数はどうする?
    - 全部markdownで
    - scaladoc compatibility modeもある
- Q. パラメータが間違ってる場合に警告出る？
    - liquidでは想定していない
    - 今後そういうチェック入れたい
- Q. Dottyはいつリリース
    - 準備でき次第
    - loadmapは存在する
        - scala-2.13を先に出したい
        - scala-2.14がDottyとの橋渡し
        - マイグレーション用ツール準備中
- Q. 使ってきた感触は？
    - 今は使いやすい、sbt設定に2〜3行追記でできる
    - 安定化は優先事項。エラーレポート募集中
- Q. language ServerとEclipse(scala-IDE)との関係性は？ ensimeとの協業？ 
    - scala-IDEはマジカル、VS code protocol だけではカバーできないものがある
    - 共存可能