# Learned Positional Embedding

**実装例**: BERT（Devlin et al., 2018）, GPT-2（Radford et al., 2019）  
**リンク**: [BERT arXiv:1810.04805](https://arxiv.org/abs/1810.04805) ・ GPT-2 ホワイトペーパー `https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf`

---

### 概要
最大長 \(L_{\max}\) までの各位置に対し学習可能なベクトルを割り当て、トークン埋め込みに加算する方式。データ分布内での表現力は高いが、学習長を超える外挿が苦手。

---

### 仕組み
- 位置埋め込み行列 \(E \in \mathbb{R}^{L_{\max} \times d}\) を学習。
- 入力列の位置 \(p \in [0, L_{\max})\) に対し、\(E[p]\) をトークン埋め込みに加算。

---

### 特徴
- **長所**: データ適応的で表現力が高い。分布内（学習長以内）で堅牢。
- **短所**: 学習長を超えると未知位置の埋め込みがなく外挿が弱い。\(L_{\max}\) 増加でパラメータ線形増。

---

### 実装メモ
- 事前学習の \(L_{\max}\) を運用要件に合わせて十分に確保するのが実務的。
- 事後に長さを伸ばす場合は、補間・再初期化・再学習のいずれかが必要。

---

### 関連
- 絶対・非学習: `Absolute Positional Encoding (sin-cos).md`
- 長文外挿志向: `ALiBi: Attention with Linear Biases.md`, `RoPE: Rotary Position Embedding.md`, `xPos.md`, `YaRN: Yet Another RoPE Extension.md`
