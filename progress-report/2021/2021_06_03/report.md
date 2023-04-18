# 進捗報告
## skewMED問題で他のアルゴリズムでの実験（実装）
 https://github.com/drumehiron/MultiObjectiveOptimization/issues/1

jMetal を用いての実装
以下が実行できるように src/myutils/objectmetal.pyを変更した。

docker compose run -rm python pytest src/test/utils_objectmetal.py
## 変更内容
自作のクラスはそのままjMetalで実装されている問題のclassと合っていない。
https://github.com/jMetal/jMetalPy/blob/c3a7be2e135379377e7b8d9a303cf624cc339e46/jmetal/core/problem.py#L68


https://github.com/jMetal/jMetalPy/blob/c3a7be2e135379377e7b8d9a303cf624cc339e46/jmetal/problem/multiobjective/zdt.py#L15

上の２つを参考にしてsrc/myutils/objtojmetal.py の tojmetalprobremメソッドを修正した。

→コードの見方がわからなかったため参考の仕方がわからず梅木さんにほとんどやって貰う形に

def create_solution(self):の中身をself.lowerboundからself.upperboundの範囲でself.N個の乱数を取るようにした。

[(np.random.rand() - 0.5) * self.upperbound * 2 for i in range(self.N)]

https://github.com/tatsuyafujise/MultiObjectiveOptimization/blob/fujise/refacter/skewMED/src/objectivefunction/skewMED.py

## 読んでいるサイト

NSGA2についての記事

http://mikilab.doshisha.ac.jp/dia/research/mop_ga/moga/3/3-5-5.html

# 今後やること
割り当てられるタスク

NSGA2についての勉強

Pythonやシェルについての勉強

https://rinatz.github.io/python-book/

https://missing-semester-jp.github.io/