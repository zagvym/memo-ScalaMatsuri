Spark: Japanese Text mining
===========================

- 前処理
- LDA(トピック抽出アルゴリズム)

### Vocabulary

- feature: 一つの項目に対する一つのパラメータ
    - 例:
- label: featureの
- Document: ID付きテキスト
- Model: 学習結果
    - 後で分析
    - データ適用
- Corpus

### Spark

- Hadoop, Mesosのような分散環境上で動作（ローカルで単体動作も可能）
- RDD: Resilient Distributed DataSet
    - Immutable データセット

### Text Mining

Hashing TF-IDF, 一般的な方法


LDA
---

前処理によってLDA行列を作成

### Word Segmentation

日本語の場合は単語区切り文字がないため、確率的なアプローチが必要

#### 形態素解析ライブラリ

- Mecab
- Kuromoji
    - Java製なのでSpark/Hadoopなどから呼び出しやすい
- ...

Amazon EMRとかで使いたいときはライブラリをどうするのか（最近はカスタマイズが可能？）

### Corpusの変換 -> トピック抽出

Spark

### LDA

前処理を適切にすれば、機械学習実行コードは1~数行。ただし、LDAは時間がかかる。

#### Stop word

です、ます など、日本語のStop WordをVocabularyに含めてはいけない

英語のようにライブラリ・データセットが用意されていないので、Top50を取り除くなど、自前でやる

### 解析データの利用

学習結果モデルに新規の入力を入れて、分類

- 例: スパムメール or not


Word2Vec
--------

文書をベクトル空間に落とし込む

- (Man, Woman) like (uncle, aunt), (King, Queen)
- (King, Kings) like (Queen, Queens)

- ベクトルの重ね合わせなど、ベクトル系の演算が可能

小さいデータセットでやると、Overfitting (データセット局所性に引っ張られる)
※ それが面白い場合もある。一般を求めるか、特化を求めるか使い分け

- 新しい言葉のベクトルは求められない。（再学習が必要？）
    - 処理時に例外になるので、そのときは 助詞などもともと数が多くてどうでもいい言葉にfallbackするといい（経験則）


Q&A
----

- Word2Vec VectorConcatnationは数学的に証明されているのか
    - 証明されてはいない
    - 定量的にいい、という論文結果に由来
    - できたVectorがいいかどうかを評価する基準がない
        ｰ 結果を見て判断？
- キャッシュ大事
ｰ Mecab, Kuromoji の開発が活発ではないんじゃない？
    ｰ 新しい言葉に対して辞書を地道に更新していく
        - 形態素解析はアルゴリズムより辞書
    - unidic or ipadic