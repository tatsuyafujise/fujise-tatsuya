# 進捗報告
## 現在取り組んでいること
- Bilevel最適化のベンチマーク問題で，パラメータごとの最適解が計算できている論文を読んでいます
        
    - [論文](https://arxiv.org/abs/1303.3901)

## 今後の方針

### 論文を参考にしてベンチマーク関数を作成する
- 現状のベンチマークでは最適解のみパラメータに線形に依存
- 関数形状がパラメータに依存するベンチマークの構成
- sin,cosなどの非線形変換を用いたベンチマークの構成
- `objective_function/benchmark.py`のクラスを参考
