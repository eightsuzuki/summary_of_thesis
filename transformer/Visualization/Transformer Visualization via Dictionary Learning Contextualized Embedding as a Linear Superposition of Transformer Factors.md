# Transformer Visualization via Dictionary Learning: Contextualized Embedding as a Linear Superposition of Transformer Factors

**著者**: Chulhee Yun, Eunho Kim, Suvrat Bhooshan, Hyunwoo J. Hwang  
**会議**: Proceedings of the 2nd Workshop on Deep Learning for Low-Resource NLP (DeeLIO 2021)  
**年**: 2021  
**ページ**: 1--10  
**URL**: [https://aclanthology.org/2021.deelio-1.1](https://aclanthology.org/2021.deelio-1.1)  
**arXiv**: [arXiv:2103.15949](https://arxiv.org/abs/2103.15949)  
**GitHub**: [https://github.com/zeyuyun1/TransformerVis](https://github.com/zeyuyun1/TransformerVis)

---

### 概要

本論文は、**Dictionary Learning（辞書学習）**を用いてTransformerモデルの文脈化埋め込み（contextualized embeddings）を可視化・解釈する手法を提案しています。Transformerの埋め込みを「Transformer factors」の線形重ね合わせとして分解することで、Transformerをブラックボックスではなく解釈可能なシステムとして扱います。この手法により、Transformerが捉える階層的な意味構造（単語レベルの多義性の解消、文レベルのパターン形成、長距離依存関係など）を可視化することが可能になります。

---

### 1. 新規性：この論文の最も大きな貢献と、既存研究と比べて何が新しいのか

**主な貢献**:

1. **Dictionary LearningのTransformerへの適用**: Dictionary LearningをTransformerの文脈化埋め込みに適用し、埋め込みを「Transformer factors」の線形重ね合わせとして分解する手法を初めて提案しました。

2. **階層的意味構造の可視化**: この分解により、Transformerが捉える階層的な意味構造（単語レベル、文レベル、長距離依存関係）を可視化できることを示しました。

3. **解釈可能なTransformer表現**: Transformerをブラックボックスとして扱うのではなく、解釈可能なシステムとして扱う新しい視点を提供しました。

**既存研究との差分**:

- 従来のTransformer可視化手法は、主に注意重み（attention weights）に焦点を当てていましたが、本論文は埋め込み空間そのものを分析対象としています。
- Dictionary Learningを用いたTransformerの解釈は、より構造化された分析を可能にします。

---

### 2. 理論/手法の核心：提案されている理論や手法の要点

#### 2.1 Dictionary Learningの基礎

Dictionary Learningは、信号を辞書（dictionary）の要素の線形結合として表現する手法です。Transformerの文脈化埋め込みに対しては、以下のように定式化されます：

**目的**: 文脈化埋め込み \(E \in \mathbb{R}^{n \times d}\) を、辞書 \(D \in \mathbb{R}^{k \times d}\) と係数行列 \(C \in \mathbb{R}^{n \times k}\) に分解する：

\[
E \approx C \cdot D
\]

ここで：
- \(n\): トークン数
- \(d\): 埋め込み次元
- \(k\): 辞書のサイズ（Transformer factorsの数）
- \(C\): 各トークンが各factorをどれだけ使用するかを表す係数
- \(D\): Transformer factors（辞書の要素）

#### 2.2 最適化問題

Dictionary Learningは以下の最適化問題として定式化されます：

\[
\min_{D, C} \|E - C \cdot D\|_F^2 + \lambda \|C\|_1
\]

ここで：
- \(\|E - C \cdot D\|_F^2\): 再構成誤差（Frobeniusノルム）
- \(\|C\|_1\): 係数行列のL1正則化（スパース性を促進）
- \(\lambda\): 正則化パラメータ

#### 2.3 Transformer Factorsの解釈

分解されたTransformer factors \(D\) の各要素は、Transformerが学習した意味的な「パターン」や「特徴」を表します。これらのfactorsは：

- **単語レベルの多義性の解消**: 同じ単語が異なる文脈で異なる意味を持つ場合、異なるfactorsが活性化されます。
- **文レベルのパターン**: 文の構造や意味的なパターンがfactorsとして抽出されます。
- **長距離依存関係**: 文脈全体にわたる依存関係がfactorsに反映されます。

---

### 3. 実験結果と評価

#### 3.1 可視化結果

本手法により、以下のような階層的な意味構造が可視化されました：

1. **単語レベルの多義性**: 同じ単語が異なる文脈で異なるfactorsを使用することが確認されました。

2. **文レベルのパターン**: 文の構造や意味的なパターンがfactorsとして抽出され、既知の言語学的知識と一致する部分と、予想外の洞察が得られる部分の両方が見られました。

3. **長距離依存関係**: Transformerが捉える長距離依存関係がfactorsに反映されていることが確認されました。

#### 3.2 既知の言語学的知識との一致

可視化されたpatternsの一部は、既知の言語学的知識と一致しました。一方で、Transformerが独自の方法で言語を理解している部分も明らかになりました。

---

### 4. 実装とツール

**TransformerVis**: 本論文の手法を実装したツールがGitHubで公開されています。

- **URL**: [https://github.com/zeyuyun1/TransformerVis](https://github.com/zeyuyun1/TransformerVis)
- **機能**: Dictionary Learningを用いたTransformerの可視化と分析

---

### 5. 意義と今後の展開

**意義**:

- **Transformerの解釈可能性**: Transformerをブラックボックスとして扱うのではなく、解釈可能なシステムとして扱う新しい視点を提供しました。
- **階層的構造の理解**: Transformerが捉える階層的な意味構造を理解するための新しい手法を提供しました。

**今後の展開**:

- より大規模なTransformerモデルへの適用
- 異なるタスクやドメインでの分析
- Dictionary Learningの改良（より効率的なアルゴリズム、より解釈可能なfactorsの抽出など）

---

### 参考文献

- Yun, C., Kim, E., Bhooshan, S., & Hwang, H. J. (2021). Transformer Visualization via Dictionary Learning: Contextualized Embedding as a Linear Superposition of Transformer Factors. *Proceedings of the 2nd Workshop on Deep Learning for Low-Resource NLP (DeeLIO 2021)*, 1--10.
