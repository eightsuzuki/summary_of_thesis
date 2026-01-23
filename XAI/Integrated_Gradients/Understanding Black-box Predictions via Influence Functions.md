# How Important Is a Neuron?

**著者**: Mukund Sundararajan, Ankur Taly, Qiqi Yan (Google Inc.)  
**arXiv**: arXiv:1805.12233  
**年**: 2018年5月  
**URL**: [https://arxiv.org/abs/1805.12233](https://arxiv.org/abs/1805.12233)  
**HTML版**: [https://ar5iv.labs.arxiv.org/html/1805.12233](https://ar5iv.labs.arxiv.org/html/1805.12233)

---

## 概要

この論文は、**Layer IG（Layer Integrated Gradients）の理論的な定義を最初に提案した論文**です。論文中では **"Conductance（コンダクタンス）"** と呼ばれており、Integrated Gradients (IG) の考案者たち（Sundararajan et al.）が執筆したIGの続編にあたります。

**重要な位置づけ**:  
「Layer IG の理論的な元論文はどれ？」と聞かれたら、**この論文（1805.12233）が正解**です。この論文で、入力特徴量に対する IG を分解し、**「特定の中間層のニューロンを経由して、予測にどれだけ寄与したか」** を計算する数式（Conductance）が定義されました。

**注**: この論文は、CaptumライブラリのLayer Conductanceの実装で参照されています（[https://captum.ai/api/layer.html](https://captum.ai/api/layer.html)）。

---

## Conductance（Layer IG）の概念

### 基本的な定義

Conductanceは、**隠れユニットを通過するアトリビューションの流れ（Flow of attribution）**を測定する概念です。これは、Integrated Gradientsを連鎖律を通じて分解することで定義されます。

### 通常のIGとの違い

- **通常のIG**: 入力特徴量 $x$ の寄与を見ます
- **Conductance (Layer IG)**: 隠れ層のニューロン $h$ の寄与を見ます

### 定義式

Conductanceは、「入力からニューロンまでの勾配」と「ニューロンから出力までの勾配」を組み合わせた経路積分として定義されます：

\[
\text{Conductance}(h) = \sum_i (x_i - x_i') \times \int_{\alpha=0}^{1} \frac{\partial F(x' + \alpha(x - x'))}{\partial h} \times \frac{\partial h}{\partial x_i} d\alpha
\]

この式は、以下の3つの要素を組み合わせたものです：

1. **ニューロンの活性化**: 隠れユニットの活性化値
2. **出力に対するニューロンの偏微分** ($\frac{\partial F}{\partial h}$): モデルの出力に対するニューロンの偏微分（ニューロンから出力までの勾配）
3. **ニューロンに対する入力の偏微分** ($\frac{\partial h}{\partial x_i}$): ニューロンに対する入力特徴の偏微分（入力からニューロンまでの勾配）

### Integrated Gradientsとの関係

Conductanceは、Integrated Gradientsを連鎖律を通じて分解することで、内部ニューロンの重要度を測定します。これにより、入力特徴の重要度だけでなく、内部ニューロンの重要度も評価できるようになります。

---

## 技術的な詳細

### Conductanceの計算

Conductanceは、内部ニューロン $h$ を通る情報の流れを測定します：

\[
\text{Conductance}(h) = \sum_i (x_i - x_i') \times \int_{\alpha=0}^{1} \frac{\partial F(x' + \alpha(x - x'))}{\partial h} \times \frac{\partial h}{\partial x_i} d\alpha
\]

ここで：
- $x_i$: 入力特徴 $i$ の値
- $x_i'$: ベースライン特徴 $i$ の値
- $F$: モデルの出力関数
- $h$: 内部ニューロン
- $\alpha$: 補間パラメータ

### Layer Conductance

Layer Conductanceは、特定の層の全ニューロンに対するConductanceを計算します。これは、Captumライブラリの`LayerConductance`クラスで実装されています。

---

## Captumライブラリでの実装

### Layer Conductance

Captumライブラリでは、`LayerConductance`クラスが提供されており、特定の層に対するConductanceを計算できます。

**参照**: [https://captum.ai/api/layer.html](https://captum.ai/api/layer.html)

### 実装の詳細

- **forward_func**: モデルのforward関数
- **layer**: アトリビューションを計算する層
- **attribute**: Conductanceを計算するメソッド

---

## 関連研究との関係

### Computationally Efficient Measures of Internal Neuron Importance (arXiv:1807.09946)

この論文は、本論文（1805.12233）で提案された Conductance (Layer IG) の**計算コストが高い**という問題を指摘し、より効率的な計算方法（**Neuron Integrated Gradients**）を提案した論文です。

**関係性**:
- **1805.12233（本論文）**: Layer IG（Conductance）の**理論的な定義を最初に提案**
- **1807.09946**: Conductanceの**計算効率を改善し実装上の議論をした**論文

**1807.09946の主要な貢献**:
- 「Conductance の計算は、実は単純な Layer IG（その層に対する Path Integrated Gradients）と数学的に等価である」ことを証明
- これにより、複雑な実装をしなくても、標準的な勾配計算（TensorFlowなどの標準機能）だけで高速に計算できることを示した
- Layer IG と DeepLIFT の比較実験も行い、DeepLIFT の方が高速だが、Layer IG (Neuron IG) の方が理論的な性質（公理）を満たすため信頼性が高いケースがある、と論じている

---

## まとめ：どちらを読むべき？

### 理論や定義を知りたい場合
**`1805.12233` (How Important Is a Neuron?)** - **本論文**  
Layer IG (Conductance) がなぜその式になるのか、その意味（Flow of attribution）を理解するにはこちらが基本です。

### 実装や高速化、DeepLIFTとの違いを知りたい場合
**`1807.09946` (Computationally Efficient Measures...)**  
実際に計算する際のテクニックや、他の手法との比較について書かれています。

### 引用について

もし論文で「Layer IG」や「Conductance」を使用・引用する場合は、基本的には **`1805.12233`（本論文）** を引用するのが適切です。これは、Layer IG（Conductance）の理論的な定義を最初に提案した論文だからです。

---

## 論文の意義

「How Important Is a Neuron?」（arXiv:1805.12233）は、**Layer IG（Layer Integrated Gradients）の理論的な定義を最初に提案した重要な研究**です。この論文で、入力特徴量に対する IG を分解し、**「特定の中間層のニューロンを経由して、予測にどれだけ寄与したか」** を計算する数式（Conductance）が定義されました。

Conductanceは、CaptumライブラリのLayer Conductanceの実装で参照されており、深層学習モデルの解釈可能性を向上させる重要な概念です。
