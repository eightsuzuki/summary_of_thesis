# Massive Values in Self-Attention Modules are the Key to Contextual Knowledge Understanding

**著者**: Mingyu Jin, Kai Mei, Wujiang Xu, Mingjie Sun, Ruixiang Tang, Mengnan Du, Zirui Liu, Yongfeng Zhang  
**公開**: 2025  
**リンク**: [OpenReview](https://openreview.net/forum?id=1SMcxxQiSL&noteId=7BAXSETAwU) ・ [Code (GitHub)](https://github.com/MingyuJ666/Rope_with_LLM)

本論文は、LLMの自己注意において、Query/Key の**特定次元に集中する大きな値（massive values）**が一貫して出現し、それが**コンテキスト知識（入力コンテキストからの知識）**の理解に不可欠であることを示す。これらの大きな値は、知識のパラメタ格納の検索よりも、コンテキストからの取り込み・解釈に寄与し、かつ **RoPE（Rotary Positional Encoding）** がその発生を誘発することを実証する。

---

### 1. 背景と主張
- 自己注意のスコアは \(\mathrm{softmax}(QK^{\top}/\sqrt{d_k})\) により計算されるが、実運用のLLMでは **Q/K の一部次元で極端に大きな値**が現れる。
- これらの大きな値を抑制・クリッピング・量子化すると、**コンテキスト依存なタスク**で性能が顕著に低下。
- 原因分析から、**RoPE** が初期層から当該挙動を誘発することを観測。

---

### 2. 数式の要点
自己注意（単一ヘッド）:
\[
\mathrm{Attention}(Q,K,V) = \mathrm{softmax}\!\left(\frac{QK^{\top}}{\sqrt{d_k}}\right) V.
\]
- 観測: \(Q\) と \(K\) の特定次元 \(j\) において \(|Q_{:,j}|\), \(|K_{:,j}|\) が他次元に比べて極端に大きい。
- RoPE 適用後の \(Q,K\) は回転変換 \(R(\theta)\) によって位相結合され、周波数成分と結合しやすい次元で**値の集中**が生じる。

---

### 3. 実験的知見（要旨）
- **量子化/クリップ**: 該当次元を重み付け抑制・量子化すると、文脈理解を要するQA/長文推論で大きく性能低下。逆に、パラメタ知識依存タスクの影響は比較的小さい。
- **RoPEの寄与**: RoPE を無効化・置換した場合、massive values の出現が弱まり、性能プロファイルが変化。
- **層位置**: 初期層から当該現象が観測され、上位層まで持続・累積する傾向。

---

### 4. 含意と実装ノート
- **設計含意**: 量子化・正規化・クリッピングを設計する際、当該次元の保持は**コンテキスト理解**に重要。無自覚な抑制は性能を損なう。
- **解析手法**: ヘッド/次元ごとの統計（分散・分位点）、RoPE周波数成分との相関、アブレーションを組合せて特定。
- **実務指針**: 
  - 量子化: ロス感度の高い次元に対し、**非一様**ビット割当や保護リストを導入。
  - クリッピング: グローバル閾値ではなく、**次元別/層別の適応的閾値**を検討。

---

### 5. 「キモ」と重要性
- **キモ**: 自己注意における Q/K の「極端値」次元はノイズではなく、**コンテキスト知識の理解**に機能的役割を担う。
- **重要性**: RoPE と massive values の機構的関係を示し、LLMの**量子化・最適化・解釈**に実践的な洞察を提供。

---

### 6. まとめ（要点）
- Q/K の一部次元に生じる massive values は、コンテキスト理解に不可欠。
- RoPE が当該現象を誘発し、初期層から表出。
- 量子化/抑制でコンテキスト系タスクが劣化するため、設計での**保全**が必要。

参考: [OpenReview](https://openreview.net/forum?id=1SMcxxQiSL&noteId=7BAXSETAwU) ・ [Code](https://github.com/MingyuJ666/Rope_with_LLM)
