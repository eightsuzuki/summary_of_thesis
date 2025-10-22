# Absolute Positional Encoding (sin/cos)

**出典**: Attention Is All You Need — Vaswani et al., 2017  
**リンク**: [arXiv:1706.03762](https://arxiv.org/abs/1706.03762)

---

### 概要
固定の三角関数基底により各トークンの絶対位置を符号化し、トークン埋め込みに加算する非学習方式。位置ごとの表現は周期関数の組み合わせで、周波数は次元ごとに幾何級数的に変化。

---

### 数式（定義）
埋め込み次元を \(d\)、位置を \(\mathrm{pos}\)、偶数/奇数次元インデックスを \(2i, 2i+1\) とすると、
\[
\mathrm{PE}(\mathrm{pos}, 2i) = \sin\!\left(\frac{\mathrm{pos}}{10000^{2i/d}}\right),\quad
\mathrm{PE}(\mathrm{pos}, 2i+1) = \cos\!\left(\frac{\mathrm{pos}}{10000^{2i/d}}\right).
\]

---

### 特徴
- **長所**: パラメータ不要、安定、任意長の位置に拡張可能（定義上）。実装が極めて簡単。
- **短所**: 相対距離の直接表現はなく、非常に長い位置での外挿は周波数設計に依存。学習で最適化されないため表現柔軟性は限定的。

---

### 実装メモ
- トークン埋め込みに単純加算する（入力層あるいは各層での加算）。
- 周波数スケール（10000）や次元割当は一般に固定。モデルサイズに合わせた調整余地はある。

---

### 関連
- 学習型の絶対位置表現: `Learned Positional Embedding.md`
- 相対位置・長文拡張系: `Relative Positional Encoding (Shaw 2018).md`, `Transformer-XL Relative Encoding.md`, `ALiBi: Attention with Linear Biases.md`, `RoPE: Rotary Position Embedding.md`

