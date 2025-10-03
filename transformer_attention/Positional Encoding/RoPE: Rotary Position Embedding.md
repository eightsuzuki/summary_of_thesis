# RoPE: Rotary Position Embedding

**出典**: RoFormer: Enhanced Transformer with Rotary Position Embedding — Su et al., 2021  
**リンク**: [arXiv:2104.09864](https://arxiv.org/abs/2104.09864)

---

### 概要
クエリ・キーを**次元ごとの回転（複素平面の位相回転）**で位置に依存させ、相対距離が位相差として内積に現れるよう設計。多くの LLM で標準的に採用。

---

### 仕組み（概念）
- 次元ペアごとに角周波数 \(\theta_i\) を設定し、位置 \(p\) に対し回転行列 \(R(\theta_i p)\) を適用。
- 内積 \(\langle R(\theta p) q, R(\theta p') k \rangle\) は位相差 \(\theta (p-p')\) に依存し、相対距離を表す。

---

### 特徴
- **長所**: 相対距離性を保持しつつ、実装は単純。多頭注意と相性が良い。
- **短所**: 超長距離では周波数設計・補間が必要。数値安定性の課題が報告される場合がある。

---

### 実装メモ
- 正弦/余弦のペア実装、または 2D 回転行列で実装。
- 長文拡張時は周波数の補間・スケーリング（xPos, YaRN など）と併用。

---

### 関連
- 長文安定化・補間: `xPos.md`, `YaRN: Yet Another RoPE Extension.md`
- 長文学習スキーム: `PoSE: Efficient Context Window Extension of LLMs via Positional Skip-wise Training.md`
- RoPE とコンテキスト理解の実証的分析: `Massive Values in Self-Attention Modules are the Key to Contextual Knowledge Understanding.md`
