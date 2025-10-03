# YaRN: Yet Another RoPE Extension

**出典**: YaRN: Efficient Context Window Extension of Large Language Models — Peng et al., 2023  
**リンク**: [arXiv:2309.00071](https://arxiv.org/abs/2309.00071)

---

### 概要
RoPE の周波数設計と補間戦略を工夫して長文への外挿を改善。学習長を超える推論長で注意の有効スケールを保ち、品質劣化を抑える。

---

### 仕組み（概念）
- 低・高周波成分のバランスを取りつつ、長距離でも位相差が過度に急峻/鈍化しないよう補間。
- 実運用の推論長に合わせたスケーリングルールを規定。

---

### 特徴
- **長所**: 既存 RoPE モデルに対して比較的軽量に適用可能。長文品質の改善が一貫して報告。
- **短所**: 最適ハイパラはモデル・データ依存。外挿限界は依然として周波数設計に制約される。

---

### 実装メモ
- 事前学習モデルの RoPE 周波数表現に補間を適用。推論長に応じてスケーリング係数を選択。
- xPos など他方式との比較・併用を検討。

---

### 関連
- 基礎: `RoPE: Rotary Position Embedding.md`
- 近縁: `xPos.md`
- 学習スキームによる延長: `PoSE: Efficient Context Window Extension of LLMs via Positional Skip-wise Training.md`
