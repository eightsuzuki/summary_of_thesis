# ALiBi: Attention with Linear Biases

**出典**: Train Short, Test Long: Attention with Linear Biases Enables Input Length Extrapolation — Press et al., 2021  
**リンク**: [arXiv:2108.12409](https://arxiv.org/abs/2108.12409)

---

### 概要
クエリ・キーの内積に、トークン間距離に比例する**線形バイアス**を加算。学習時の短い長さから、推論時により長い入力へ外挿できる挙動を促す。

---

### 仕組み（概念）
- 距離 \(d = i - j\) に対し、ヘッドごとに係数 \(m_h\) を持つバイアス \(b_{ij} = - m_h \cdot |d|\) を注意ロジットに加算。
- 遠距離ほど減衰を強めるバイアスにより、長さが伸びても注意のスケールが破綻しにくい。

---

### 特徴
- **長所**: 追加パラメータが小さく実装容易。短学習→長推論の外挿が強い。
- **短所**: バイアス形状が固定で表現柔軟性は限定。距離減衰一様性がタスクにより最適でないことも。

---

### 実装メモ
- ヘッドごとに異なる \(m_h\) を設定（事前定義または学習）。
- 相対距離の計算はブロードキャストで高速化可能。

---

### 関連
- 相対距離の学習表現: `Relative Positional Encoding (Shaw 2018).md`
- 回転位相ベース: `RoPE: Rotary Position Embedding.md`
