# ELMo：深い文脈化された単語表現

**著者**:
* Matthew E. Peters (Allen Institute for Artificial Intelligence)
* Mark Neumann (Allen Institute for Artificial Intelligence)
* Mohit Iyyer (Allen Institute for Artificial Intelligence)
* Matt Gardner (Allen Institute for Artificial Intelligence)
* Christopher Clark (University of Washington)
* Kenton Lee (University of Washington)
* Luke Zettlemoyer (Allen Institute for Artificial Intelligence / University of Washington)

**出版**: North American Chapter of the Association for Computational Linguistics (NAACL 2018)

**URL**: [Deep contextualized word representations](https://arxiv.org/abs/1802.05365)

**プロジェクトページ**: [https://allennlp.org/elmo](https://allennlp.org/elmo)

---

### 概要

ELMo（Embeddings from Language Models）は、**深い文脈化された単語表現（deep contextualized word representations）**を生成する画期的な手法です。従来のWord2VecやGloVeなどの単語埋め込みは、各単語に対して文脈に依存しない固定のベクトルを割り当てていましたが、ELMoは**文脈に応じて動的に変化する単語表現**を生成します。

ELMoは、大規模コーパスで事前学習された双方向LSTM（bidirectional LSTM）言語モデルの内部状態から、各単語の表現を抽出します。この表現は、単語の統語的・意味的な情報だけでなく、文脈に依存する意味（多義性）も捉えることができます。ELMoを既存のモデルに組み込むだけで、質問応答、感情分析、固有表現認識など、6つの異なるNLPタスクで当時の最先端性能を達成しました。

### 新規性

ELMoの主な貢献は以下の点です：

* **文脈依存の単語表現**: 同じ単語でも、文脈によって異なる表現を持つことができます。これにより、多義性（polysemy）の問題を自然に解決できます。
* **深い双方向モデル**: 複数層の双方向LSTMを使用することで、単語の深い文脈情報を捉えます。
* **階層的な表現の活用**: 言語モデルの異なる層からの情報を組み合わせることで、統語的情報と意味的情報の両方を活用します。
* **転移学習の効果**: 一度学習したELMo表現は、様々な下流タスクに適用可能で、大幅な性能向上をもたらします。
* **Feature-based転移学習**: モデル全体をファインチューニングするのではなく、事前学習済みの表現を特徴量として使用する、というアプローチを示しました（その後のBERTとは対照的）。

### 理論/手法の核心

#### 1. 双方向言語モデル（biLM）

ELMoの基盤は、深い双方向LSTM言語モデルです。

**順方向言語モデル（forward LM）**:
長さ $N$ のトークン列 $(t_1, t_2, \ldots, t_N)$ が与えられたとき、順方向LMは各位置 $k$ において、履歴 $(t_1, \ldots, t_{k-1})$ が与えられたときのトークン $t_k$ の確率をモデル化します：

$$
p(t_1, t_2, \ldots, t_N) = \prod_{k=1}^{N} p(t_k \mid t_1, t_2, \ldots, t_{k-1})
$$

**逆方向言語モデル（backward LM）**:
同様に、逆方向LMは未来の文脈から過去を予測します：

$$
p(t_1, t_2, \ldots, t_N) = \prod_{k=1}^{N} p(t_k \mid t_{k+1}, t_{k+2}, \ldots, t_N)
$$

**双方向言語モデル（biLM）**:
ELMoは両方向のLMを結合し、以下の目的関数を最大化します：

$$
\sum_{k=1}^{N} \left( \log p(t_k \mid t_1, \ldots, t_{k-1}; \Theta_x, \overrightarrow{\Theta}_{\text{LSTM}}, \Theta_s) + \log p(t_k \mid t_{k+1}, \ldots, t_N; \Theta_x, \overleftarrow{\Theta}_{\text{LSTM}}, \Theta_s) \right)
$$

ここで：
* $\Theta_x$ は入力トークン表現（文字CNNの後述参照）のパラメータ
* $\overrightarrow{\Theta}_{\text{LSTM}}$ と $\overleftarrow{\Theta}_{\text{LSTM}}$ はそれぞれ順方向と逆方向のLSTMのパラメータ
* $\Theta_s$ はソフトマックス層のパラメータ

**重要な設計選択**: 順方向と逆方向のLSTMはパラメータを共有せず、それぞれ独立に学習されます。ただし、トークン表現（$\Theta_x$）とソフトマックス層（$\Theta_s$）は共有されます。

#### 2. ELMo表現の構築

biLMが学習されると、各トークン $t_k$ について、以下の表現が得られます：

* 入力層（文字ベースの表現）：$\mathbf{x}_k^{\text{LM}}$
* 各LSTM層の隠れ状態：
  * 層 $j$ の順方向隠れ状態：$\overrightarrow{\mathbf{h}}_{k,j}^{\text{LM}}$
  * 層 $j$ の逆方向隠れ状態：$\overleftarrow{\mathbf{h}}_{k,j}^{\text{LM}}$

$L$ 層のbiLMでは、各トークン $t_k$ に対して $(2L + 1)$ 個の表現が得られます（入力層 + 各層の2方向）：

$$
R_k = \{\mathbf{x}_k^{\text{LM}}, \overrightarrow{\mathbf{h}}_{k,j}^{\text{LM}}, \overleftarrow{\mathbf{h}}_{k,j}^{\text{LM}} \mid j = 1, \ldots, L\}
= \{\mathbf{h}_{k,j}^{\text{LM}} \mid j = 0, \ldots, L\}
$$

ここで、$\mathbf{h}_{k,j}^{\text{LM}} = [\overrightarrow{\mathbf{h}}_{k,j}^{\text{LM}}; \overleftarrow{\mathbf{h}}_{k,j}^{\text{LM}}]$ は両方向の連結です。

**ELMoの定義**:
ELMoは、これらすべての層の表現を、**タスク固有の重みで線形結合**したものです：

$$
\text{ELMo}_k^{\text{task}} = E(R_k; \Theta^{\text{task}}) = \gamma^{\text{task}} \sum_{j=0}^{L} s_j^{\text{task}} \mathbf{h}_{k,j}^{\text{LM}}
$$

ここで：
* $s^{\text{task}} = (s_0^{\text{task}}, \ldots, s_L^{\text{task}})$ はソフトマックスで正規化された重み
* $\gamma^{\text{task}}$ は全体のスケーリングパラメータ

重み $s_j^{\text{task}}$ と $\gamma^{\text{task}}$ は、下流タスクの学習時に学習されます。

#### 3. 文字ベースの入力表現

ELMoは、FastTextと同様に、**文字レベルのCNN**を使用して単語の初期表現を生成します：

1. 各単語を文字列として扱う
2. 文字埋め込みを適用
3. 畳み込み層を適用（複数のフィルタサイズ）
4. max-poolingで固定長ベクトルを生成
5. 2層のhighway network を適用

この設計により、未知語にも対応でき、形態論的情報も捉えることができます。

#### 4. 下流タスクへの適用

ELMoを既存のモデルに組み込む方法は非常にシンプルです：

1. 事前学習済みのbiLMを用意
2. 入力文のトークン列に対してELMo表現を計算
3. ELMo表現を既存モデルの入力に連結：

$$
[\mathbf{x}_k; \text{ELMo}_k^{\text{task}}]
$$

4. 下流タスクの学習中に、重み $s^{\text{task}}$ と $\gamma^{\text{task}}$ も一緒に学習

オプションとして、モデルの出力層にもELMoを追加することができます。

#### 5. 実装の詳細

論文で使用されたbiLMの構成：
* **層数**: 2層の双方向LSTM
* **隠れ状態サイズ**: 各方向4096次元（合計8192次元）
* **文字CNNの出力**: 512次元（投影後）
* **学習データ**: 1B Word Benchmark（約8億単語）
* **パラメータ数**: 約93.6M

学習には4つのGPUで約2週間かかりました。

### 「キモ」と重要性

ELMoの「キモ」は、**単語の意味は固定されたものではなく、文脈によって動的に変化するものである**という言語の本質的な性質を、計算モデルで実現した点にあります。

**重要性**:

1. **多義性の解決**:
   * 例："bank"という単語は「銀行」と「土手」の2つの意味を持ちますが、従来の埋め込みでは1つのベクトルしか持てませんでした
   * ELMoでは、"bank"が"river bank"の文脈に現れる場合と"financial bank"の文脈に現れる場合で、異なる表現を生成できます

2. **深い言語理解**:
   * 異なる層が異なる種類の情報を捉えることが分析で示されています：
     * 低層：統語的情報（品詞タグ、構文構造など）
     * 高層：意味的情報（語義、意味的役割など）
   * タスクに応じて、適切な層の情報に重点を置くことができます

3. **転移学習の効果**:
   * 6つの異なるNLPタスクで大幅な性能向上を達成：
     * SQuAD（質問応答）：+4.7% F1
     * SNLI（含意関係）：+0.7% accuracy
     * SRL（意味役割ラベリング）：+3.2% F1
     * Coref（共参照解析）：+3.2% F1
     * NER（固有表現認識）：+0.7% F1
     * SST-5（感情分析）：+1.0% accuracy

4. **Feature-based転移学習のパラダイム**:
   * ELMoは、事前学習済みのモデルから「特徴量」を抽出し、それを既存モデルに追加するというアプローチを示しました
   * これは、モデル全体をファインチューニングするBERTのアプローチとは異なりますが、実用上の利点があります（既存システムに容易に統合可能）

5. **BERTへの道**:
   * ELMoの成功は、事前学習された深い言語モデルが多様なNLPタスクに有効であることを実証しました
   * これは、数ヶ月後に登場するBERT（双方向Transformer）の開発を促す重要な前駆となりました
   * LSTM vs Transformer、Feature-based vs Fine-tuning という違いはありますが、「大規模コーパスで事前学習した深い文脈モデル」という基本思想は共通です

6. **言語の複雑性への対応**:
   * 単語の意味が文脈依存であることを明示的にモデル化することで、言語の本質的な複雑さにより適切に対応できるようになりました

7. **実用性**:
   * 公開された事前学習済みモデルにより、研究者や実務家が容易に最先端の性能を達成できるようになりました
   * AllenNLPライブラリとの統合により、使いやすさも確保されています

**歴史的意義**:
ELMoは、「静的埋め込み」から「動的・文脈化された埋め込み」へのパラダイムシフトを象徴する論文です。この転換は、その後のNLP研究の方向性を決定づけ、BERT、GPT、RoBERTaなどの現代的な事前学習言語モデルの基礎を築きました。ELMoの成功により、「大規模コーパスでの教師なし事前学習 → 特定タスクでの利用」という、現在のNLPの標準的なワークフローが確立されました。

