# Attention is Not Only a Weight: Analyzing Transformers with Vector Norms

**著者**: Goro Kobayashi (Tohoku University), Tatsuki Kuribayashi (Tohoku University), Sho Yokoi (Tohoku University), Kentaro Inui (Tohoku University)
**会議**: Proceedings of the 2020 Conference on Empirical Methods in Natural Language Processing (EMNLP 2020)
**arXiv**: [arXiv:2004.10102](https://arxiv.org/abs/2004.10102)

この論文は、Transformerモデルの注意機構を、注意重みだけでなく**ベクトルノルム**も考慮して分析する手法を提案しています。

---

### 1. 新規性：この論文の最も大きな貢献と、既存研究と比べて何が新しいのか

この論文の最大の貢献は、Transformerモデルの注意機構の出力が**注意重みと変換された入力ベクトルのノルムの2つの要素**で決定されることを明らかにし、ノルムベースの分析を提案した点です。

既存研究の多くは、注意機構の分析において**注意重みのみ**に焦点を当てていました。しかし、注意機構の出力は以下のように計算されます：

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

この式から、出力は注意重み（softmaxの結果）だけでなく、値ベクトル$V$（変換された入力ベクトル）のノルムも影響を受けることがわかります。本論文は、この**ノルムの重要性**に着目し、注意重みだけでは見えない情報を明らかにしました。

### 2. 理論/手法の核心：提案されている理論や手法の要点

#### 注意機構におけるノルムの役割

Transformerモデルの注意機構では、各トークンの出力は以下のように計算されます：

$$\text{output}_i = \sum_j \alpha_{ij} \mathbf{v}_j$$

ここで：
- $\alpha_{ij}$: トークン$i$からトークン$j$への注意重み
- $\mathbf{v}_j$: トークン$j$の値ベクトル（変換された入力ベクトル）

この式から、出力は**注意重み$\alpha_{ij}$と値ベクトル$\mathbf{v}_j$のノルム$\|\mathbf{v}_j\|$の両方**に依存することがわかります。

#### ノルムベースの分析手法

本論文では、以下のようなノルムベースの分析を提案しています：

1. **ノルムの可視化**: 各トークンの値ベクトルのノルムを可視化することで、注意重みだけでは見えない情報を明らかにする
2. **ノルムと注意重みの相互作用**: ノルムと注意重みがどのように相互作用して出力を決定するかを分析する
3. **ノルムベースの重要度評価**: ノルムを考慮した新しい重要度評価手法を提案する

### 3. 「キモ」と重要性：この論文の核となるアイデアと分野への影響

* **論文の「キモ」 (核となるアイデア)**
    1. **注意重みだけでは不十分**: 注意機構の出力は注意重みだけでなく、値ベクトルのノルムも重要な役割を果たす
    2. **ノルムベースの分析**: ノルムを考慮することで、注意機構の内部動作をより深く理解できる

* **分野への重要性と影響**
    1. **BERTの分析**: BERTが特別なトークン（[CLS]、[SEP]など）に対して注意を払っていないことをノルムベースの分析で明らかにした
    2. **機械翻訳への応用**: Transformerベースの機械翻訳システムにおいて、ノルムベースの分析により合理的な単語アライメントを抽出できることを示した
    3. **注意機構の理解の深化**: 注意機構の解釈可能性に関する研究に新しい視点を提供

#### 主要な発見

1. **BERTにおける特別なトークン**: BERTは[CLS]や[SEP]などの特別なトークンに対して高い注意重みを割り当てるが、これらのトークンの値ベクトルのノルムは小さい。そのため、実際の出力への寄与は限定的であることが判明
2. **機械翻訳における単語アライメント**: Transformerの注意機構から、ノルムを考慮することで、より合理的な単語アライメントを抽出できることを示した

---

**参考文献**:
- Kobayashi, G., Kuribayashi, T., Yokoi, S., & Inui, K. (2020). Attention is Not Only a Weight: Analyzing Transformers with Vector Norms. Proceedings of the 2020 Conference on Empirical Methods in Natural Language Processing (EMNLP 2020). arXiv:2004.10102

