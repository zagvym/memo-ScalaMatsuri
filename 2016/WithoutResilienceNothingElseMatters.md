Without Resilience Nothing Else Matters
=======================================

@jboner

レジリエンスが無ければ、他は無いも同じ

ロッキーはFalit Tolerance、not resilience
ふらつきながらも倒れない、というのはResilienceではない

すぐに元にもどる能力が必要

Complicated is not complex （煩雑と複雑は違う）

複雑なシステムは劣化していく
複雑なシステムは壊れたまま動き続ける。

人がかかわるとシステムは複雑化する

どうやって立ち向かっていくかを考え続けていく

Resilienceは設計方針、後から付け加えられるものではない

- 例:
    - 洪水に耐え抜いた家
    - ミーアキャット
        - 社会性
            - トップのカップルのみ子孫を残せる、他のグループメンバはそれのサポート
            - サポート先に斥候させる

simplicity -> [gather] -> complexity -> resilience

タマネギ型Security
    我々は階層的な保護を受けている

障害は例外ではない。アプリケーションの一部、ステートマシンの一部、以下にそこから素早く抜け出すかを考える

Self-Sustainability

### let it crash

- Crash Only Software (COS)
    - stop = crash safelu
    - start = recover fast

COSを再帰的に適用


### State Tar Pit

- input data: 外部入力、失ったら取り戻せない
- derived data: 計算によってできたデータ

Synchronised Dispatchは結合がきつい、途中で死んだらみんな死ぬ


### Isolation

障害の原因は、事故ったパーツではなく、関係性である

Failire need to be:
1. contained (dont cascade)
2. refeieed: メッセージ化
3. signal: 送信
4. ...

Bulkhead Pattern: 船の事故防止のための隔壁パターン、複数の頑強な隔壁で区切っておく

封じ込めるだけでなく、自己回復、通知

supervisorが必要

### Think Vending machine

ミル引き機能つきのやつ

コーヒー豆が切れたら、客にエラーを見せるのではなく、サポートベンダーに送る
クライアントには進捗と対処法を通知

操作ミスなど、クライアント側に責任があるときのみエラーメッセージを表示

### Error Kernel

Onion-Layer: Applicationのコアに到達する前に、複数のレイヤーでチェック。

エラー状態管理の多層化

各階層にSupervisor（親）がいて、子の処理を監視。
あるノードが死んだら、その親が責任をもって子を蘇生し、孫がゾンビにならないようにする

Summary
=======

Complex systems always run as broken.

悪いことではないし、本質であるから変えられない。
それを踏まえてシステムを作るべき。
