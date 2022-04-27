# テーマ案：パラメータ付き最適化問題のための最適化法の提案
## 背景
- 最適化問題の定式化は難しい
  - 例：食品の製造プロセスの最適化
    - 最適解は気温/湿度など外部パラメータに依存する（はず？）
  - 例：制約付き最適化問題
    - 制約関数の設計が手間
      - 得られる解によって，制約の強化/緩和を検討したい場合など
- 複数の目的関数から最適化問題が構成される場合
  - 多目的最適化に帰着可能
- しかし上記の例の場合，多目的最適化には帰着できない
  - 問題の定式化の拡張が必要
---
## 問題の定式化
- パラメータ付き最適化問題
  - 設計変数：$x \in \mathcal{X}$
  - パラメータ：$\alpha \in \mathcal{A}$
  - 目的関数：$f: \mathcal{X} \times \mathcal{A} \to \mathbb{R}$
- 最小化問題
```math
x^\ast(\alpha) = \mathop{\rm arg~min}\limits_{x \in \mathcal{X}} \, f(x, \alpha)
```

### やりたいこと
- 各パラメータ$\alpha$に対応する最適解$x^\ast(\alpha)$がほしい
  - ただし，パラメータが変わるごとに最初から最適化を行うのはコストが大きいため不可
- 最適化の目標
  - パラメータ$\alpha$から$x^\ast(\alpha)$を返すモデルの獲得
​
#### 例：食品の製造プロセスの最適化
- パラメータ：気温，湿度，製造量など1日ごとに設定
  - 状況に応じた最適化が可能
  - 食品工場やチェーン店でのプロセス自動化など（いろいろ応用場所はありそう）
#### 例：多目的最適化のスカラ化関数
- パラメータ：重みベクトル
- スカラ化関数の最適化を行う手法の拡張が良い？
  - その場合はパラメータが$M-1$単体上にないとうまくいかない（？）
---
## アルゴリズム
- 新しい問題設定なので，基本的な手法が望ましい
  - 性能改善は別の研究になるので，基本的な手法での性能評価が必要
- 今のところ，2つの案が存在（案1の方が良いかも）
- 多目的最適化法を参考にアルゴリズムを構成しても良い（案3）
  - パラメータ空間によっては，適用できない場合もありそう
  - 多くの多目的最適化法のスカラ化関数は，重みが$M-1$単体上にあることを仮定しているため（正しい？）
​
### 案1：ガウス過程回帰を用いた最適化
- ガウス過程回帰は回帰モデルの1つ
  - パラメータ$\alpha$を与えると，最適解$x^\ast(\alpha)$を返すモデルを考える
    - 最適化中に利用するモデルは最良評価値$\min_{x} f(x, \alpha)$を返すモデルでも良いかも
    - 最終的に返すモデルは$x^\ast(\alpha)$を返すモデル
      - このモデルのみベジエ単体で獲得しても良いかも
  - モデルの分散が大きい点を次の評価点に設定
​
```python
initialization() # 初期化
f = get_objective_function() # パラメータ付き目的関数
ite = 0
archive = [] # 評価済みの解の保存用配列
​
while not teminate_condition():
  # 初期フェーズ
  if ite < init_phase:
    # パラメータをサンプリング
    alpha = sampling_parameter()
    # パラメータのもとでの最適解の計算（任意の最適化法）
    best_eval, best_x = optimize(f, alpha)　
    # リストに保存
    archive.append((alpha, best_eval))
  else:
    # フィッティングによりモデルを獲得
    gaussian_process = fititng(archive)
    # ベイズ最適化の枠組みで次のパラメータを計算
    next_alpha = gaussian_process.get_next_parameter()
    # 最適化法の初期値をモデルを使って設定（計算コスト削減のため）
    optimizer = get_optimizer(init_solution = bezier(next_alpha))
    # パラメータのもとでの最適解の計算（任意の最適化法）
    best_eval, best_x = optimizer.optimize(f, next_alpha)  
    # リストに保存
    archive.append((next_alpha, best_eval))
  ite += 1
​
# フィッティングにより最終的なモデルを獲得
gaussian_process_all = fititng(archive)
return gaussian_process_all
```
​
### 案2：ベジエ単体を用いた最適化
- ガウス過程回帰の代わりにベジエ単体を利用
  - 新しく評価するパラメータはサンプリングするしかない？（要検討）

```python
initialization() # 初期化
f = get_objective_function() # パラメータ付き目的関数
ite = 0
archive = [] # 評価済みの解の保存用配列
​
while not teminate_condition():
  # 初期フェーズ
  if ite < init_phase:
    # パラメータをサンプリング
    alpha = sampling_parameter()
    # パラメータのもとでの最適解の計算（任意の最適化法）
    best_eval, best_x = optimize(f, alpha)　
    # リストに保存
    archive.append((alpha, best_eval))
  else:
    # パラメータをサンプリング
    next_alpha = sampling_parameter()
    # フィッティングによりベジエ単体を獲得
    bezier = fititng(archive)
    # 最適化法の初期値をベジエ単体を使って設定（計算コスト削減のため）
    optimizer = get_optimizer(init_solution = bezier(next_alpha))
    # パラメータのもとでの最適解の計算（任意の最適化法）
    best_eval, best_x = optimizer.optimize(f, next_alpha)
    # リストに保存
    archive.append((next_alpha, best_eval))
  ite += 1
​
# フィッティングにより最終的なモデルを獲得
gaussian_process_all = fititng(archive)
return gaussian_process_all
```
​
### 案3：スカラ化関数を用いた多目的最適化法に基づく手法
- スカラ化関数の代わりに$f(x, \alpha)$を利用
- パラメータ$\alpha$の定義域が$M-1$単体でない場合はうまく行かない？
```python
initialization() # 初期化
f = get_objective_function() # パラメータ付き目的関数
alpha_list = set_parameter() # パラメータのリスト（梅木くんの手法と同じで最初に生成）
optimizer = get_optimizer() # 多目的最適化法
ite = 0
​
while not teminate_condition():
    # 解生成
    next_x_list = optimizer.get_candidate_solutions(sample_size)
    # 評価
    solution_list = []
    for x in next_x_list:
      for alpha in alpha_list:
        f_eval = f(x, alpha) # 目的関数での評価
        solution = (x, alpha, f_eval)
        solution_list.append(solution)
    # 多目的最適化法の更新
    optimizer.update(solution_list)
    # リストに保存
    archive.append((next_alpha, best_eval))
  ite += 1
​
# フィッティングにより最終的なモデルを獲得（ベジエ単体など）
gaussian_process_all = fititng(archive)
return gaussian_process_all
```
---
## 評価実験
- ベンチマーク関数自体から作る必要がある
  - bi-level optimizationのベンチマーク関数が利用できるかも（要調査）
- 制約なし，制約付きの場合に2通りあっても良いかも
  - 内部の最適化法を変える必要がある
---
## TODO
- 同じ問題設定を提案している関連研究がないかの調査
- ガウス過程回帰，ライブラリの勉強
  - https://github.com/SheffieldML/GPy
  - 日本語の解説記事も色々
    - https://statmodeling.hatenablog.com/entry/how-to-use-GPy
- ベジエ単体のフィッティングライブラリの使い方の勉強（案2の場合）
  - 梅木くんのコード，下記のライブラリのサンプルを参考
  - Git：https://github.com/rafcc/pytorch-bsf
  - Document：https://rafcc.github.io/pytorch-bsf/
- 提案手法の実装
- ベンチマーク関数の実装