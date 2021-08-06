# 進捗報告

## やったこと

### アルゴリズムの調査

- 多目的アルゴリズムの１つであるmoead について細かく調べてわかったことをまとめた。
[まとめた内容](https://github.com/drumehiron/MultiObjectiveOptimization/issues/14)

  - 上の内容の反省点
    - 擬似コードで書いたほうが相手に伝わりやすい
    - 参考にした記事においては一次情報のものを用いたほうが良い

### シェルの勉強  
[missing-semester 第２項](https://missing-semester-jp.github.io/2020/shell-tools/)  
  - ワイルドカード  
  何らかのワイルドカード検索をかけたい場合、?と*はそれぞれ1文字および任意の文字数にマッチする。

  - 波括弧 { }  
  複数のコマンドにおいて共通の部分文字列があった場合、波括弧を使ってbashに自動的に展開させることができる。

  ## これからやること
### 実験結果の分析
[issue](https://github.com/drumehiron/MultiObjectiveOptimization/issues/15)
-  実験結果の分析でグラフを作る
- アルゴリズム毎に、問題を変えると性能がどうなるかを比較
### 探索中の非劣解集合の実験データを得る
[issue](https://github.com/drumehiron/MultiObjectiveOptimization/issues/16)
- MOEA/D と評価回数毎の探索効率の計算
  - GDX
  - IGDX
  - GD
  - IGD
  - HV

