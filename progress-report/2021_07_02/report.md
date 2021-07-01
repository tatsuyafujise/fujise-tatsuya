# 進捗報告
## やったこと
### skew-MED問題でさまざまなアルゴリズムの性能評価  
https://github.com/drumehiron/MultiObjectiveOptimization/issues/8  

- notebookでGDX,IGDX,HVが出力されるように実験スクリプトを書いた。

- 実験スクリプト
https://github.com/tatsuyafujise/MultiObjectiveOptimization/blob/fujise/experiments-skewMED-MOEAD/src/experiments.py

    - HVは解集合と参照点の間で囲まれる領域の面積を表す。  
    よって、計算時に参照点を用意する必要がある。  
    →基準である(1,1)で上手くいった。
    https://www.jstage.jst.go.jp/article/iscie/23/8/23_8_165/_pdf

![Qiita](https://user-images.githubusercontent.com/31867338/123608951-34c59980-d83a-11eb-879a-7b6ec75c238e.png)  

- HV  
https://github.com/jMetal/jMetalPy/blob/c3a7be2e135379377e7b8d9a303cf624cc339e46/jmetal/core/quality_indicator.py#L109-L257  

- GD,IGD  
https://github.com/jMetal/jMetalPy/blob/c3a7be2e135379377e7b8d9a303cf624cc339e46/jmetal/core/quality_indicator.py#L49-L90  

## シェルの実践課題

https://missing-semester-jp.github.io/2020/course-shell/  
  

## これからやること

- https://missing-semester-jp.github.io/  のⅡ項以降の読み合わせ  