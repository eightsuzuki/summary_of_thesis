# Explaining Explanations: Axiomatic Feature Interactions for Deep Networks

**著者**: Joseph D. Janizek (University of Washington), Pascal Sturmfels (University of Washington), Su-In Lee (University of Washington)  
**arXiv**: [arXiv:2002.04138](https://arxiv.org/abs/2002.04138)
---
https://arxiv.org/abs/2002.04138/
Integrated Hessians (IH)っているIGを使って、入力iとjが出力に対してどれだけ相互も影響したかを表す。"painfully funny" というフレーズにおいて、"painfully" 単体では負の寄与を持ちますが、"funny" との間に正の相互作用を持つ。ヘッセ行列使ってる相互作用を計算しているから、二回微分して0になるReLUとかには使えないGELUには使える。
---
この論文は、ディープニューラルネットワークの予測を解釈するための新しい手法「**Integrated Hessians (IH)**」を提案するものです [cite: 5]。従来の特徴量アトリビューション手法が「どの特徴量が重要か」を説明するのに対し、本手法は「特徴量がどのように相互作用して予測に影響を与えるか」という、より深いレベルでの説明を可能にすることを目的としています [cite: 3, 4]。

以下に、その理論的背景、数式による定義、特性、および応用について詳細に解説します。

### 1. 背景：Integrated Gradients (IG)

Integrated Hessiansを理解するためには、まずその基礎となる**Integrated Gradients (IG)** を理解する必要があります [cite: 43]。

モデルを、入力ベクトル $x \in \mathbb{R}^d$ を受け取り、スカラー値を出力する関数 $f: \mathbb{R}^d \mapsto \mathbb{R}$ として表現します [cite: 44]。IGは、ある入力 $x$ に対する予測を説明する際に、特徴量 $i$ のアトリビューション（寄与度）を、ベースライン（情報がない状態を表す入力）$x'$ から $x$ への経路に沿った勾配の積分として定義します [cite: 44]。

特徴量 $i$ のIGアトリビューション $\phi_i(x)$ は、以下の式で与えられます [cite: 44]。

$$\phi_i(x) = (x_i - x'_i) \times \int_{\alpha=0}^{1} \frac{\partial f(x' + \alpha(x - x'))}{\partial x_i} d\alpha$$
(数式1)

この式の意味するところは、ベースライン $x'$ から入力 $x$ へと至る直線経路上の各点における特徴量 $i$ に関する勾配を全て集め、その合計を寄与度とする、というものです。

### 2. Integrated Hessians (IH) の導出と定義

本論文の核となるアイデアは、「**IGアトリビューション $\phi_i(x)$ それ自体もまた、入力 $x$ に関する微分可能な関数である**」という点です [cite: 54]。したがって、ある特徴量 $i$ のアトリビューション $\phi_i(x)$ が、別の特徴量 $j$ の値によってどのように変化したかを説明するために、**アトリビューション関数 $\phi_i$ に対してIGを適用する**ことができます [cite: 54]。

これがIntegrated Hessiansの基本的な考え方であり、特徴量 $i$ と $j$ の間の相互作用 $\Gamma_{i,j}(x)$ は以下のように定義されます [cite: 54, 55]。

$$\Gamma_{i,j}(x) = \phi_j(\phi_i(x))$$
(数式2)

この定義を、IGの定義式 (数式1) を用いて展開することで、具体的な数式を導出します。付録A.1の導出過程を以下に示します [cite: 322]。

まず、$\phi_j(\phi_i(x))$ をIGの定義に従って展開します。
$$\Gamma_{i,j}(x) = (x_j - x'_j) \times \int_{\beta=0}^{1} \frac{\partial \phi_i(x' + \beta(x - x'))}{\partial x_j} d\beta$$
(数式3)

次に、積分の中の項 $\frac{\partial \phi_i(x)}{\partial x_j}$ を計算します ($i \neq j$ の場合)。
$$\frac{\partial \phi_i(x)}{\partial x_j} = \frac{\partial}{\partial x_j} \left( (x_i - x'_i) \times \int_{\alpha=0}^{1} \frac{\partial f(x' + \alpha(x - x'))}{\partial x_i} d\alpha \right)$$
$f$ の2階偏導関数が連続であるという条件下（ライプニッツの積分法則）で、微分と積分の順序を交換できます [cite: 323]。
$$= (x_i - x'_i) \times \int_{\alpha=0}^{1} \frac{\partial}{\partial x_j} \left( \frac{\partial f(x' + \alpha(x - x'))}{\partial x_i} \right) d\alpha$$
$$= (x_i - x'_i) \times \int_{\alpha=0}^{1} \alpha \frac{\partial^2 f(x' + \alpha(x - x'))}{\partial x_i \partial x_j} d\alpha$$
(数式4)

この(数式4)を(数式3)に代入し、変数を整理すると、Integrated Hessiansの最終的な式が得られます [cite: 328]。

$$\Gamma_{i,j}(x) = (x_i - x'_i)(x_j - x'_j) \times \int_{\beta=0}^{1} \int_{\alpha=0}^{1} \alpha\beta \frac{\partial^2 f(x' + \alpha\beta(x - x'))}{\partial x_i \partial x_j} d\alpha d\beta \quad (\text{for } i \neq j)$$
(数式5)

この式は、2つの特徴量間の相互作用が、ベースラインから入力への経路上の**ヘッセ行列（2階偏導関数）の積分**によって捉えられることを示しています。

同様に、$i=j$ の場合（自己相互作用）は、積の微分法則により1階の項が追加されます [cite: 331]。
$$\Gamma_{i,i}(x) = (x_i - x'_i)\int_{\beta=0}^{1}\int_{\alpha=0}^{1}\frac{\partial f(x'+\alpha\beta(x-x'))}{\partial x_{i}}d\alpha d\beta + (x_{i}-x_{i}^{\prime})^{2}\times\int_{\beta=0}^{1}\int_{\alpha=0}^{1}\alpha\beta\frac{\partial^{2}f(x^{\prime}+\alpha\beta(x-x^{\prime}))}{\partial x_{i}^{2}}d\alpha d\beta$$
(数式6)

### 3. Integrated Hessiansが満たすべき公理

良い説明手法であるためには、直感的に理にかなった性質（公理）を満たす必要があります。IGは**完全性 (Completeness)** という公理 $\sum_{i}\phi_{i}(x) = f(x) - f(x')$ を満たします [cite: 59]。Integrated Hessiansはこの性質を継承・拡張した以下の重要な公理を満たします。

* **相互作用の完全性 (Interaction Completeness)**: 全ての相互作用の合計は、モデルの出力の変化量と等しくなります [cite: 59]。
    $$
    \sum_{i} \sum_{j} \Gamma_{i,j}(x) = f(x) - f(x')
    $$
    (数式7)
    この公理により、計算された相互作用の値のスケールがモデルの出力と直接関連付けられ、解釈が容易になります [cite: 63, 64]。

* **自己完全性 (Self Completeness)**: ある特徴量 $i$ の自己相互作用（主効果）は、その特徴量のIGアトリビューションから、他の全ての特徴量との相互作用を差し引いたものとして定義されます [cite: 65]。
    $$
    \Gamma_{i,i}(x) = \phi_i(x) - \sum_{j \neq i} \Gamma_{i,j}(x)
    $$
    (数式8)
    これは、もし特徴量 $i$ が他のどの特徴量とも相互作用しない場合、その主効果はIGアトリビューションそのものに等しくなることを保証します ($\Gamma_{i,i}(x) = \phi_i(x)$) [cite: 66, 67]。

* **相互作用の対称性 (Interaction Symmetry)**: 2階偏導関数が連続であれば、特徴量 $i$ と $j$ の相互作用は対称になります ($\Gamma_{i,j}(x) = \Gamma_{j,i}(x)$) [cite: 70]。

その他にも、**相互作用の感度 (Interaction Sensitivity)** [cite: 345]、**実装不変性 (Implementation Invariance)** [cite: 349]、**相互作用の線形性 (Interaction Linearity)** [cite: 360] などの公理を満たすことが示されています。

### 4. ReLUネットワークへの対応：Softplusによる平滑化

多くのニューラルネットワークで用いられるReLU活性化関数 ($ReLU(x) = max(0,x)$) は区分的線形であり、その2階偏導関数はほとんどの領域で0になってしまいます [cite: 72, 73]。これは、ヘッセ行列に基づくIntegrated Hessiansが相互作用を検出できないという深刻な問題を引き起こします [cite: 73]。

この論文では、この問題を解決するために、ReLUをその滑らかな近似である**Softplus関数**に置き換えることを提案しています [cite: 74]。

$$\text{SoftPlus}_\beta(x) = \frac{1}{\beta} \log(1 + e^{\beta x})$$
(数式9)

この置き換えには以下の利点があります。
* Softplus関数は無限回微分可能であるため、2階微分を計算できます [cite: 75]。
* この置き換えは説明時に行うだけでよく、モデルの**再学習は不要**です [cite: 78]。
* $\beta$ を小さくして関数をより滑らかにすると、少ないサンプリング数（積分の近似のためのステップ数）でIntegrated Hessiansの値を正確に近似できるようになり、計算効率が向上します (Theorem 1) [cite: 80, 81]。

### 5. 実証評価と応用例

#### a. XOR関数による有効性の証明
XOR関数を学習したネットワークにおいて、「両入力が0」の場合と「両入力が1」の場合、出力は共に0（ベースラインと同じ）になります。IGではこれらの2つのケースで同じアトリビューション（両方0）が算出され区別できません [cite: 87]。しかしIntegrated Hessiansは、「両入力が1」の時に2つの入力間に強い**負の相互作用**を検出することで、両者が異なる理由（後者は正の効果が互いに打ち消し合った結果）を明確に区別できます [cite: 96, 97]。

#### b. 既存手法との比較
* **計算時間**: 特徴量数が大きい場合、他の厳密な手法（例：Shapley Interaction Index）よりも計算が高速です [cite: 109, 115]。
* **精度**: シミュレーションデータを用いた評価（Remove and Retrainベンチマーク）において、既存手法よりも正確に既知の相互作用を特定できることが示されています [cite: 110, 126, 127]。

#### c. 応用例
* **自然言語処理 (NLP)**:
    * **モデル比較**: 高性能なTransformerモデル (DistilBERT) が、単純なCNNモデルに比べてなぜ優れているかを、相互作用の観点から説明します。例えば、DistilBERTは "not" と "bad" のような単語間の**否定の文脈を捉えた正の相互作用**を学習していますが、CNNはそれを十分に捉えきれていません [cite: 51, 52, 147]。
    * **文脈理解**: "painfully funny" というフレーズにおいて、"painfully" 単体では負の寄与を持ちますが、"funny" との間に**正の相互作用**を持つことで、フレーズ全体の肯定的な意味合いをモデルがどのように理解しているかを可視化します [cite: 145]。
    * **飽和効果**: 「a bad, terrible, awful movie」のように否定的な単語が増えるほど、個々の単語の否定的な寄与度は下がる一方で、否定的な単語同士が**正の相互作用**を持つ「飽和効果」という言語モデルの興味深い振る舞いを明らかにします [cite: 148, 164]。

* **創薬（抗がん剤の組み合わせ効果予測）**:
    * 2つの薬の組み合わせを評価するモデルにおいて、特定の薬のペア（例：VenetoclaxとArtemisinin）が**負の相互作用（拮抗作用）を持つ**ことをモデルが学習したことを示します [cite: 159]。
    * さらに、この相互作用の強さが、患者のがん細胞の特定の遺伝子経路の発現レベルに依存していることを突き止め、モデルの予測根拠に関する生物学的な洞察を得ることに成功しています [cite: 170, 171, 172]。

### 結論

Integrated Hessiansは、Integrated Gradientsを公理的に拡張することで、ニューラルネットワークにおけるペアワイズの特徴量相互作用を捉えるための、理論的に堅牢で実用的な手法です [cite: 174, 175]。ReLUネットワークの問題点をSoftplusで克服し、既存手法に対する優位性も示されています [cite: 177, 175]。本手法は、モデルの予測根拠をより深く、多角的に理解するための強力なツールであり、説明可能なAI (XAI) の分野における重要な一歩と言えます [cite: 181]。()