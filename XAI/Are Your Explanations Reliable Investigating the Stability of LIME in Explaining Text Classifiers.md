# Are Your Explanations Reliable? Investigating the Stability of LIME in Explaining Text Classifiers

**著者**: Ethan Burger, Arushi Arora, Xi Chen, Junyi Jessy Li  
**会議**: Proceedings of the 2023 Conference on Empirical Methods in Natural Language Processing (EMNLP 2023)  
**年**: 2023  
**ページ**: 12824--12840  
**URL**: [https://aclanthology.org/2023.emnlp-main.792](https://aclanthology.org/2023.emnlp-main.792)

---

### 概要

本論文は、**LIME (Local Interpretable Model-agnostic Explanations) の安定性（stability）**をテキスト分類タスクにおいて調査した研究です。LIMEは広く使われている説明可能性手法ですが、テキストデータにおいて**類似した入力に対して一貫性のない説明を生成する**という問題があることを明らかにしました。さらに、この不安定性を悪用する**XAIFooler**というアルゴリズムを提案し、LIMEの説明を操作可能であることを実証しました。

---

### 1. 新規性：この論文の最も大きな貢献と、既存研究と比べて何が新しいのか

**主な貢献**:

1. **LIMEの不安定性の実証**: テキスト分類タスクにおいて、LIMEが類似した入力に対して一貫性のない説明を生成することを初めて体系的に実証しました。これは、医療や金融などの重要なアプリケーションにおける信頼性を損なう重大な問題です。

2. **XAIFoolerアルゴリズムの提案**: LIMEの安定性をテキスト摂動最適化問題として定式化し、**XAIFooler**という新しいアルゴリズムを提案しました。このアルゴリズムは、意味を保持しながらテキスト入力を摂動させ、LIMEの説明を操作することができます。

3. **Rank-biased Overlap (RBO) の活用**: 説明の類似性を測定するために、**Rank-biased Overlap (RBO)** を類似度指標として使用し、最適化プロセスを導く手法を提案しました。

**既存研究との差分**:

- 従来の研究では、LIMEの不安定性は画像データなどで指摘されていましたが、テキストデータにおける体系的な調査は行われていませんでした。
- 本論文は、XAI（説明可能AI）とadversarial attackの観点を組み合わせ、説明の安定性を攻撃可能な問題として定式化した点が新規です。

---

### 2. 理論/手法の核心：提案されている理論や手法の要点

#### 2.1 LIMEの不安定性の問題

LIMEは、ブラックボックスモデルの予測を説明するために、入力の近傍で解釈可能な局所モデル（通常は線形モデル）を学習します。しかし、テキストデータにおいて：

- **サンプリングの非決定性**: LIMEは近傍サンプルをランダムに生成するため、同じ入力でも異なる説明が生成される可能性があります。
- **特徴選択の不安定性**: テキストの特徴（単語の有無）の選択が不安定で、類似した入力に対して異なる重要単語を提示することがあります。

#### 2.2 XAIFoolerアルゴリズム

XAIFoolerは、以下の最適化問題を解きます：

**目的**: 元の入力 \(x\) に対して、意味を保持し、モデルの予測を変更せずに、LIMEの説明を最大限に操作する摂動 \(x'\) を見つける。

**制約条件**:
- **意味保持**: 摂動後のテキスト \(x'\) は元のテキスト \(x\) と意味的に類似している必要がある。
- **予測保持**: ブラックボックスモデル \(f\) の予測が変更されない（\(f(x) = f(x')\)）。

**最適化目標**: LIMEの説明のランキングを変更する（Rank-biased Overlap (RBO) を使用）。

#### 2.3 Rank-biased Overlap (RBO)

RBOは、2つのランキング間の類似性を測定する指標で、上位の要素により大きな重みを付与します：

\[
\text{RBO}(S, T, p) = (1-p) \sum_{d=1}^{\infty} p^{d-1} \cdot \frac{|S_{1:d} \cap T_{1:d}|}{d}
\]

ここで、\(S\) と \(T\) は2つのランキング、\(p\) は重みパラメータ（通常0.9）、\(S_{1:d}\) は上位 \(d\) 個の要素のセットです。

---

### 3. 実験結果と評価

#### 3.1 データセット

- 実世界のテキスト分類データセットを使用
- 感情分析、トピック分類などのタスク

#### 3.2 主要な発見

1. **LIMEの不安定性の実証**: 類似した入力に対して、LIMEは一貫性のない説明を生成することが確認されました。

2. **XAIFoolerの有効性**: XAIFoolerは、ベースライン手法と比較して、LIMEの説明をより効果的に操作しながら、高い意味保持率を維持しました。

3. **実用上の影響**: この結果は、LIMEの説明が信頼できない可能性があることを示し、重要な意思決定の場面での使用に注意が必要であることを示唆しています。

---

### 4. 意義と今後の展開

**意義**:

- **説明可能性手法の評価**: 説明可能性手法の安定性を評価する重要性を示しました。
- **セキュリティへの示唆**: 説明可能性手法が攻撃可能であることを示し、セキュリティ上の懸念を提起しました。

**今後の展開**:

- より安定したLIMEの変種（例: GLIME）の開発
- 説明可能性手法の堅牢性評価の標準化
- テキストデータにおける説明の安定性を保証する手法の開発

---

### 参考文献

- Burger, E., Arora, A., Chen, X., & Li, J. J. (2023). Are Your Explanations Reliable? Investigating the Stability of LIME in Explaining Text Classifiers. *Proceedings of the 2023 Conference on Empirical Methods in Natural Language Processing*, 12824--12840.
- Ribeiro, M. T., Singh, S., & Guestrin, C. (2016). "Why Should I Trust You?": Explaining the Predictions of Any Classifier. *Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining*, 1135--1144.
