### **[MNIST-MLP-2017](https://github.com/DFin/Neural-Network-Visualisation)**

David Finkelstein によって開発された、MLP を含むニューラルネットワークの挙動を **視覚的に確認できる可視化デモ**。

**可視化粒度**: 画素レベル、ニューロンレベル、レイヤーレベル

* 入力（MNIST 画素）から出力クラスまでの重み付き伝播
* 各ニューロンの活性化値の変化
* 学習に伴う重み・判断境界の変化
  MLP が入力特徴をどう組み合わせて分類に至るかを直感的に把握できる。



### **[InterpretML-2019](https://arxiv.org/abs/1909.09223)**

Microsoft Research によって開発された、**機械学習モデル解釈の統一フレームワーク**。

**可視化粒度**: トークン/特徴レベル、モデル全体レベル

* グローバル解釈（モデル全体の傾向）
* ローカル解釈（個別予測の理由）
* 線形・非線形・ブラックボックスモデル対応

NLP 特化ではないが、**解釈手法を体系的に整理・適用**できる基盤。



### **[AllenNLP Interpret-2019](https://github.com/allenai/allennlp)**

Allen Institute for AI によって開発された、**NLP モデル予測の解釈・分析フレームワーク**。

**可視化粒度**: トークンレベル

* トークン重要度のサリエンシーマップ
* 出力を変化させる最小入力変更（Adversarial）
* クラス判断に寄与した入力要素の可視化
  モデルの予測がどの入力要素に依存しているかを定量的に確認できる。



### **[Interactive Tools for ML-2019](https://github.com/Machine-Learning-Tokyo/Interactive_Tools)**

Machine Learning Tokyo によって公開された、**機械学習・深層学習・数学の概念を直感的に理解するための可視化ツール集**。

**可視化粒度**: ニューロンレベル、モデル全体レベル

* 勾配降下・最適化アルゴリズムの挙動
* ニューラルネットワークの学習過程
* 数学的概念（線形代数・確率）の動的可視化

モデル内部というより、**学習ダイナミクスや基礎概念の理解**に強い。



### **[ExBERT-2020](https://exbert.net/)**

Harvard NLP / MIT 研究者らによって開発された、**BERT の内部表現探索ツール**。

**可視化粒度**: トークンレベル、埋め込みレベル

* トークンごとの埋め込みベクトルの近傍探索
* Attention に基づく文脈依存表現の変化
* 類似表現・意味空間の可視化

BERT が獲得した意味表現を **埋め込み空間ベースで探索**できる。



### **[bertviz-2020](https://github.com/jessevig/bertviz)**

Jesse Vig によって開発された、**Transformer の Attention 構造を可視化するツール**。

**可視化粒度**: トークンレベル、ヘッドレベル、レイヤーレベル

* 各レイヤー・各ヘッドの Attention 重み
* トークン間の依存関係の構造
* 層ごとの注意パターンの違い
  文脈情報が Attention を通じてどのように集約されるかを観察できる。



### **[Attention Rollout-2020](https://arxiv.org/abs/2005.00928)**

Samira Abnar (現Apple)、Willem Zuidema（University of Amsterdam）によって提案された、**Transformer 内の入力トークンから高層埋め込みへの情報伝播を追跡する手法**。

**可視化粒度**: トークンレベル、レイヤーレベル

* 各層の注意重みを前層の注意重みと再帰的に掛け合わせて計算
* 残差接続を考慮した単位行列の加算と重みの再正規化
* 入力トークンの識別子が注意重みに基づいて線形的に結合されると仮定
  生の注意重みが深い層で均一化する問題を克服し、入力トークンから高層埋め込みまでの情報の流れを集中的に解析できる。



### **[LLM-Architecture-2020s](https://bbycroft.net/llm)**

Brendan Bycroft によって制作された、LLM（Transformer）アーキテクチャの構造を **網羅的に可視化したインタラクティブ資料**。

**可視化粒度**: モデル全体レベル、レイヤーレベル、トークン次元

* Embedding / Attention / MLP / Residual の全体構造
* トークン次元・層次元を含む計算の流れ
* LLM を巨大な計算グラフとして俯瞰する視点
  LLM の全体像を「一枚の構造物」として理解できる。



### **[Transformers Interpret-2021](https://www.reddit.com/r/MachineLearning/comments/lipl90)**

Reddit の r/MachineLearning コミュニティで共有された、**Transformer 向け説明可能 AI ツール群**（特定の大学・研究室に限定されないコミュニティ主導の提案）。

**可視化粒度**: トークンレベル

* Attention・勾配に基づくトークン寄与度
* 入力摂動による予測変化の解析
* 複数解釈手法の比較

