# 進捗報告
## 行っていること
-  新しいアルゴリズムの実装中     

## これからやること
- 実験を回す
- 資料作成

### アルゴリズムの概要
- ベジエ単体bの制御点の初期化
- while 終了条件まで繰り返す
    - weight_set を random に n 個生成
    - X = [b(weight) for weight in weight_set]
    - f(X) を計算
    - X' = X + sigma
    - f(X') を計算
    - スカラー化関数の平均が小さかったら探索を成功としてstep-size を大きくする．
    - archive に f(X) と f(X') を追加
    - archive に対して non-dominated-sort をして，Bとする
    - weight を計算
    - B にベジエ単体のフィティング

 ![new theorem](image.png)

