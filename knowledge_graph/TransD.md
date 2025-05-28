# TransD: 動的マッピング行列による知識グラフ埋め込み

**著者**:
* Guoliang Ji (Institute of Automation, Chinese Academy of Sciences, Beijing, China)
* Shizhu He (Institute of Automation, Chinese Academy of Sciences, Beijing, China)
* Liheng Xu (Institute of Automation, Chinese Academy of Sciences, Beijing, China)
* Kang Liu (Institute of Automation, Chinese Academy of Sciences, Beijing, China)
* Jun Zhao (Institute of Automation, Chinese Academy of Sciences, Beijing, China)

**出版**: Proceedings of the 53rd Annual Meeting of the Association for Computational Linguistics and the 7th International Joint Conference on Natural Language Processing (ACL-IJCNLP 2015)
**URL**: [Knowledge Graph Embedding via Dynamic Mapping Matrix](https://aclanthology.org/P15-1067.pdf)

---

### 概要

この論文では、知識グラフの埋め込みにおけるエンティティと関係の多様性を同時に考慮する新しいモデル「TransD（Translating with Dynamic mapping matrix）」を提案しています。TransDは、TransR/CTransRの改良版として位置づけられ、エンティティを関係固有の空間に投影する際の「マッピング行列」を、エンティティと関係それぞれの情報を動的に組み合わせて構築します。これにより、TransR/CTransRに比べてパラメータ数を削減しつつ、行列-ベクトル積の計算を回避できるため、大規模な知識グラフにもより効率的に適用できるモデルとなっています。実験により、TransDがトリプル分類とリンク予測の両タスクにおいて、既存のTransE、TransH、TransR/CTransRといったモデルを上回る性能を達成していることが示されています。

### 新規性

TransDの最も重要な新規性は、**エンティティと関係の両方の多様性を考慮した「動的なマッピング行列」を構築する**点にあります。先行研究であるTransRは、関係ごとに固定の投影行列 $\mathbf{M}_r$ を用いてエンティティを関係空間に投影していました。しかし、このアプローチには以下の問題がありました。

* **エンティティの多様性の無視**: $\mathbf{M}_r$ は関係にのみ依存するため、関係空間に投影されるエンティティごとの固有の性質（多様性）を十分に考慮できませんでした。
* **パラメータ数の多さ**: 関係の種類が多い場合、各関係に独立した行列を学習する必要があるため、パラメータ数が爆発的に増え、計算コストが増大するという問題がありました。
* **行列-ベクトル積の計算コスト**: 投影操作で行列-ベクトル積が必要となるため、計算が重くなる傾向がありました。

TransDはこれらの問題を解決するために、各エンティティと各関係にそれぞれ「意味ベクトル」と「投影ベクトル」の2つのベクトルを持たせることで、**エンティティ-関係のペアごとに異なる動的な投影行列を生成する**ことを可能にしました。これにより、エンティティと関係の両方の多様性を同時に捉え、かつTransRよりも少ないパラメータ数と計算コスト（行列-ベクトル積の回避）でより柔軟な投影を実現しています。

### 理論/手法の核心

TransDモデルでは、各エンティティ $e$ と各関係 $r$ が、それぞれ2つのベクトルで表現されます。

1.  **エンティティのベクトル**:
    * 意味ベクトル: $\mathbf{e} \in \mathbb{R}^n$
    * 投影ベクトル: $\mathbf{e}_p \in \mathbb{R}^n$
2.  **関係のベクトル**:
    * 意味ベクトル: $\mathbf{r} \in \mathbb{R}^m$
    * 投影ベクトル: $\mathbf{r}_p \in \mathbb{R}^m$

ここで、$n$ はエンティティ空間の次元、$m$ は関係空間の次元であり、通常 $n$ と $m$ は異なる値をとることが許されます。

**動的なマッピング行列の構築とエンティティの投影**:
トリプル $(h, r, t)$ が与えられたとき、ヘッドエンティティ $\mathbf{h}$ とテールエンティティ $\mathbf{t}$ を関係空間 $\mathbb{R}^m$ に投影するためのマッピング行列 $\mathbf{M}_{rh}$ および $\mathbf{M}_{rt}$ を動的に構築します。これらのマッピング行列は、ヘッドエンティティの意味ベクトル $\mathbf{h}$ とその投影ベクトル $\mathbf{h}_p$、および関係の意味ベクトル $\mathbf{r}$ とその投影ベクトル $\mathbf{r}_p$ を用いて以下のように定義されます。

* ヘッドエンティティの投影行列:
    $$
    \mathbf{M}_{rh} = \mathbf{r}_p \mathbf{h}_p^T + \mathbf{I}_{m \times n}
    $$
* テールエンティティの投影行列:
    $$
    \mathbf{M}_{rt} = \mathbf{r}_p \mathbf{t}_p^T + \mathbf{I}_{m \times n}
    $$
    ここで、$\mathbf{I}_{m \times n}$ は適切なサイズの単位行列（またはゼロ行列）です。この定義により、投影行列はエンティティと関係の両方の投影ベクトルに基づいて動的に生成され、両者の相互作用がより細かく考慮されます。

投影されたエンティティベクトルは以下のように計算されます。
* ヘッドエンティティの投影:
    $$
    \mathbf{h}_{\perp r} = \mathbf{M}_{rh} \mathbf{h} = (\mathbf{r}_p \mathbf{h}_p^T + \mathbf{I}_{m \times n}) \mathbf{h} = \mathbf{r}_p (\mathbf{h}_p^T \mathbf{h}) + \mathbf{h}
    $$
* テールエンティティの投影:
    $$
    \mathbf{t}_{\perp r} = \mathbf{M}_{rt} \mathbf{t} = (\mathbf{r}_p \mathbf{t}_p^T + \mathbf{I}_{m \times n}) \mathbf{t} = \mathbf{r}_p (\mathbf{t}_p^T \mathbf{t}) + \mathbf{t}
    $$
    この数式は、元の行列-ベクトル積をベクトル積とスカラー積の組み合わせで表現できることを示しており、計算効率の向上に寄与します。

**スコアリング関数**:
TransRと同様に、投影されたエンティティと関係ベクトルを用いて適合度を評価します。

$$f_r(\mathbf{h}, \mathbf{t}) = ||\mathbf{h}_{\perp r} + \mathbf{r} - \mathbf{t}_{\perp r}||_2^2$$
またはL1ノルムも使用可能です。

**損失関数**:
学習は、TransEやTransRと同様のマージンベースのランキング基準を最小化することで行われます。

$$\mathcal{L} = \sum_{(h, r, t) \in S} \sum_{(h', r', t') \in S'_{(h,r,t)}} [\gamma + f_r(\mathbf{h}, \mathbf{t}) - f_{r'}(\mathbf{h'}, \mathbf{t'})]_+$$
* $S$: 正しいトリプルの訓練セット。
* $S'_{(h,r,t)}$: 破損した（負例の）トリプルのセット。
* $[x]_+ = \max(0, x)$: ヒンジ損失。
* $\gamma > 0$: マージンハイパーパラメータ。

**制約**:
エンティティおよび関係の全てのベクトル（意味ベクトルと投影ベクトル）に対して、$L_2$ ノルムが1以下であるという制約 $||v||_2 \le 1$ が課せられます。これは、学習プロセスが自明な解に収束するのを防ぎ、埋め込み空間のスケールを安定させるために重要です。

### 「キモ」と重要性

TransDの「キモ」は、**エンティティと関係の「多様性」を同時に考慮し、エンティティと関係の投影ベクトルから動的にマッピング行列を生成する**という点にあります。これにより、エンティティが関係空間に投影される際に、そのエンティティ自体の特性と関係の特性の両方が反映されるようになります。

このアイデアの重要性は以下の点に集約されます。

* **より柔軟で正確なモデリング**: TransRが一つの関係に一つの固定された投影行列を使用していたのに対し、TransDはエンティティ-関係ペアごとに動的に投影行列を生成することで、エンティティの多面性と関係の多様性をよりきめ細かく捉えることが可能になりました。これは、知識グラフの複雑な構造をより正確に表現するために不可欠な要素です。
* **効率性とスケーラビリティの向上**: TransRの課題であったパラメータ数の多さと行列-ベクトル積の計算コストを解決しました。TransDでは、各エンティティと関係が追加で1つの投影ベクトルを持つだけで済むため、パラメータ数が大幅に削減されます。また、投影操作を行列-ベクトル積ではなく、ベクトル同士の積と和に変換することで、計算効率が向上し、非常に大規模な知識グラフにもより効率的に適用できるようになりました。
* **知識グラフ埋め込み研究の進化**: TransDは、TransEから始まった「並進」の概念に基づくモデル系列において、表現能力と計算効率のバランスをさらに改善した重要なステップとなりました。エンティティと関係の多様性を同時かつ効率的に考慮するこのアプローチは、その後の知識グラフ埋め込みモデルの開発に大きな影響を与え、より複雑な関係パターンやセマンティクスを捉える研究の基礎を築きました。

TransDは、そのシンプルさと効率性、そして優れた性能により、知識グラフの補完や関連タスクにおいて広く利用されるモデルの一つとなりました。
