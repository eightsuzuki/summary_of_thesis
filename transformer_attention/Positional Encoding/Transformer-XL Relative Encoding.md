# Transformer-XL Relative Encoding

**出典**: Transformer-XL: Attentive Language Models Beyond a Fixed-Length Context — Dai et al., 2019  
**リンク**: [arXiv:1901.02860](https://arxiv.org/abs/1901.02860)

---

### 概要
セグメント間の再帰（メモリ）と相対位置表現を組み合わせ、固定長を超える長距離依存を学習・推論時に維持。位置表現は内容バイアスと位置バイアスを分離して導入。

---

### 仕組み（ロジットの分解・概念）
ロジットを概念的に
\[
q_i^\top k_j \;\;+\; q_i^\top R_{i-j} \;\;+\; u^\top k_j \;\;+\; v^\top R_{i-j}
\]
の和として扱い、内容（content）と位置（position）の寄与を分離。セグメント境界をまたぐ情報はメモリとしてキャッシュして参照。

---

### 特徴
- **長所**: 実質的なコンテキスト延長、長距離依存の改善。
- **短所**: 実装・計算の複雑化（メモリ管理、勾配の取り扱い）。

---

### 実装メモ
- 推論ではメモリ（過去キー・バリュー）をキャッシュし再利用。
- 学習では適切なセグメント長・メモリ長のスケジューリングが重要。

---

### 関連
- 相対位置の原型: `Relative Positional Encoding (Shaw 2018).md`
- 長文外挿の軽量代替: `ALiBi: Attention with Linear Biases.md`, `RoPE: Rotary Position Embedding.md`

