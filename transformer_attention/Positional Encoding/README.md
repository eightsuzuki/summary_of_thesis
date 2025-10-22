# Positional Encoding 総まとめ（系譜と比較）

本ノートは Transformer 系における Positional Encoding（位置表現）の主要系譜と、長文対応に関する発展（拡張・補助手法）を簡潔に整理する。

---

### 系譜（年順）
1. **Absolute Positional Encoding (sin/cos)** — 固定の三角関数基底で絶対位置を符号化  
   参考: [Vaswani et al., 2017](https://arxiv.org/abs/1706.03762)
2. **Learned Positional Embedding** — 位置ごとの埋め込みを学習  
   参考: BERT [Devlin et al., 2018](https://arxiv.org/abs/1810.04805), GPT-2 [Radford et al., 2019](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf)
3. **Relative Positional Encoding** — トークン間の相対距離を明示的に表現  
   参考: [Shaw et al., 2018](https://arxiv.org/abs/1803.02155)
4. **Transformer-XL Relative Encoding** — セグメント間の再帰＋相対位置で長距離依存を保持  
   参考: [Dai et al., 2019](https://arxiv.org/abs/1901.02860)
5. **ALiBi (Attention with Linear Biases)** — 距離に比例する線形バイアスで“短学習・長推論”を可能化  
   参考: [Press et al., 2021](https://arxiv.org/abs/2108.12409)
6. **RoPE (Rotary Position Embedding)** — 複素平面回転で位置を相対位相として組み込む  
   参考: [Su et al., 2021](https://arxiv.org/abs/2104.09864)
7. **xPos** — RoPE のスケーリング安定化（減衰制御）  
   参考: [Sun et al., 2022](https://arxiv.org/abs/2212.10554)（付録）
8. **YaRN (Yet Another RoPE Extension)** — RoPE の周波数拡張と補間で長文汎化を改善  
   参考: [Peng et al., 2023](https://arxiv.org/abs/2309.00071)

---

### ざっくり比較（要点）
- **Absolute (sin/cos)**: 非学習・安定・外挿は弱め。実装容易。
- **Learned**: 分布内で強いが外挿に弱い。学習長の範囲外は不安定。
- **Relative / Transformer-XL**: 距離表現に強く長距離依存に有利。実装コストはやや高。
- **ALiBi**: 直線バイアスで“短学習→長推論”の外挿が堅牢。低コスト。
- **RoPE**: 広く採用。相対位相で距離を表現。長文は補間・周波数設計が鍵。
- **xPos / YaRN**: RoPE の長文安定化・拡張。スケーリングと減衰で外挿を改善。

---

### 長文対応の観点（実務指針）
- 学習資源が限られる場合は **ALiBi** や **RoPE+補間（xPos/YaRN 等）** が有力。
- 既存 RoPE 系モデルの文脈延長には **周波数補間・減衰調整**が効果的。
- メモリ/時間を増やさずに“学習時に長位置を見せる”発想として、
  - 学習スキーム: **PoSE（位置スキップ学習）** で固定ウィンドウ学習のまま長文汎化を獲得。  
    参考: [Zhu et al., 2024](https://arxiv.org/abs/2309.10400)

---

### まとめ（系譜のスナップショット）
- 2017: Absolute Positional Encoding — Vaswani+2017  
- 2018: Learned Positional Embedding — BERT（2018）, GPT-2（2019）  
- 2018: Relative Positional Encoding — Shaw+2018  
- 2019: Transformer-XL Relative — Dai+2019  
- 2021: ALiBi — Press+2021  
- 2021: RoPE — Su+2021  
- 2022: xPos — Sun+2022  
- 2023: YaRN — Peng+2023

---

### 関連ノート（このリポジトリ）
- RoPE と長文理解の挙動: `Massive Values in Self-Attention Modules are the Key to Contextual Knowledge Understanding.md`
- 位置スキップ学習による文脈拡張: `PoSE: Efficient Context Window Extension of LLMs via Positional Skip-wise Training.md`
