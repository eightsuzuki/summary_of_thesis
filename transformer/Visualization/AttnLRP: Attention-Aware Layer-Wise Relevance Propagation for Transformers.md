# AttnLRP: Attention-Aware Layer-Wise Relevance Propagation for Transformers

**著者**: Reduan Achtibat (Fraunhofer Heinrich-Hertz-Institute), Aakriti Jain (Fraunhofer Heinrich-Hertz-Institute), Sayed Mohammad Vakilzadeh Hatefi (Fraunhofer Heinrich-Hertz-Institute), Thomas Wiegand (Fraunhofer Heinrich-Hertz-Institute, Technische Universität Berlin, BIFOLD), Maximilian Dreyer (Fraunhofer Heinrich-Hertz-Institute), Sebastian Lapuschkin (Fraunhofer Heinrich-Hertz-Institute, BIFOLD), Wojciech Samek (Fraunhofer Heinrich-Hertz-Institute, Technische Universität Berlin, BIFOLD)
**arXiv**: [arXiv:2402.05602](https://arxiv.org/abs/2402.05602)

---

この論文は、Transformerモデルの判断根拠を説明するための新しい手法「AttnLRP」を提案しています。以下にその詳細を解説します。

## 1. 新規性

AttnLRPの主な貢献と既存研究との違いは以下の通りです。

* **Transformer全体への忠実な説明**: この論文は、Transformerモデルの入力だけでなく、その内部の潜在表現に対しても、忠実なアトリビューション（各要素がモデルの出力にどれだけ寄与したかを示す指標）を計算できる初めての手法であると主張しています。 [cite: 4] 既存研究では、Transformerモデル全体に対して忠実性と計算効率を両立させることは困難な課題でした。 [cite: 2]
* **Transformer全体への適用**: Layer-wise Relevance Propagation (LRP) という既存の枠組みを拡張し、Transformerの**すべての構成要素**（アテンション層、MLP、正規化層）に対して、より適切で忠実な新しいLRP伝播ルールを導出しました。 [cite: 3, 32, 28] 特に、アテンション層内の非線形演算（ソフトマックス、行列積など）やMLP内の線形層・活性化関数、正規化層に対して、DTDに基づく数学的に正当化された伝播ルールを提供します。これにより、従来手法の課題であった数値的不安定性や説明の忠実性の低さを克服しています。 [cite: 30, 31]
* **計算効率**: 提案手法は、単一の後方パス（backward pass）に近い計算効率を維持しており、大規模なモデルにも適用可能です。 [cite: 4]

## 2. 理論/手法の核心

AttnLRPは、Deep Taylor Decomposition (DTD) フレームワーク [cite: 32] に基づいてLRPを拡張し、Transformerの各構成要素に対する関連性の伝播ルールを定義します。

### 2.1. Layer-wise Relevance Propagation (LRP) の基礎

LRPは、ニューラルネットワークの予測結果 $f(x)$ を、各入力特徴量 $x_i$ の寄与度（関連性スコア） $R_i$ の和として分解する手法です。 [cite: 75]

$f(x) \approx \sum_i R_i$

より正確には、ある層のニューロン $j$ の出力 $f_j(x)$ は、そのニューロンへの入力 $x_i$ からの関連性 $R_{i \leftarrow j}$ の合計として表されます。 [cite: 75]

$f_j(x) \approx \sum_i R_{i \leftarrow j}$ (式1に基づく表記 [cite: 76])

LRPの重要な特性の一つに**保存則 (Conservation Property)** があり、これは各層で関連性の総和が（ほぼ）一定に保たれることを意味します。 [cite: 81]

$\sum_i R_i^{l-1} = \sum_j R_j^l$ (式3より簡略化 [cite: 81])
ここで、$R_j^l$ は第 $l$ 層のニューロン $j$ の関連性スコア、$R_i^{l-1}$ は第 $l-1$ 層のニューロン $i$ の関連性スコアです。

### 2.2. Deep Taylor Decomposition (DTD) による線形化と伝播ルール

DTDは、テイラー展開を用いてニューラルネットワーク内の各ニューロンの活性化関数を局所的に線形化し、関連性を下位層へ伝播させるためのルールを導出します。 [cite: 84] あるニューロン $j$ の関数 $f_j(x)$ を参照点 $\tilde{x}$ で一次テイラー展開すると以下のようになります。 [cite: 87]

$f_j(x) = f_j(\tilde{x}) + \sum_i J_{ji}(\tilde{x})(x_i - \tilde{x}_i) + \mathcal{O}(|x-\tilde{x}|^2)$
$f_j(x) = \sum_i J_{ji}x_i + \underbrace{f_j(\tilde{x}) - \sum_i J_{ji}\tilde{x}_i}_{\text{bias } b_j} + \mathcal{O}(|x-\tilde{x}|^2)$ (式4 [cite: 87])
ここで、$J_{ji}$ はヤコビアン（$f_j$ の $x_i$ に関する偏微分）、$\mathcal{O}(\cdot)$ は高次の項を示します。

この線形化された関数を用いて、上位層のニューロン $j$ の関連性 $R_j$ を、その入力ニューロン $i$ への関連性 $R_{i \leftarrow j}$ に分配します。基本的な伝播ルールは以下のように表されます（安定化のため小さな値 $\epsilon$ を導入）。 [cite: 96]

$R_i = \sum_j J_{ji}x_i \frac{R_j}{f_j(x) + \epsilon \cdot \text{sign}(f_j(x))}$ (式5 [cite: 96])

### 2.3. Transformer内の各要素に対するAttnLRPルール

AttnLRPでは、Transformerを構成する主要な要素に対して、DTDに基づいた新しいLRPルールを導出・適用します。

**Transformerアーキテクチャの構成要素**:
Transformerの各ブロックは以下の主要な要素から構成されます：
1. **Multi-Head Attention**: クエリ、キー、バリューを用いた注意機構
2. **MLP (Feed Forward Network)**: 位置ごとの多層パーセプトロン
3. **正規化層**: LayerNorm、RMSNormなど
4. **残差接続**: スキップ接続

AttnLRPは、これらの**すべての要素**に対して適切な伝播ルールを提供します。名前は「Attention-Aware」ですが、MLPも同様に重要で、AttnLRPはMLPに対しても忠実な関連性伝播を実現します。

#### 2.3.1. 多層パーセプトロン (MLP / Feed Forward Network)

**MLPの重要性**:
Transformerアーキテクチャにおいて、MLPは各Transformerブロックの2つの主要なサブレイヤーのうちの1つです（もう1つはMulti-Head Attention）。MLPは位置ごとに独立して適用され、非線形変換を提供します。AttnLRPは、MLP内の各演算に対して適切な関連性伝播ルールを提供します。

**MLPの典型的な構造**:
\[
\text{MLP}(x) = W_2 \cdot \sigma(W_1 x + b_1) + b_2
\]

ここで、\(W_1, W_2\) は線形層の重み、\(b_1, b_2\) はバイアス、\(\sigma\) は活性化関数（ReLU、GELUなど）です。

**AttnLRPにおけるMLPの扱い**:

MLPは以下の3つの主要な演算から構成されます：
1. **第1線形層** (\(W_1 x + b_1\))
2. **活性化関数** (\(\sigma(\cdot)\))
3. **第2線形層** (\(W_2 \cdot \sigma(W_1 x + b_1) + b_2\))

AttnLRPは、これらの各演算に対して適切なLRP伝播ルールを適用します。

**1. 線形層の伝播ルール**:

線形層 \(z_j = \sum_i W_{ji}x_i + b_j\) に対して、\(\epsilon\)-LRPルールが基本として用いられます。 [cite: 102, 103]

\[
R_i^{l-1} = \sum_j W_{ji}x_i \frac{R_j^l}{z_j(x) + \epsilon} \quad \text{(式8 [cite: 102])}
\]

ここで：
- \(W_{ji}\): 重み行列の要素
- \(x_i\): 入力特徴
- \(z_j(x)\): 線形層の出力（活性化関数適用前）
- \(R_j^l\): 出力側の関連性スコア
- \(R_i^{l-1}\): 入力側の関連性スコア
- \(\epsilon\): 数値安定化のための小さな値（通常 \(10^{-9}\) など）

**Vision Transformer (ViT) での特別な扱い**:
ViTの場合は、勾配破壊（gradient shattering）によるノイズを軽減するために、線形層に対して \(\gamma\)-LRPルールを適用します。 [cite: 105, 107] これは、重みの絶対値に基づいて関連性を分配するルールです。

**2. 活性化関数の伝播ルール**:

要素ごとの非線形活性化関数 \(a_j = \sigma(z_j)\)（例: ReLU、GELU）に対して、**恒等ルール (Identity Rule)** が適用されます。 [cite: 108, 109, 110]

\[
R_i^{l-1} = R_i^l \quad \text{(式9 [cite: 110])}
\]

**なぜ恒等ルールが適用されるか**:
- 要素ごとの活性化関数（ReLU、GELUなど）は、入力と出力が1対1に対応する
- 各要素 \(i\) の関連性は、その要素の活性化前後の値に依存しない
- したがって、関連性はそのまま伝播する

**3. MLP全体での関連性の流れ**:

MLPを通じた関連性の伝播は以下の順序で行われます：

```
MLP入力 x
    ↓
[第1線形層] ε-LRPルールで関連性を分配
    ↓
[活性化関数] 恒等ルールで関連性をそのまま伝播
    ↓
[第2線形層] ε-LRPルールで関連性を分配
    ↓
MLP出力
```

**保存則の維持**:
各演算で保存則が維持されるため、MLP全体でも関連性の総和が保存されます：
\[
\sum_i R_i^{\text{MLP入力}} = \sum_j R_j^{\text{MLP出力}}
\]

**MLPの重要性**:
- MLPはTransformerブロックの約2/3のパラメータを占める
- 非線形変換を提供し、モデルの表現力を大きく左右する
- AttnLRPにより、MLP内のどのニューロンが予測に寄与したかを理解できる

#### 2.3.2. 非線形アテンションメカニズム

アテンションは以下の計算で表されます。 [cite: 111]
$A = \text{softmax}\left(\frac{Q K^T}{\sqrt{d_k}}\right)$ (式10 [cite: 111])
$O = A V$ (式11 [cite: 111])
ここで、$Q$ はクエリ、$K$ はキー、$V$ はバリュー、$d_k$ はキーの次元数、$A$ はアテンション行列、$O$ はアテンションの出力です。

* **ソフトマックス関数 ($\text{softmax}_j(x) = \frac{e^{x_j}}{\sum_k e^{x_k}}$)**:
    Proposition 3.1として、ソフトマックス関数に対する新しい伝播ルールを導出しています。 [cite: 120] これは、参照点 $x$ でのテイラー展開に基づき、隠れバイアス項が関連性の一部を吸収することを許容します。 [cite: 120, 121]
    $R_i^{l-1} = x_i(R_i^l - s_i \sum_j R_j^l)$ (式13 [cite: 120])
    ここで、$x_i$ はソフトマックスへの入力の $i$ 番目の要素、$R_i^l$ はソフトマックス出力の $i$ 番目の要素の関連性、$s_i$ はソフトマックス出力の $i$ 番目の値です。このルールは、従来手法で見られた数値的不安定性を回避します。 [cite: 125]
* **行列積 ($Q K^T$ および $A V$)**:
    行列積はバイリニア（各入力に対して線形）な演算です。Proposition 3.3では、行列積 $O_{jp} = \sum_i A_{ji}V_{ip}$ における入力 $A_{ji}$ への関連性伝播ルールを以下のように提案しています。 [cite: 129] これは、まず和の演算を $\epsilon$-ルールで分解し、次に要素ごとの積 $A_{ji}V_{ip}$ を均一ルール (Proposition 3.2, $R_{ji}^{l-1} = \frac{1}{N}R_j^l$ (式14 [cite: 128])、ここで $N=2$) で分解することで導かれます。
    $R_{ji}^{l-1} = \sum_p A_{ji}V_{ip} \frac{R_{jp}^l}{2 O_{jp} + \epsilon}$ (式15 [cite: 129])
    ここで、$R_{jp}^l$ は出力 $O_{jp}$ の関連性です。このルールは保存則を厳密に満たします。 [cite: 131] 同様のルールが $V_{ip}$ に対しても導出されます。
* **正規化層 (LayerNorm, RMSNorm)**:
    LayerNorm: $LayerNorm(x)_j = \frac{x_j - \mathbb{E}[x]}{\sqrt{Var[x]+\epsilon}}\gamma_j + \beta_j$ (式16 [cite: 134])
    RMSNorm: $RMSNorm(x)_j = \frac{x_j}{\sqrt{\frac{1}{N}\sum_k x_k^2+\epsilon}}\gamma_j$ (式17 [cite: 134])
    Proposition 3.4として、これらの正規化層の非線形な正規化部分 $f_j(x) = x_j / g(x)$ に対して、参照点0でのテイラー展開を用いることで恒等ルールが導出されることを示しています。 [cite: 139, 140]
    $R_i^{l-1} = R_i^l$ (式19 [cite: 139])
    これは、関連性がバイアス項に吸収されることなく伝播することを意味し、効率的かつ忠実な説明を可能にします。 [cite: 140] アフィン変換部分（$\gamma_j$ によるスケーリングや $\beta_j$ によるシフト）は線形演算として別途 $\epsilon$-LRPルールで扱われます。 [cite: 135]

## 3. 「キモ」と重要性

### 3.1. 「キモ」（核となるアイデア）

AttnLRPの「キモ」は、**Transformerのすべての構成要素（アテンション層、MLP、正規化層）に対して、Deep Taylor Decomposition (DTD) フレームワークに基づいて数学的に正当化された新しいLRP伝播ルールを精密に設計した点**にあります。 [cite: 32, 36] 特に、アテンションメカニズム内の複雑な非線形演算（ソフトマックス、行列積）だけでなく、MLP内の線形層や活性化関数、正規化層に対しても適切な伝播ルールを提供します。これにより、従来曖昧だったり数値的に不安定だったりした部分の関連性伝播を、忠実かつ安定的に行えるようにしました。特にソフトマックス関数や行列積に対する扱いが独創的です。

### 3.2. 重要性

この研究の重要性は多岐にわたります。

* **Transformerモデルの透明性向上**: 大規模言語モデル（LLM）やVision Transformer（ViT）など、現代のAIの中核をなすTransformerモデルの「ブラックボックス」性を低減し、なぜそのような予測や生成を行ったのかを人間が理解できるようにします。 [cite: 1, 9, 230]
* **信頼性の高いAIの実現**: モデルの誤った予測（例：ハルシネーション）やバイアスがかった挙動の原因を特定しやすくなり、より信頼性が高く安全なAIシステムの開発に貢献します。 [cite: 1, 8, 230]
* **潜在表現の理解と操作**: 入力だけでなくモデル内部の潜在的な特徴量がどのように使われているかを明らかにできます。 [cite: 4, 5, 33] これにより、特定の「概念」がニューロンレベルでどのように表現されているかを解析したり（図2、図3 [cite: 19, 149]）、さらにはモデルの挙動を意図的に操作したりする（例：特定の概念に関連するニューロンの活動を増減させて出力を変える [cite: 35, 219, 220]）といった応用への道が開かれます。
* **XAI分野への貢献**: 説明可能なAI（XAI）の分野において、Transformerに特化した強力な説明手法を提供することで、この分野の研究を大きく前進させます。 [cite: 31] 特に、これまで両立が難しかった「説明の忠実性」と「計算効率」を高いレベルで実現し、さらに「潜在表現の説明」という新たな次元を切り拓いた点は重要です。 [cite: 2, 4]
* **実用的なツールの提供**: 提案手法の実装がオープンソースで提供される [cite: 6, 38] ことで、研究者や開発者が容易に利用し、さらなる応用研究や改善を進めることができます。
