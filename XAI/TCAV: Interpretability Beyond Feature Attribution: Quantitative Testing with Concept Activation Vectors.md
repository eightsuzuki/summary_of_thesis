# TCAV: Interpretability Beyond Feature Attribution: Quantitative Testing with Concept Activation Vectors

**著者**: Been Kim, Martin Wattenberg, Justin Gilmer, Carrie Cai, James Wexler, Fernanda Viégas, Rory Sayres  
**公開**: ICML 2018  
**リンク**: [arXiv:1711.11279](https://arxiv.org/abs/1711.11279)

本論文は、特徴量アトリビューションを超えて、ユーザ定義の「概念」がモデル予測にどれだけ寄与するかを定量化する **TCAV (Testing with Concept Activation Vectors)** を提案します。概念の例画像から**概念方向（CAV）**を学習し、その方向へのクラススコアの方向微分に基づいて、概念の影響度を測ります。

---

### 1. 新規性：この論文の貢献
- **ユーザ定義概念**（例: 「ストライプ」「縞模様」）の重要度を定量化し、モデル内部表現と結びつける。
- **層・クラス・概念ごとの統計的検定**により、有意に寄与する概念を判定。
- モデル・データ非依存で、後付けで適用可能。

---

### 2. 理論/手法の核心：数式できっちり定式化

#### 記法
- 中間層 \(\ell\) の活性を \(f_{\ell}(x)\in\mathbb{R}^{m}\)、クラス \(k\) のスコア（ロジット）を \(S_k(x)\) とする。
- 概念 \(C\) の正例集合 \(\mathcal{X}_C\) と、対照（ランダム）集合 \(\mathcal{X}_R\)。

#### 概念方向（CAV）の学習
- 特徴空間 \(\mathbb{R}^m\) で、\(f_{\ell}(x),\; x\in \mathcal{X}_C\cup \mathcal{X}_R\) を用いて**線形識別器**（正例=概念）を学習：
\[
\text{sign}\big(w_C^{\top} z + b\big), \quad z=f_{\ell}(x).
\]
- 概念方向ベクトル（CAV）を正規化：\(v_C = \tfrac{w_C}{\lVert w_C\rVert}\)。

#### 方向微分（Directional Derivative）
- 概念方向に沿ったクラススコアの感度：
\[
D_{k,\ell}^{(C)}(x) \;=\; \nabla_{f_{\ell}} S_k(x) \;\cdot\; v_C.
\]
- \(D_{k,\ell}^{(C)}(x) > 0\): 概念 \(C\) がクラス \(k\) を増加させる方向に寄与。\(D<0\): 反対に寄与。

#### TCAV スコア（概念の重要度）
- クラス \(k\) のデータ集合 \(\mathcal{X}_k\) に対し、
\[
\mathrm{TCAV}_{k,\ell}(C) \;=\; \frac{1}{|\mathcal{X}_k|}\sum_{x\in \mathcal{X}_k} \mathbf{1}\big[ D_{k,\ell}^{(C)}(x) > 0 \big].
\]
- 値が高いほど、概念 \(C\) がクラス \(k\) の予測を押し上げる割合が大きい。

#### 有意性検定
- 対照集合を複数（\(R_1, R_2, \dots\)）用いて CAV 学習と TCAV を繰り返し、\(\{\mathrm{TCAV}_{k,\ell}^{(C,R_i)}\}\) と \(\{\mathrm{TCAV}_{k,\ell}^{(R_j)}\}\) を比較。
- 両側/片側 t 検定などで \(\mathrm{TCAV}_{k,\ell}(C)\) がランダム概念より有意に大きいか（\(p<\alpha\)）を判定。

---

### 3. 実装ノート
- **層選択**: 直前の畳み込み/全結合層など、概念が分離しやすい層で CAV を学習。
- **概念/対照サンプル**: 30–100 枚程度の画像が目安。対照は複数集合で安定化。
- **ロジット推奨**: \(S_k\) はソフトマックス前（飽和回避）。
- **勾配計算**: 自動微分で \(\nabla_{f_{\ell}} S_k\) を取得（入力勾配でなく活性勾配）。

---

### 4. 「キモ」と重要性
- **キモ**: ユーザが意味づけた概念をベクトル方向でモデル内部表現に埋め込み、その方向微分の符号・割合で寄与を定量化。
- **重要性**: 「何が概念として効いているか」をモデル視点で検証可能になり、ドメイン知識とモデル解釈の橋渡しに有用。

---

### 5. まとめ（要点）
- CAV: 概念 vs ランダムで線形分離し、法線を概念方向とする。
- 方向微分: \(D_{k,\ell}^{(C)}=\nabla_{f_{\ell}} S_k\cdot v_C\)。
- TCAV: 正方向割合で概念の寄与を定量化し、反復と統計検定で有意性を判断。

参考: [arXiv:1711.11279](https://arxiv.org/abs/1711.11279)
