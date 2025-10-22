# Word2GM：ガウス混合分布による単語埋め込み

**著者**:
* Luke Vilnis (University of Massachusetts Amherst)
* Andrew McCallum (University of Massachusetts Amherst)

**出版**: International Conference on Learning Representations (ICLR 2015)

**タイトル**: "Word Representations via Gaussian Embedding"

**URL**: [Word Representations via Gaussian Embedding](https://arxiv.org/abs/1412.6623)

---

### 概要

この論文では、単語を単一のベクトル（点）ではなく、**ガウス分布（正規分布）**として表現する新しい埋め込み手法が提案されています。各単語は、平均ベクトル $\boldsymbol{\mu}$ と共分散行列 $\boldsymbol{\Sigma}$ によって特徴付けられるガウス分布として表現されます。

この確率的表現により、単語の意味の「不確実性」や「多義性」を自然にモデル化できます。また、分布間の類似度を測る方法（KLダイバージェンス、内積など）を用いることで、単語間の意味的関係をより豊かに表現できます。特に、**非対称な類似度**（例：「犬」は「動物」に似ているが、その逆は必ずしも真ではない）を自然に捉えることができる点が特徴的です。

### 新規性

この手法の主な貢献は以下の点です：

* **確率分布としての単語表現**: 単語を点ではなく確率分布として表現することで、意味の不確実性や広がりを明示的にモデル化します。
* **非対称な類似度**: KLダイバージェンスなどの非対称な尺度により、包含関係や階層性を捉えることができます。
* **多義性と曖昧性**: 分布の分散（uncertainty）により、単語の意味の曖昧さや一般性を表現できます。
* **柔軟な距離尺度**: 複数の確率的距離（KLダイバージェンス、内積、ワッサースタイン距離など）を使い分けることができます。

### 理論/手法の核心

#### 1. ガウス埋め込みの定義

各単語 $w$ を、$d$ 次元ガウス分布 $\mathcal{N}(\boldsymbol{\mu}_w, \boldsymbol{\Sigma}_w)$ として表現します：

$$
p(\mathbf{x} \mid w) = \mathcal{N}(\mathbf{x}; \boldsymbol{\mu}_w, \boldsymbol{\Sigma}_w) = \frac{1}{(2\pi)^{d/2} |\boldsymbol{\Sigma}_w|^{1/2}} \exp\left(-\frac{1}{2}(\mathbf{x} - \boldsymbol{\mu}_w)^T \boldsymbol{\Sigma}_w^{-1} (\mathbf{x} - \boldsymbol{\mu}_w)\right)
$$

ここで：
* $\boldsymbol{\mu}_w \in \mathbb{R}^d$ は平均ベクトル（単語の「中心的な意味」を表す）
* $\boldsymbol{\Sigma}_w \in \mathbb{R}^{d \times d}$ は共分散行列（単語の「意味の広がり」や「不確実性」を表す）

**実装上の簡略化**:
完全な共分散行列は $O(d^2)$ のパラメータを必要とするため、通常は**対角共分散行列**を使用します：
$$
\boldsymbol{\Sigma}_w = \text{diag}(\sigma_{w,1}^2, \ldots, \sigma_{w,d}^2)
$$

これにより、パラメータ数を $O(d)$ に削減できます。

#### 2. 分布間の類似度尺度

**（1）KLダイバージェンス（非対称）**:

2つのガウス分布間のKLダイバージェンスは、解析的に計算できます：

$$
\text{KL}(\mathcal{N}(\boldsymbol{\mu}_1, \boldsymbol{\Sigma}_1) \| \mathcal{N}(\boldsymbol{\mu}_2, \boldsymbol{\Sigma}_2)) = \frac{1}{2} \left[ \log \frac{|\boldsymbol{\Sigma}_2|}{|\boldsymbol{\Sigma}_1|} - d + \text{tr}(\boldsymbol{\Sigma}_2^{-1}\boldsymbol{\Sigma}_1) + (\boldsymbol{\mu}_2 - \boldsymbol{\mu}_1)^T \boldsymbol{\Sigma}_2^{-1} (\boldsymbol{\mu}_2 - \boldsymbol{\mu}_1) \right]
$$

KLダイバージェンスは非対称であるため、$\text{KL}(p \| q) \neq \text{KL}(q \| p)$ です。これにより、「犬→動物」と「動物→犬」の関係の違いを捉えることができます。

**（2）期待値尤度カーネル（対称）**:

$$
K(w_1, w_2) = \mathbb{E}_{\mathbf{x} \sim \mathcal{N}(\boldsymbol{\mu}_1, \boldsymbol{\Sigma}_1)}[p(\mathbf{x} \mid w_2)]
$$

これは、分布 $w_1$ からサンプルした点が分布 $w_2$ のもとでどれだけ尤もらしいかを表します。

2つのガウス分布に対して、これも解析的に計算できます：

$$
K(w_1, w_2) = \mathcal{N}(\boldsymbol{\mu}_1; \boldsymbol{\mu}_2, \boldsymbol{\Sigma}_1 + \boldsymbol{\Sigma}_2)
$$

この尺度は対称的です：$K(w_1, w_2) = K(w_2, w_1)$。

**（3）内積（非対称）**:

ガウス分布の内積を以下のように定義します：

$$
\langle p_1, p_2 \rangle = \int p_1(\mathbf{x}) p_2(\mathbf{x}) d\mathbf{x}
$$

2つのガウス分布の内積は：

$$
\langle \mathcal{N}(\boldsymbol{\mu}_1, \boldsymbol{\Sigma}_1), \mathcal{N}(\boldsymbol{\mu}_2, \boldsymbol{\Sigma}_2) \rangle \propto \mathcal{N}(\boldsymbol{\mu}_1; \boldsymbol{\mu}_2, \boldsymbol{\Sigma}_1 + \boldsymbol{\Sigma}_2)
$$

#### 3. エネルギーベースのSkip-gram目的関数

Word2Vecのように、文脈予測タスクを通じて埋め込みを学習します。単語 $w$ が与えられたときの文脈単語 $c$ の条件付き確率を以下のようにモデル化します：

$$
p(c \mid w) = \frac{\exp(E(w, c))}{\sum_{c'} \exp(E(w, c'))}
$$

