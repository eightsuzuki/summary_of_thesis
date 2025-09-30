# Relative Attributing Propagation (RAP): Interpreting the Comparative Contributions of Individual Units in Deep Neural Networks

**著者**: Woo-Jeoung Nam, Shir Gur, Jaesik Choi, Lior Wolf, Seong-Whan Lee  
**公開**: arXiv 2019（AAAI 2020 採択）  
**DOI**: 10.48550/arXiv.1904.00605  
**リンク**: [arXiv:1904.00605](https://arxiv.org/abs/1904.00605)

RAP（Relative Attributing Propagation）は、各層間の「相対的な影響度」に基づき、ニューロンごとの関連性（relevance）を**正（relevant）と負（irrelevant）に分離**しながら保存則を満たすように伝播する手法です。従来のLRPに比べ、出力に対して相対的に重要／不重要なユニットを双極（bi-polar）スコアで明確に区別し、視覚的に明瞭な説明を提供します。

---

### 1. 新規性：この論文の貢献と既存研究との差分

- **相対影響度に基づく重み付け**: 絶対寄与に基づく正規化で、層間の「相対的優先度」を明示化。
- **双極スコア（bi-polar relevance）**: 正（高度に関連）から負（高度に非関連）まで連続的に割当。
- **保存則を維持**: 正・負を分離しつつ、合計関連性が層を跨いで保存されるように再正規化。
- **定量評価での優位性**: Outside-inside relevance ratio, Segmentation mIOU, Region perturbation で既存法を上回る結果。

---

### 2. 理論/手法の核心：数式できっちり定式化

#### 記法
- 下層ニューロン集合を \(J\)、上層を \(K\) とする。
- 活性 \(a_j\)、重み \(w_{jk}\)、線形寄与 \(z_{jk} = a_j w_{jk}\)。
- 上層関連性 \(R_k\)（総和 \(\sum_k R_k\)）。

#### 2.1 相対影響度による一次割当（absolute-contribution normalization）
各上層ユニット \(k\) に対し、下層ユニット \(j\) の相対影響度を
\[
\rho_{jk} \;=\; \frac{|z_{jk}|}{\sum_{j'\in J} |z_{j'k}|}\,\in[0,1], \qquad \sum_{j}\rho_{jk}=1.
\]
と定め、暫定関連性を
\[
\tilde{R}_j \;=\; \sum_{k\in K} \rho_{jk}\, R_k
\]
と集約する。これにより、層間で相対的に大きい寄与（|z|が大）のユニットに優先的に関連性が割当される。

#### 2.2 保存補正と双極分離（bipolar decomposition）
暫定関連性 \(\tilde{R}_j\) の総和は一般に \(\sum_k R_k\) と一致しないため、保存則を満たすように補正し、双極に分離する：
\[
R_j \;=\; \tilde{R}_j \; + \; \delta, \quad \text{where }\; \delta \;=\; \frac{\sum_{k} R_k - \sum_{j} \tilde{R}_j}{|J|}.
\]
このとき \(\sum_j R_j = \sum_k R_k\)。さらに、
\[
R_j^{+} = \max(0, R_j), \qquad R_j^{-} = \min(0, R_j), \qquad R_j = R_j^{+} + R_j^{-}.
\]
により、正（関連）と負（非関連）の双極スコアに分解する。

#### 2.3 符号分離した下位層への伝播
次層へは、正負を個別に正規化して保存則を保つよう分配する。例えば、正寄与について：
\[
R_i^{+} \;=\; \sum_{j\in J} \frac{(z_{ij})^{+}}{\sum_{i'\in I} (z_{i'j})^{+} + \epsilon}\, R_j^{+}, \qquad (x)^{+} = \max(0,x).
\]
負寄与は \((z)^{-}=\min(0,z)\) を用いて同様：
\[
R_i^{-} \;=\; \sum_{j\in J} \frac{(z_{ij})^{-}}{\sum_{i'\in I} (z_{i'j})^{-} - \epsilon}\, R_j^{-}.
\]
小さな安定化項 \(\epsilon>0\) を分母に加え、数値不安定を回避する（符号に応じて符号付きで加減）。最終的な下位層関連性は \(R_i = R_i^{+} + R_i^{-}\)。

- 直感：LRPの \(\alpha\)-\(\beta\) ルールに似るが、RAPはまず層間の相対影響度 \(|z|\) で優先度を与え、保存補正後に双極分離して伝播するため、非関連領域には明確に負のスコアが割当され、境界がシャープになる。

---

### 3. 実装ノート（重要ポイント）
- **安定化**: \(\sum_{j}|z_{jk}| \approx 0\) の場合に備え \(\epsilon\) を導入。
- **活性の前処理**: BatchNorm等は学習後にアフィン変換として吸収し、\(z=a\cdot w\) に一貫化。
- **プーリング**: Max-pooling はスイッチ位置に関連性を集中（Unpooling with switches）。
- **出力選択**: 分類ではロジットに対して関連性を初期化（目的クラスに正の初期関連性）。
- **可視化**: \(R^{+}\) と \(R^{-}\) を別カラーマップで描画し、関連／非関連領域を明瞭化。

---

### 4. 評価指標（論文で使用）
- **Outside-inside relevance ratio**: 物体外と内の関連性比で局在性を評価。
- **Segmentation mIOU**: 関連性マップから得た領域とGTセグの一致度。
- **Region perturbation**: 重要領域の順次破壊による出力低下（faithfulness）。

---

### 5. 「キモ」と重要性：本論文の核と影響
- **キモ**: 層間の相対寄与に基づく一次割当 → 保存補正 → 双極分離 → 符号分離伝播、という流れで「重要」と「非重要」を分離強調しつつ保存則を維持する点。
- **重要性/影響**: LRP系の曖昧さ（弱い非関連領域にも微小正値が残る）を解消し、視覚的にコントラストの高い説明を提供。弱教師あり局在や妥当性評価で良好な指標を達成。

---

### 6. まとめ（要点）
- 相対影響度：\(\rho_{jk} = |z_{jk}|/\sum_{j'}|z_{j'k}|\)。
- 保存補正：\(R_j = \tilde{R}_j + (\sum_k R_k - \sum_j \tilde{R}_j)/|J|\)。
- 双極分離：\(R_j^{+}=\max(0,R_j),\; R_j^{-}=\min(0,R_j)\)。
- 伝播：正負別に \((z)^{\pm}\) を用いて正規化し保存則を維持。

参考: [arXiv:1904.00605](https://arxiv.org/abs/1904.00605)
