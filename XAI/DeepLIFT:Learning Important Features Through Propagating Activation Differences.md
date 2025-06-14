DeepLIFT（Deep SHAPとしても知られています）は、ディープニューラルネットワークの予測を説明するための手法です。ネットワークの各ニューロンの出力が、入力特徴量の変化によってどのように変化するかを追跡することで、各入力特徴量が最終的な予測にどれだけ貢献したかを定量化します。

### DeepLIFTの基本的な考え方と理論

DeepLIFTの基本的な考え方は、ニューラルネットワークの順方向パスを「逆伝播」させ、各ニューロンの出力の変化に対する入力特徴量の貢献度を計算することです。これは、勾配ベースの手法とは異なり、非線形な活性化関数や相互作用を持つ複雑なネットワークでも、より正確な貢献度を評価できるとされています。

DeepLIFTの核となるアイデアは、**「参照（reference）」入力**と**「差分（difference）」**の概念です。

1.  **参照（Reference）入力 $x_0$**: これは、入力特徴量が「非アクティブ」または「基底状態」にあると見なされる入力です。例えば、画像分類タスクでは、すべてのピクセルがゼロである画像や、平均的な背景の画像が参照入力として使われることがあります。この参照入力に対するモデルの予測 $f(x_0)$ は、各特徴量が予測に寄与しない場合の「ベースライン」となります。

2.  **差分（Difference） $\Delta x = x - x_0$**: これは、実際の入力 $x$ と参照入力 $x_0$ の間の差です。DeepLIFTは、この差分がネットワークを伝播する際に、最終的な出力にどのように寄与するかを計算します。

3.  **貢献度（Contribution）**: DeepLIFTの目標は、各入力特徴量 $x_i$ が最終的な出力 $f(x)$ にどれだけ貢献したかを示す「貢献度」を計算することです。これは、次のような形式で表されます。

    $$
    \sum_{i} C_{\Delta x_i \rightarrow \Delta f} = f(x) - f(x_0)
    $$

    ここで、$C_{\Delta x_i \rightarrow \Delta f}$ は、入力特徴量 $i$ の差分 $\Delta x_i$ が、最終的な出力の差分 $\Delta f = f(x) - f(x_0)$ に与える貢献度を示します。

### 理論/手法の核心

DeepLIFTは、連鎖律を適用して、各層のニューロンの出力の差分がその前の層のニューロンの出力の差分によってどのように説明されるかを分解します。

各層のニューロン $t$ の出力 $y_t$ を考えます。その入力は $y_p$ （前の層のニューロン）と重み $w_{tp}$ です。
$y_t = g(\sum_p w_{tp} y_p + b_t)$

ここで、$g$ は活性化関数、$b_t$ はバイアスです。
参照入力に対する出力 $y_t^0$ も同様に計算されます。
$y_t^0 = g(\sum_p w_{tp} y_p^0 + b_t)$

DeepLIFTは、ニューロン $t$ の出力の差分 $\Delta y_t = y_t - y_t^0$ を、その入力ニューロン $p$ の出力の差分 $\Delta y_p = y_p - y_p^0$ に基づいて分解します。

主要な概念は、**「貢献度スコア」の伝播**です。
各ニューロン $t$ には、最終的な出力 $f(x)$ への貢献度スコア $C_{\Delta y_t \rightarrow \Delta f}$ が割り当てられます。DeepLIFTのアルゴリズムは、この貢献度スコアを逆伝播させ、最終的に各入力特徴量への貢献度スコア $C_{\Delta x_i \rightarrow \Delta f}$ を計算します。

貢献度スコアの計算には、いくつかのルールがあります。ここでは、線形層とReLUsを例に説明します。

**1. 線形層の場合**

ニューロン $t$ の出力が、その入力ニューロン $p$ から線形結合で得られる場合、すなわち、
$y_t = \sum_p w_{tp} y_p + b_t$
$\Delta y_t = y_t - y_t^0 = \sum_p w_{tp} (y_p - y_p^0) = \sum_p w_{tp} \Delta y_p$

この場合、$\Delta y_t$ への各入力 $\Delta y_p$ の貢献度は、$w_{tp} \Delta y_p$ です。
貢献度スコアの伝播は、線形結合のルールに従います。

$$C_{\Delta y_p \rightarrow \Delta f} = \sum_{t} C_{\Delta y_t \rightarrow \Delta f} \cdot \frac{w_{tp} \Delta y_p}{\Delta y_t} \quad (\text{if } \Delta y_t \neq 0)$$

この式は、ニューロン $t$ の最終出力への貢献度 $C_{\Delta y_t \rightarrow \Delta f}$ が、その入力 $\Delta y_p$ にどのように分配されるかを示しています。

**2. 非線形活性化関数（ReLUs）の場合**

