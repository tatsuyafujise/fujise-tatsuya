# 進捗報告

## やったこと

### skew-MED問題でさまざまなアルゴリズムの性能評価 

[対応したissue](https://github.com/drumehiron/MultiObjectiveOptimization/issues/8)  

- notebook で GDX、IGDX、HV が出力されるように実験スクリプトを書いた。

- [実験スクリプト](https://github.com/tatsuyafujise/MultiObjectiveOptimization/blob/fujise/experiments-skewMED-MOEAD/src/experiments.py)

    - HVは非劣フロントと参照点の間で囲まれる領域の面積を表す。  
    よって、計算時に参照点を設定する必要がある。  
    →参照点は(1,1)で設定した。

    [参考記事](https://www.jstage.jst.go.jp/article/iscie/23/8/23_8_165/_pdf)


- [HVのソースコード](https://github.com/jMetal/jMetalPy/blob/c3a7be2e135379377e7b8d9a303cf624cc339e46/jmetal/core/quality_indicator.py#L109-L257)  

- [GD,IGD のソースコード](https://github.com/jMetal/jMetalPy/blob/c3a7be2e135379377e7b8d9a303cf624cc339e46/jmetal/core/quality_indicator.py#L49-L90)  
GDX ,IGDX を計算するために使用  
目的空間を解空間に置き換えて計算

### missing-semester

- 第Ⅰ項の終了

https://missing-semester-jp.github.io/2020/course-shell/  
  

## これからやること

- https://missing-semester-jp.github.io/  のⅡ項  

- 上のスクリプトを元に MOEA/D で実験