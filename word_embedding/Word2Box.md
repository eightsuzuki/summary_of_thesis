# Word2Box：単語をボックス（超矩形）で表現する埋め込み

**著者**:
* Shib Sankar Dasgupta (Indian Institute of Technology Kharagpur)
* Michael Boratko (University of Massachusetts Amherst)
* Dongxu Zhang (University of Massachusetts Amherst)
* Luke Vilnis (University of Massachusetts Amherst)
* Xiang Lorraine Li (University of Massachusetts Amherst)
* Andrew McCallum (University of Massachusetts Amherst)

**出版**: Findings of EMNLP 2020

**URL**: [Improving Local Identifiability in Probabilistic Box Embeddings](https://arxiv.org/abs/2010.04831)

---

### 概要

Word2Boxは、単語を単一のベクトル（点）ではなく、**ボックス（box、超矩形）**として表現する新しい埋め込み手法です。ボックスは、$d$ 次元空間における軸平行な超矩形として定義され、最小座標と最大座標のペアによって表現されます。

この手法の動機は、自然言語の意味的な関係が、しばしば**包含関係（entailment）**や**不整合（disjointness）**のような構造を持つという観察に基づいています。例えば、"animal"（動物）という概念は "dog"（犬）を包含し、"mammal"（哺乳類）と "reptile"（爬虫類）は互いに素です。ボックス表現は、これらの関係を自然に表現できる幾何学的な構造を提供します。

### 新規性

Word2Boxの主な貢献は以下の点です：

* **点ではなく領域としての表現**: 単語を空間内の単一点ではなく、ボリュームを持つ領域として表現することで、概念の「広がり」や「包含関係」を自然にモデル化できます。
* **非対称な関係のモデリング**: ボックスの包含関係は非対称であり、"dog is-a animal"（犬は動物である）は真でも、その逆は真ではないという性質を自然に表現できます。
* **確率的解釈**: ボックスを確率分布として解釈することで、不確実性や曖昧性をモデル化できます。
* **共起と含意の同時学習**: Word2Vecのような共起ベースの目的関数と、概念階層を捉える包含関係の目的関数を組み合わせています。

### 理論/手法の核心

#### 1. ボックスの定義

$d$ 次元空間におけるボックス $\mathcal{B}$ は、最小座標ベクトル $\mathbf{m} \in \mathbb{R}^d$ と最大座標ベクトル $\mathbf{M} \in \mathbb{R}^d$ によって定義されます：

$$
\mathcal{B} = [\mathbf{m}, \mathbf{M}] = \{\mathbf{x} \in \mathbb{R}^d \mid \mathbf{m} \leq \mathbf{x} \leq \mathbf{M}\}
$$

ここで、不等式は要素ごとに成り立ちます（$m_i \leq x_i \leq M_i$ for all $i$）。

**制約**: ボックスが空でないためには、すべての次元で $m_i \leq M_i$ である必要があります。

#### 2. ボックスの演算

**ボックスの体積（Volume）**:
$$
\text{Vol}(\mathcal{B}) = \prod_{i=1}^{d} \max(M_i - m_i, 0)
$$

**2つのボックスの交差（Intersection）**:
$$
\mathcal{B}_1 \cap \mathcal{B}_2 = [\max(\mathbf{m}_1, \mathbf{m}_2), \min(\mathbf{M}_1, \mathbf{M}_2)]
$$

ここで、$\max$ と $\min$ は要素ごとの演算です。

**包含度（Containment）**:
ボックス $\mathcal{B}_1$ がボックス $\mathcal{B}_2$ に包含される度合いを、以下のように定義します：

$$
P(\mathcal{B}_1 \subseteq \mathcal{B}_2) = \frac{\text{Vol}(\mathcal{B}_1 \cap \mathcal{B}_2)}{\text{Vol}(\mathcal{B}_1)}
$$

これは、$\mathcal{B}_1$ のどれだけの割合が $\mathcal{B}_2$ に含まれているかを表します。

#### 3. 確率的解釈

ボックスを一様分布として解釈することができます：
$$
p(\mathbf{x} \mid \mathcal{B}) = \begin{cases}
\frac{1}{\text{Vol}(\mathcal{B})} & \text{if } \mathbf{x} \in \mathcal{B} \\
0 & \text{otherwise}
\end{cases}
$$

2つのボックスの重なりは、対応する分布間のKLダイバージェンスや他の確率的距離で測ることができます。

#### 4. 学習目的関数

Word2Boxは、複数の目的を組み合わせて学習します：

**（1）共起ベースの目的関数（Skip-gram風）**:
単語 $w$ とその文脈単語 $c$ のボックスが重なるべきであることを学習します：

$$
\mathcal{L}_{\text{context}} = -\sum_{(w,c) \in D} \log \sigma(\text{Vol}(\mathcal{B}_w \cap \mathcal{B}_c))
$$

ここで、$\sigma$ はシグモイド関数、$D$ は訓練データです。

**（2）包含関係の目的関数**:
もし外部知識（WordNetなど）から、単語 $w_1$ が $w_2$ に含まれる（例："dog" → "animal"）という情報があれば、それを以下の損失で学習します：

$$
\mathcal{L}_{\text{entailment}} = -\sum_{(w_1, w_2) \in \mathcal{E}} \log P(\mathcal{B}_{w_1} \subseteq \mathcal{B}_{w_2})
$$

ここで、$\mathcal{E}$ は包含関係のペアの集合です。

**（3）正則化**:
ボックスが適切なサイズを持つように正則化します：

$$
\mathcal{L}_{\text{reg}} = \lambda \sum_{w} \log \text{Vol}(\mathcal{B}_w)
$$

これにより、ボックスが無限大に大きくなったり、極端に小さくなったりするのを防ぎます。

**全体の目的関数**:
$$
\mathcal{L} = \mathcal{L}_{\text{context}} + \alpha \mathcal{L}_{\text{entailment}} + \beta \mathcal{L}_{\text{reg}}
$$

#### 5. ボックスのパラメータ化

実装上、ボックスは以下のように表現されます：

* 中心ベクトル $\mathbf{c} \in \mathbb{R}^d$
* オフセット（半サイズ）ベクトル $\mathbf{o} \in \mathbb{R}^d$（常に正値）

ボックスの座標は以下のように計算されます：
$$
\mathbf{m} = \mathbf{c} - \mathbf{o}, \quad \mathbf{M} = \mathbf{c} + \mathbf{o}
$$

オフセット $\mathbf{o}$ は、$\mathbf{o} = \text{softplus}(\tilde{\mathbf{o}})$ のように非負性を保証します。

#### 6. 類似度の計算

学習後、2つの単語の類似度を以下のように計算できます：

**1. ボックス交差に基づく類似度**:
$$
\text{sim}(\mathcal{B}_1, \mathcal{B}_2) = \frac{\text{Vol}(\mathcal{B}_1 \cap \mathcal{B}_2)}{\text{Vol}(\mathcal{B}_1 \cup \mathcal{B}_2)}
$$

（Jaccard係数に類似）

**2. 中心ベクトルの類似度**:
$$
\text{sim}(\mathcal{B}_1, \mathcal{B}_2) = \cos(\mathbf{c}_1, \mathbf{c}_2)
$$

（従来のベクトル埋め込みと同様）

### 「キモ」と重要性

Word2Boxの「キモ」は、**言語の意味構造が持つ非対称性と階層性を、幾何学的な包含関係として明示的にモデル化する**という点にあります。

**重要性**:

1. **階層的関係の自然な表現**:
   * 概念階層（taxonomy）は言語理解の基本的な要素です
   * ボックス表現は、"dog ⊆ mammal ⊆ animal"のような階層を、物理的な包含関係として直接的に表現できます
   * これは、通常のベクトルでは難しい（ベクトル演算で階層を表現するのは間接的）

2. **非対称性の保存**:
   * "is-a"関係は非対称です（犬は動物だが、動物は犬ではない）
   * ベクトルの内積やコサイン類似度は対称ですが、ボックスの包含関係は非対称です
   * これにより、より正確な意味表現が可能になります

3. **不確実性と曖昧性**:
   * ボックスのサイズは、概念の「一般性」や「特異性」を表現できます
   * 大きなボックス = 一般的な概念（"thing", "entity"）
   * 小さなボックス = 具体的な概念（"golden retriever"）

4. **複数の意味関係**:
   * 包含（entailment）: $\mathcal{B}_1 \subseteq \mathcal{B}_2$
   * 重なり（overlap）: $\text{Vol}(\mathcal{B}_1 \cap \mathcal{B}_2) > 0$
   * 排他（disjoint）: $\mathcal{B}_1 \cap \mathcal{B}_2 = \emptyset$
   
   これらすべてをボックスの幾何学的関係として自然に表現できます。

5. **知識グラフとの親和性**:
   * Word2Boxのアイデアは、知識グラフ埋め込みにも拡張されています（Box Embeddings for KG）
   * WordNetやFreebaseのような構造化知識を、連続空間で表現する強力な方法を提供します

6. **論理的推論**:
   * ボックスの演算（交差、和集合など）は、論理的な演算（AND, ORなど）と対応します
   * これにより、埋め込み空間上での推論が可能になります

**課題と限界**:
* 計算コスト：ボックスはベクトルの2倍のパラメータを持つ（最小・最大座標）
* 回転や変換：軸平行な超矩形という制約があり、回転した関係の表現には限界がある
* 負のサンプリング：ボックスの交差が空である場合の勾配計算が難しい

**後続研究への影響**:
* Box Embeddings for Knowledge Base Completion
* Probabilistic Box Embeddings
* 確率的プログラミングへの応用
* ハイパーボリックボックス（双曲空間でのボックス）

Word2Boxは、単語埋め込みを「点」から「領域」へと拡張することで、言語の意味構造をより豊かに表現する新しい方向性を示しました。特に、階層的知識や論理的関係を扱う必要があるアプリケーションにおいて、大きな可能性を持つアプローチです。