ReLU（Rectified Linear Unit）関数 $g(x) = \max(0, x)$ の場合、単純な線形結合とは異なります。
DeepLIFTでは、活性化関数の動作に応じて貢献度の分配ルールを調整します。

-   **"Rescale" ルール**: これは、最も一般的なDeepLIFTのルールです。
    ReLUの場合、入力 $x_{in}$ と参照入力 $x_{in}^0$ を考えます。
    出力 $y = \max(0, x_{in})$ と $y^0 = \max(0, x_{in}^0)$。
    差分 $\Delta y = y - y^0$。

    もし $x_{in} > 0$ かつ $x_{in}^0 > 0$ であれば、$\Delta y = \Delta x_{in}$ となり、線形の場合と同じです。
    もし $x_{in} \le 0$ かつ $x_{in}^0 \le 0$ であれば、$\Delta y = 0$ となります。
    もし $x_{in} > 0$ かつ $x_{in}^0 \le 0$ であれば、$y = x_{in}$、$y^0 = 0$ なので、$\Delta y = x_{in}$ となります。
    この場合、貢献度は以下のようになります。
    $$
    C_{\Delta x_{in} \rightarrow \Delta y} = \begin{cases}
    \Delta y & \text{if } \Delta x_{in} \neq 0 \text{ and } x_{in} \text{ and } x_{in}^0 \text{ are on opposite sides of 0} \\
    \Delta y \frac{\Delta x_{in}}{\Delta x_{in}} = \Delta y & \text{if } \Delta x_{in} \neq 0 \text{ and } x_{in}, x_{in}^0 \text{ are on same side of 0} \\
    0 & \text{otherwise}
    \end{cases}
    $$
    これは、各ニューロンの出力の差分 $\Delta y_t$ を、その入力の差分 $\Delta x_{in}$ に基づいて「再スケール」することで、貢献度を計算します。
    より一般的には、ある層のニューロン $t$ の入力 $z_t = \sum_p w_{tp} y_p + b_t$ を考えたとき、
    $$
    C_{\Delta y_p \rightarrow \Delta f} = \sum_{t} C_{\Delta y_t \rightarrow \Delta f} \cdot \frac{w_{tp} \Delta y_p}{\Delta z_t} \quad (\text{if } \Delta z_t \neq 0)
    $$
    ただし、活性化関数が非線形であるため、この関係はより複雑になります。DeepLIFTは、活性化関数の特性を考慮した特定のルール（例えば、"Rescale"ルールや"RevealCancel"ルールなど）を適用して、$\frac{\Delta z_t}{\Delta y_t}$ や $\frac{\Delta y_p}{\Delta z_t}$ のような項を適切に計算します。

-   **"RevealCancel" ルール**: 相互に打ち消し合う影響を考慮します。

DeepLIFTの重要な特徴は、**勾配情報を使用しない**ことです。その代わりに、各ニューロンの活性化の「差分」を追跡することで、勾配がゼロになる領域（ReLUsの非アクティブ領域など）でも貢献度を適切に分配できます。

### 「キモ」と重要性

DeepLIFTの「キモ」は、以下の点に集約されます。

1.  **参照入力を導入したこと**: 予測が「変化」した理由を説明するために、明確な「ベースライン」を設けることで、各特徴量の貢献度をより意味のある形で定量化できるようになった点です。これにより、単に勾配を見るだけでは捉えられない、特徴量の「存在」自体による影響を捉えることができます。

2.  **非線形性を適切に扱うルール**: 勾配ベースの手法が苦手とするReLUなどの非線形活性化関数に対しても、特定のルール（Rescaleルールなど）を適用することで、貢献度を適切に分配できる点です。これにより、ディープニューラルネットワークの複雑な挙動をより忠実に説明することが可能になります。特に、ある入力が活性化関数を通過する際に「ゼロ」になる場合でも、その前の層での貢献が考慮されるため、勾配消失問題を回避し、より深い層からの影響を追跡できます。

3.  **貢献度の完全分解**: ネットワークの出力の差分を、入力特徴量の差分の貢献度の和として完全に分解できるという性質は、説明可能性の分野において非常に強力です。これは、なぜモデルがそのような予測をしたのかを、個々の入力特徴量にまで遡って理解することを可能にします。

この論文の重要性は、ディープラーニングモデルの**説明可能性（Explainable AI, XAI）**の分野に大きな進歩をもたらしたことです。特に、医療診断、金融取引、自動運転など、モデルの予測が人間の生活に直接影響を与える分野では、その予測の根拠を理解することが不可欠です。DeepLIFTは、モデルがなぜそのような決定を下したのかを、人間が理解しやすい形で提示することで、モデルの信頼性を高め、開発者がモデルの欠陥を特定し、改善する手助けをします。また、DeepLIFTの考え方は、後のSHAP（SHapley Additive exPlanations）フレームワークにも影響を与え、統合勾配法やLIMEなどの他の説明可能性手法と統合され、より汎用的な説明フレームワークの基礎となりました。