ここで、$E(w, c)$ は単語 $w$ と文脈 $c$ のエネルギー関数（類似度）です。

**エネルギー関数の選択**:
* KLダイバージェンス：$E(w, c) = -\text{KL}(\mathcal{N}_w \| \mathcal{N}_c)$
* 内積：$E(w, c) = \langle p_w, p_c \rangle$
* 期待値尤度：$E(w, c) = K(w, c)$

**ネガティブサンプリング**:
計算効率のため、Word2Vecと同様にネガティブサンプリングを使用します：

$$
\log \sigma(E(w, c)) + \sum_{i=1}^{k} \mathbb{E}_{c_i \sim P_n}[\log \sigma(-E(w, c_i))]
$$

ここで、$\sigma$ はシグモイド関数、$P_n$ はノイズ分布です。

#### 4. パラメータの更新

**平均 $\boldsymbol{\mu}$ の更新**:
平均ベクトルは、通常のベクトル埋め込みと同様に勾配降下法で更新できます。

**共分散 $\boldsymbol{\Sigma}$ の更新**:
共分散行列は正定値対称行列である必要があります。これを保証するために、以下のパラメータ化を使用します：

$$
\boldsymbol{\Sigma} = \mathbf{LL}^T + \epsilon \mathbf{I}
$$

ここで、$\mathbf{L}$ は下三角行列、$\epsilon$ は小さな正の定数です。

対角共分散の場合、$\sigma_i = \text{softplus}(\tilde{\sigma}_i) + \epsilon$ のようにパラメータ化します。

#### 5. 解釈

**分散の意味**:
* **小さい分散**：具体的で特定の意味を持つ単語（例："penguin", "Paris"）
* **大きい分散**：抽象的で一般的な意味を持つ単語（例："thing", "entity", "concept"）

**非対称性の例**:
* $\text{KL}(\text{"dog"} \| \text{"animal"})$ は小さい（犬は動物に近い）
* $\text{KL}(\text{"animal"} \| \text{"dog"})$ は大きい（動物は犬に近いとは限らない）

これは、包含関係や階層性を捉えるのに適しています。

### 「キモ」と重要性

ガウス埋め込みの「キモ」は、**単語の意味を確率分布として表現することで、不確実性、階層性、非対称性を統一的にモデル化する**という点にあります。

**重要性**:

1. **不確実性の明示的表現**:
   * 従来のベクトル埋め込みは、単語の意味を「確定的な点」として扱います
   * ガウス埋め込みは、分散によって意味の「広がり」や「曖昧さ」を表現できます
   * これは、多義語や抽象概念の表現に特に有効です

2. **階層的関係の捉え方**:
   * 一般的な概念（"animal"）は大きな分散を持ち、具体的な概念（"golden retriever"）は小さな分散を持ちます
   * KLダイバージェンスの非対称性により、階層的な包含関係を自然に表現できます

3. **非対称な意味関係**:
   * "A is similar to B" と "B is similar to A" は、必ずしも同じ程度ではありません
   * 例：「コップはグラスに似ている」は真だが、「グラスはコップに似ている」はやや異なる
   * KLダイバージェンスなどの非対称尺度により、これを捉えることができます

4. **理論的な美しさ**:
   * ガウス分布は解析的に扱いやすく、多くの演算（KL、内積など）が閉形式で計算できます
   * 中心極限定理により、多くの自然現象がガウス分布で近似できるという統計的な裏付けがあります

5. **多義性への対応**:
   * 単一のガウス分布では多義性を直接扱えませんが、**ガウス混合モデル**（Gaussian Mixture Model, GMM）に拡張することで対応可能です
   * 例："bank" = 0.5 × ガウス1（金融） + 0.5 × ガウス2（川岸）

6. **後続研究への影響**:
   * **Poincaré embeddings**：双曲空間でのガウス分布
   * **Box embeddings**：ボックスとガウスのハイブリッド
   * **確率的知識グラフ埋め込み**：エンティティと関係を分布として表現
   * **VAE・生成モデル**：潜在変数をガウス分布として扱う技術との接続

7. **実用上の利点**:
   * **不確実性の定量化**：モデルの予測に対する「信頼度」を評価できます
   * **アウトライア検出**：低尤度の単語を異常として検出できます
   * **ベイズ推論との統合**：確率的モデリングの枠組みと自然に統合できます

**課題と限界**:
* **計算コスト**：分散パラメータを追加するため、通常のベクトル埋め込みの2倍のパラメータが必要
* **最適化の難しさ**：共分散行列の正定値性を保つための制約が必要
* **多義性の限界**：単一ガウスでは、明確に異なる複数の意味（多義性）を直接表現できない（混合モデルが必要）

**実験結果**:
論文では、単語類似度タスクや含意検出タスクにおいて、従来のベクトル埋め込みと同等以上の性能を達成しつつ、階層的関係や非対称性をより適切に捉えることができることが示されています。

ガウス埋め込みは、単語埋め込みの表現力を拡張し、言語の意味構造の複雑さをより忠実にモデル化する重要な一歩となりました。特に、不確実性や階層性が重要となる知識表現や推論タスクにおいて、大きな可能性を持つアプローチです。

