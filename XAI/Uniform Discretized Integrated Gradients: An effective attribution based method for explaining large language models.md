# Uniform Discretized Integrated Gradients: An effective attribution based method for explaining large language models

**著者**: Swarnava Sinha Roy, Ayan Kundu
**会議**: arXiv:2412.03886 [cs.CL] (Submitted on 5 Dec 2024)
**arXiv**: https://arxiv.org/abs/2412.03886

この論文は、大規模言語モデル（LLM）の説明可能性を向上させるために、離散的な特徴空間（単語埋め込み）に適した非線形経路を用いた「Uniform Discretized Integrated Gradients (UDIG)」という手法を提案しています。

---

### 1. 新規性：この論文の最も大きな貢献と、既存研究と比べて何が新しいのか

この論文の最大の貢献は、Integrated Gradients (IG) の**直線経路を非線形経路に置き換え**、離散的な単語埋め込み空間において中間点が実際の単語に近い位置に来るようにすることで、より適切なアトリビューションを生成する手法を提案した点です。

既存のIntegrated Gradients (IG)は、連続的な特徴空間ではうまく機能しますが、離散的な空間（単語埋め込みなど）では最適ではありません。IGはベースラインから入力までの**直線経路**に沿って勾配を積分しますが、この直線経路上の中間点は埋め込み空間内の実際の単語に対応しない可能性があります。UDIGは、**非線形経路**を採用し、中間点が埋め込み空間内の実際の単語に近い位置に来るようにすることで、言語モデルに対するより意味のある説明を提供します。

### 2. 理論/手法の核心：提案されている理論や手法の要点

Uniform Discretized Integrated Gradients (UDIG)の核心は、IGの直線経路を**非線形経路に置き換え**、離散的な単語埋め込み空間に適した補間戦略を採用することです。

#### Uniform Discretized Integrated Gradients (UDIG) の基本的な考え方

- **従来のIG**: ベースラインから入力までの**直線経路**に沿って勾配を積分
  $$\text{IG}_i(x) = (x_i - x'_i) \times \int_{\alpha=0}^{1} \frac{\partial F(x' + \alpha(x - x'))}{\partial x_i} d\alpha$$
  この直線経路上の中間点 $x' + \alpha(x - x')$ は、埋め込み空間内の実際の単語に対応しない可能性がある

- **UDIG**: **非線形経路**を採用し、中間点が埋め込み空間内の実際の単語に近い位置に来るようにする
  - 中間点を選択する際に、埋め込み空間内の実際の単語に近い位置を選ぶ
  - これにより、勾配を計算する際により意味のある中間表現を扱うことができる

#### UDIGの主な特徴

1. **非線形経路の採用**: 直線経路ではなく、埋め込み空間内の実際の単語に近い非線形経路を採用
2. **離散空間への適応**: 離散的な単語埋め込み空間の特性を考慮した補間戦略
3. **意味のある中間表現**: 中間点が実際の単語に対応するため、より解釈しやすい説明が可能

### 3. 「キモ」と重要性：この論文の核となるアイデアと分野への影響

* **論文の「キモ」 (核となるアイデア)**
    1. **非線形経路による適応**: 連続的な特徴空間を前提としたIGの直線経路を、離散的な単語埋め込み空間に適した非線形経路に置き換えることで、より意味のある説明を生成
    2. **実際の単語への近接性**: 中間点が埋め込み空間内の実際の単語に近い位置に来るようにすることで、モデルが学習した意味空間をより適切に反映

* **分野への重要性と影響**
    1. **大規模言語モデルへの適用**: 特に大規模言語モデル（LLM）に対して、より適切で解釈しやすい説明を提供
    2. **評価結果**: 感情分類（SST2, IMDb, Rotten Tomatoes）と質問応答（SQuAD）のタスクにおいて、Log odds、Comprehensiveness、Sufficiencyの3つのメトリクスで既存手法をほぼすべて上回る性能を示した
    3. **実用的な説明手法**: 離散的な特徴空間を持つ言語モデルに対して、より実用的で意味のある説明を提供する重要な拡張

#### 評価結果

- **タスク**: 
  - 感情分類: SST2, IMDb, Rotten Tomatoes
  - 質問応答: SQuAD（fine-tuned BERT）
- **評価メトリクス**: Log odds, Comprehensiveness, Sufficiency
- **結果**: 既存手法をほぼすべてのメトリクスで上回る性能を達成

---

**参考文献**:
- Sinha Roy, S., & Kundu, A. (2024). Uniform Discretized Integrated Gradients: An effective attribution based method for explaining large language models. arXiv:2412.03886 [cs.CL]. https://arxiv.org/abs/2412.03886

