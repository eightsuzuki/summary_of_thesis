# Relative Positional Encoding (Shaw et al., 2018)

**出典**: Self-Attention with Relative Position Representations — Shaw et al., 2018  
**リンク**: [arXiv:1803.02155](https://arxiv.org/abs/1803.02155)

---

### 概要
クエリ位置とキー位置の**相対距離**を埋め込みとして表現し、注意スコアおよび値の集約に反映。距離情報を直接扱うことで長距離依存の建設的なバイアスを導入。

---

### 仕組み（簡略式）
- 相対距離 \(r = i - j\) に対し学習埋め込み \(a_r\) を用意（範囲外はクリップ）。
- 注意ロジット: \( \alpha_{ij} \propto q_i^\top k_j + q_i^\top a_{i-j} \)
- 値の集約にも相対埋め込みを加味（\(v_j + a^V_{i-j}\)）。

---

### 特徴
- **長所**: 相対距離の一般化がしやすく、学習長外でも挙動が安定しやすい。
- **短所**: 追加埋め込み・距離クリップ処理が必要。実装・計算がやや複雑。

---

### 実装メモ
- 最大相対距離 \(K\) を決め、\([-K, K]\) で埋め込みを学習。\(|i-j|>K\) は端にクリップ。
- メモリ帯域に注意（相対位置テンソルのブロードキャスト）。

---

### 関連
- セグメント再帰と相対位置の拡張: `Transformer-XL Relative Encoding.md`
- 他方式: `ALiBi: Attention with Linear Biases.md`, `RoPE: Rotary Position Embedding.md`
