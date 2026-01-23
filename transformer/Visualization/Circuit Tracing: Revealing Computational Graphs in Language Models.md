# Circuit Tracing: Revealing Computational Graphs in Language Models

**著者**: Anthropic Research Team  
**arXiv**: [arXiv:2402.17764](https://arxiv.org/abs/2402.17764)

![alt text](./../Interpretability/image/Circuit%20Tracing:%20Revealing%20Computational%20Graphs%20in%20Language%20Models/image.png) 

## 1. 背景と目的

近年、大規模言語モデルに対しては**機械論的解釈性**（mechanistic interpretability）の確立が求められています。これは「モデル内部で実際に実行される計算グラフを人間が解釈できる形で再構築する」ことを意味します。従来はTransformerの「注意ヘッド」や「MLPブロック」を解析単位とし、それぞれの挙動を調べる手法が主流でした。しかし、これらのユニットは多様な機能を併せ持つため、個々の機能が混在し解釈の精度が限定的でした。

本研究では、この限界を打破するために**Cross‑Layer Transcoder (CLT)** を導入し、TransformerのMLP部分を「疎に発火する feature ベクトル」の集合として再構成します。CLTを用いることで、**小規模かつ明瞭な計算ユニット**を得るとともに、次の二つの解析軸を提供します：

1. **局所解析 (Attribution Graph)**: 各プロンプトにおける因果寄与を線形重み付き有向グラフとして可視化。  
2. **大域解析 (Global Weights)**: 全プロンプトに共通する仮想重みを抽出し、モデル全体の汎用回路構造を定量化。  

これにより「feature-level」の高解像度な解釈を実現します。 citeturn1view0

### 1.1 CLT導入の動機

- **層横断的相互作用の可視化**  
  従来のMLPでは各層が独立した重み行列を持つため、層間の情報結合がブラックボックス化します。CLTはEncoder–Decoder対を通じて異なる層の出力を齟齬なく結合し可視化可能にします。  
- **高解像度な feature-level 解析**  
  単一ヘッドやノードではなく、タスク特異的に活性化する小規模 feature を「原子ユニット」と見なし、その寄与を精密に追跡します。  
- **線形伝播強化**  
  Attribution Graph構築には線形伝播が前提です。CLTでは非線形性を JumpReLU と Error Node のみに限定し、Attention や LayerNorm を固定することで、寄与の厳密な算出を可能にします。

## 2. CLTによる置換モデルと線形化
![alt text](./../Interpretability/image/Circuit%20Tracing:%C2%A0Revealing%C2%A0Computational%C2%A0Graphs%C2%A0in%C2%A0Language%C2%A0Models/image-1.png)
### 2.1 Cross‑Layer Transcoder の構造

元のMLP \(f^\ell_{\mathrm{MLP}}\) を、以下の三段階で置換します：

1. **Encoder**  
   \[
     \mathbf z^\ell = W_{\mathrm{enc}}^\ell\,\mathbf x^\ell
   \]  
2. **JumpReLU**  
   \[
     \mathbf a^\ell = \mathrm{JumpReLU}(\mathbf z^\ell)
   \]  
3. **Decoder**  
   \[
     \hat{\mathbf y}^\ell = \sum_{\ell'=1}^\ell W_{\mathrm{dec}}^{\ell'\to\ell}\,\mathbf a^{\ell'}
   \]

- **JumpReLU**: 少数の要素を選択的に通過させる疎化活性化関数。  
- **学習目標**: \(f^\ell_{\mathrm{MLP}}(\mathbf x^\ell)\) を最小二乗誤差で再現しつつ、feature の平均活性度と非ゼロ数に対して sparsity penalty を課す。  
- **結果**: 置換後モデルは元モデルと同等の生成品質を維持し、最大50%のプロンプトで同一トークンを生成（可解釈性と性能の両立）。 citeturn1view0

### 2.2 局所置換モデル (Local Replacement Model)
![alt text](./../Interpretability/image/Circuit%20Tracing:%C2%A0Revealing%C2%A0Computational%C2%A0Graphs%C2%A0in%C2%A0Language%C2%A0Models/image-2.png)
1. プロンプト \(p\) で一度推論し、AttentionパターンとLayerNorm係数を**固定**。  
2. CLT出力との差分を**Error Node**としてバイアス項へ注入。  
3. 非線形性は JumpReLU と Error Node のみ、全体を全結合線形伝播モデルと見なす。  

この線形化モデルから、**Attribution Graph** を構築し、ノード間の寄与  
\[
  A_{s\to t} = a_s\,w_{s\to t}
\]  
を定量化します。 citeturn1view0

> **従来Transformerとの相違点**  
> - MLP層をCLTへ置換し、層横断的feature結合を獲得。  
> - Attention/LayerNorm固定による線形化強化。  
> - Error Nodeにより非線形影響を局所化。

## 3. Attribution Graphの要点

- **ノード種類**: 入力埋め込み、CLT feature、Error Node、出力ロジット  
- **エッジ重み**:  
  \[
    A_{s\to t} = a_s\,w_{s\to t}
  \]  
  ここで \(w_{s\to t}\) は固定JacobianとEncoder/Decoder行列の組み合わせで計算。  

この局所解析により、特定プロンプト上でどの feature がどのように出力に寄与したかを可視化します。 citeturn1view0

## 4. Global Weights 理論

局所解析だけでは捉えきれない**モデル全体の共通構造**を抽出するため、全プロンプトに共通な仮想重みを定義します。以下、理論的に深掘りします。

### 4.1 パス分類

特徴 \(s\) から \(t\) への影響経路は、大きく三つに分類されます：  
- **Residual‑direct**: Decoder→Residualストリーム→Encoder 経路  
- **Attention‑direct**: Decoder→Residual→OV注意→…→Encoder 経路  
- **Indirect**: 複数 feature ステップを経由する多段階経路  citeturn4view0  

このうち、**Residual‑direct** 経路はコンテキスト非依存な仮想重み（Virtual Weight）として扱うことが可能です。

### 4.2 Virtual Weight の定義

特徴 \(s\) が属する層を \(\ell_s\)、\(t\) を \(\ell_t\) とし、間の層集合を  
\[
  L_{st} = \{\ell \mid \ell_s \le \ell < \ell_t\}
\]  
とすると、Residual‑direct の仮想重みは以下で与えられます：  
\[
  V_{st}
  = \Bigl\langle
    \sum_{\ell\in L_{st}} W_{\mathrm{dec}}^{s,\ell},\,
    W_{\mathrm{enc}}^t
    \Bigr\rangle
\]  
Attribution Graph上では、この \(V_{st}\) に feature の活性化 \(a_s\) を掛けた寄与量 \(a_s\,V_{st}\) として扱います。 citeturn4view0

### 4.3 干渉（Interference）の課題

仮想重みは「コンテキスト非依存」である反面、**共起しない特徴ペア間にも大きな値**を取り得るため、モデル挙動に無関係な干渉経路が大量に生成されます。  
- 多様な概念を少数のニューロンで表現するため、decoder→encoder の射影行列に多くの非ゼロ要素が存在  
- 実際には同時活性化しないペアにも大きな \(V_{st}\) が生じ、ノイズ源となる  citeturn1view0

### 4.4 ERA と TWERA による干渉除去

干渉を抑制し、真に意味ある大域経路を抽出するために、**特徴共起統計**を用いて仮想重みに重み付けを行います。

1. **Expected Residual Attribution (ERA)**  
   \[
     V_{ij}^{\mathrm{ERA}}
     = \mathbb{E}\bigl[\mathbf{1}(a_j>0)\,a_i\bigr]\;V_{ij}
   \]
   特徴 \(j\) が活性化した際の平均 residual‑direct 寄与を捉えます。 citeturn1view0

2. **Target‑Weighted ERA (TWERA)**  
   \[
     V_{ij}^{\mathrm{TWERA}}
     = \frac{\mathbb{E}[a_i\,a_j]}{\mathbb{E}[a_j]}\;V_{ij}
   \]
   強い共起パターンに注目しつつ、小規模活性化への過大評価を抑制します。 citeturn1view0

これらにより、ノイズ経路を減衰し、大域回路の可視性を大幅に向上させます。

### 4.5 Attention‑direct や Indirect への拡張

- **Attention‑direct** 経路は、OV 注意行列を介した仮想重み  
  \[
    V_{st}^{\mathrm{att}}
    = \Bigl\langle
      \sum_{\ell\in L_{st}} W_{\mathrm{dec}}^{s,\ell}
      \;\prod_{\text{heads}} OV
      ,\,
      W_{\mathrm{enc}}^t
      \Bigr\rangle
  \]
  OV はプロンプト依存のスケーリングを伴い均一な定値化が難しいため、今後の課題です。 citeturn1view0  

- **Indirect** 経路は中間featureを媒介とする多段階線形結合として定義できますが、経路数が爆発的に増大するため、Pruning や Supernode 集約が必須です。

Residual‑direct＋ERA/TWERA だけでも、加算タスクの回路構造再現に成功しており、完全版の大域回路抽出は今後の研究課題です。 citeturn1view0turn4view0

## 5. 実験例：二桁加算

- **設定**: “a+b=” (\(a,b\in[0,99]\)) を10,000件生成し、2,931 feature 抽出  
- **結果**: Virtual Weights行列を解析し、“_6+_9→sum=_15”という加算ルール結合を確認。Lookupから加算への多段階ヒューリスティックが再現されました。

## 6. 結論と今後の課題

**結論**: CLTによるMLP置換と線形化手法により、局所寄与 (Attribution Graph) と大域回路 (Global Weights) の両立的可視化が可能になりました。

**今後の課題**:
- Attention‑direct／Indirect 経路の定量的評価手法開発  
- 文生成や QA タスクへの適用と一般化検証  
- 大規模モデル全体へのスケール適用性評価

---