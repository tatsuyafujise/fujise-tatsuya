# 進捗報告
## やっていること
### 新たなアルゴリズムBOBYQA を使って実験をやり直す.
[BOBYQAのライブラリ](https://github.com/numericalalgorithmsgroup/pybobyqa)

- これまでのアルゴリズムではMOEA/D より良いベジエ単体が獲得できなかった
- その原因がフィッティング先のB*の質のせいだと考えているため、現状のベジエ単体を用いてB＊を探索中に使用する優位性がなくなった.
### 仮説
- BOBYQAだけを使うよりベジエ単体をフィッティングした方が探索に必要なよき初期集団を獲得できる
## これからやること
### 以下のやり方で仮説を検証
- BOBYQA だけで最適化
- BOBYQAで初期集団を作ってから第二段のアルゴリズムを動かす
- 第二段のアルゴリズムだけで最適化
- BOBYQA で探索してB*を獲得しベジエ単体をフィッティングし，初期集団を生成
### 現在,梅木さんに環境構築してもらっています。