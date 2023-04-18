# 進捗報告
## 現在取り組んでいること
- パラメータに依存した制約をもつ制約付き最適化問題における最適化法の提案
    - 例：食品の製造プロセスの最適化
        - 最適解は気温/湿度など外部パラメータに依存する（はず？）

### 最適化の目標
-  各パラメータからそれに対応する最適解を獲得するモデルの作成
    - 例：食品の製造プロセスの最適化
        - パラメータ：気温，湿度，製造量など1日ごとに設定
        - 状況に応じた最適化が可能
        - 食品工場やチェーン店でのプロセス自動化など（いろいろ応用場所はありそう）

### アルゴリズム
- ガウス過程回帰を用いた最適化
    - パラメータ$\alpha$を与えると，最適解$x^\ast(\alpha)$を返すモデルを考える
        - 最適化中に利用するモデルは最良評価値$\min_{x} f(x, \alpha)$を返すモデルでも良いかも
        - 最終的に返すモデルは$x^\ast(\alpha)$を返すモデル
            - このモデルのみベジエ単体で獲得しても良いかも
  - モデルの分散が大きい点を次の評価点に設定
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
### 評価実験
- ベンチマーク関数自体から作る必要がある

## 上記と関連する研究背景，先行研究の説明
- [Bilevel optimizationでsurrogate modelを適用する手法](https://www.jstage.jst.go.jp/article/sicetr/56/5/56_317/_pdf/-char/ja)
    - パラメータの更新方法で差別化
- [Bilevel最適化のベンチマーク問題で，パラメータごとの最適解が計算できている論文](https://arxiv.org/abs/1303.3901)

## 今後の方針
### 実験の設定用のconfig fileの作成

### 実験ログを取るスクリプトの実装
- 評価指標の推移，評価した解とパラメータなどを保存
- config fileの保存も

### ベンチマーク関数の種類を増やす
- 現状のベンチマークでは最適解のみパラメータに線形に依存
- 関数形状がパラメータに依存するベンチマークの構成
- sin,cosなどの非線形変換を用いたベンチマークの構成
- `objective_function/benchmark.py`のクラスを参考
``` python
class NewBenchmark(ParametrizedObjectiveFunction):
    minimization_problem = True

    def __init__(self, 
                    d,                      # 次元数
                    max_eval=np.inf,        # 最大評価回数
                    param=None,             # パラメータ
                    param_range = [[-2,2]]  # パラメータの定義域
                ):
        super(NewBenchmark, self).__init__(max_eval, param, param_range)
        self.d = d          # 問題の次元数
        self.param_dim = 1  # パラメータの次元数

    def _evaluation(self, X, param):
        # (X, param)の評価値を返す
        return None
    
    def optimal_solution(self, param):
        # パラメータに対応する最適解（テスト用）
        # 計算ができなければNoneを返す
        return None
    
    def optimal_evaluation(self, param):
        # パラメータに対応する最良評価値（テスト用）
        # 計算ができなければNoneを返す
        return None
```
### ガウス過程回帰のカーネル，獲得関数の変更
- `experiment_template/experiment_train.py`を参考（下記の部分でカーネルは変更可能）
``` python
kernels_list = [RBF(f.param_dim), Linear(f.param_dim), Matern52(f.param_dim)]
```
- 獲得関数の変更は後回しでも
    - 現在は予測の共分散行列の行列式を獲得関数に利用（`model/gp/bo_acq.py`を参考）
### 実問題の実装・評価
- パラメータに依存した制約をもつ制約付き最適化
- 機械学習のハイパーパラメータ最適化
    - パラメータ：データセットの特徴量