Attention だけに依らない、**多角的な寄与度評価**を志向している。



### **[ecco-2021](https://github.com/jalammar/ecco)**

Jay Alammar によって開発された、**Transformer 系 NLP モデルの内部表現と寄与を実際の挙動を全て可視化して表示したツール**。

**可視化粒度**: トークンレベル、レイヤーレベル

* レイヤーごとの活性化パターン
* トークン単位の寄与度
* 表現が層を通して変換される様子
  モデル内部で意味表現がどのように形成・変化するかを観察できる。



### **[NeMo-2022](https://github.com/NVIDIA/NeMo)**

NVIDIA によって開発された、**大規模言語・音声モデルの構築と解析のためのフレームワーク**。

**可視化粒度**: レイヤーレベル、モジュールレベル

* モデルのモジュール構成とブロック単位の設計
* 中間層表現・出力挙動の解析
* 学習・推論過程における内部状態の確認
  大規模モデルを前提とした内部構造と挙動を把握できる。



### **[TransformerLens-2023](https://zenn.dev/50s_zerotohero/articles/19c46f9de55b54)**

Neel Nanda（Google DeepMind）らを中心とした **オープンソース研究コミュニティ（Mechanistic Interpretability）** によって開発された、Transformer 内部解析ライブラリ。

**可視化粒度**: ヘッドレベル、ニューロンレベル、レイヤーレベル

* GPT-2 などの Attention Head / Neuron の直接介入（patching）
* 特定ヘッド・MLP が出力に与える因果的影響
* 残差ストリーム上での情報の流れの分解

LLM の挙動を「ブラックボックスの可視化」ではなく、**機能単位での因果解析**として扱える。



### **[Interpretability Starter-2023](https://github.com/apartresearch/interpretability-starter)**

Apart Research によって公開された、**解釈性研究のためのスターターテンプレート集**。

**可視化粒度**: ニューロン/特徴レベル、回路レベル

* LLM 内部解析の実験コード例
* Feature attribution / activation analysis
* 回路・機能分解の初期実装

「ツール」というより、**研究を始めるための実践的足場**。



### **[Transformer Explainer-2024](https://github.com/poloclub/transformer-explainer)**

Georgia Tech の Polo Club of Data Science によって開発された、**テキスト生成 Transformer の内部動作を段階的に可視化するツール**。

**可視化粒度**: トークンレベル、レイヤーレベル、時間方向

* トークン生成ごとの確率分布の変化
* Attention・中間表現の逐次的推移
* 入力が次トークン予測に与える影響
  生成モデルの意思決定過程を時間方向に追える。



### **[LRP-eXplains-Transformers (LXT)-2024](https://github.com/rachtibat/LRP-eXplains-Transformers)**

Reduan Achtibat ら（Fraunhofer Heinrich-Hertz-Institute）によって開発された、**Transformer モデルに対する Layer-wise Relevance Propagation (LRP) による説明可能 AI ツール**（ICML 2024）。

**可視化粒度**: トークンレベル、ニューロンレベル、レイヤーレベル

* AttnLRP（Attention-Aware LRP）による入力トークン・内部ニューロンへの忠実なアトリビューション
* 単一の backward pass で計算可能な効率的な実装
* LLaMA 2/3、Gemma 3、Qwen 2、BERT、GPT-2、Vision Transformers など幅広いモデルに対応
* 潜在特徴のアトリビューションと可視化（ニューロン・SAE 特徴のラベリング、生成プロセスの制御）
  勾配ベース手法よりも忠実でノイズの少ない説明を提供し、Transformer 全体のブラックボックス性を低減できる。



### **[Awesome Transformer Visualization-2024](https://github.com/Ki-Seki/Awesome-Transformer-Visualization)**

オープンソースコミュニティによって整備された、**Transformer / LLM 可視化ツールの網羅的まとめ**。

**可視化粒度**: メタリソース（各種粒度のツールを網羅）

* Attention 可視化
* 内部表現・回路解析
* 教育用・研究用ツールの整理

分野全体を俯瞰するための **メタリソース**。



### **[Circuit Tracer-2025](https://github.com/safety-research/circuit-tracer)**

Anthropic によって開発された、**LLM の内部「思考回路（回路トレース）」を可視化・解析するツール**。

**可視化粒度**: トークンレベル、ニューロンレベル、ヘッドレベル、回路レベル

* 入力トークンから出力へのアトリビューション・グラフ
* Attention / MLP / Neuron を横断した回路グラフの可視化
* 特定特徴を操作した際の出力変化
  出力に至るまでの内部要素の因果的寄与を経路として追跡できる。
