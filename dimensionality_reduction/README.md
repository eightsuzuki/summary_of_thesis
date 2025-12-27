# Dimensionality Reduction

This directory collects summaries and notes on dimensionality reduction methods (e.g., PCA, t-SNE, UMAP, ICA, Isomap).

- Add method-specific markdown files here, following the existing summary style in this repository.

## Methods and seminal references

- PCA — Pearson (1901); Hotelling (1933)
  - Pearson: On lines and planes of closest fit to systems of points in space. Philosophical Magazine. [DOI](https://doi.org/10.1080/14786440109462720)
  - Hotelling: Analysis of a complex of statistical variables into principal components. Journal of Educational Psychology. [DOI (Part I)](https://doi.org/10.1037/h0071325), [DOI (Part II)](https://doi.org/10.1037/h0070888)
- ICA — Comon (1994); Hyvärinen (1999)
  - Comon: Independent component analysis, a new concept? Signal Processing. [DOI](https://doi.org/10.1016/0165-1684(94)90029-9)
  - Hyvärinen: Fast and robust fixed-point algorithms for independent component analysis. IEEE TNN. [DOI](https://doi.org/10.1109/72.761722)
- t-SNE — van der Maaten & Hinton (2008)
  - Visualizing Data using t-SNE. JMLR. [Link](http://www.jmlr.org/papers/v9/vandermaaten08a.html)
- UMAP — McInnes, Healy, Melville (2018)
  - UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction. arXiv:1802.03426. [arXiv](https://arxiv.org/abs/1802.03426)
- Isomap — Tenenbaum, de Silva, Langford (2000)
  - A Global Geometric Framework for Nonlinear Dimensionality Reduction. Science. [DOI](https://doi.org/10.1126/science.290.5500.2319)
- LLE — Roweis & Saul (2000)
  - Nonlinear Dimensionality Reduction by Locally Linear Embedding. Science. [DOI](https://doi.org/10.1126/science.290.5500.2323)
- Hessian LLE — Donoho & Grimes (2003)
  - Hessian Eigenmaps: Locally Linear Embedding Techniques for High-Dimensional Data. PNAS. [DOI](https://doi.org/10.1073/pnas.0930874100)
- Laplacian Eigenmaps — Belkin & Niyogi (2003)
  - Laplacian Eigenmaps for Dimensionality Reduction and Data Representation. Neural Computation. [DOI](https://doi.org/10.1162/089976603321780317)
- Diffusion Maps — Coifman & Lafon (2006)
  - Diffusion Maps. PNAS. [DOI](https://doi.org/10.1073/pnas.0500334103)
- Kernel PCA — Schölkopf, Smola, Müller (1998)
  - Nonlinear Component Analysis as a Kernel Eigenvalue Problem. Neural Computation. [DOI](https://doi.org/10.1162/089976698300017467)
- MDS — Kruskal (1964)
  - Nonmetric multidimensional scaling: a numerical method. Psychometrika. [DOI](https://doi.org/10.1007/BF02289694)
- NMF — Lee & Seung (1999)
  - Learning the parts of objects by non-negative matrix factorization. Nature. [DOI](https://doi.org/10.1038/44565)/
- Autoencoders (nonlinear DR) — Hinton & Salakhutdinov (2006)
  - Reducing the Dimensionality of Data with Neural Networks. Science. [DOI](https://doi.org/10.1126/science.1127647)
- PHATE — Moon, van Dijk, Wang et al. (2019)
  - Visualizing structure and transitions in high-dimensional biological data. Nature Biotechnology. [DOI](https://doi.org/10.1038/s41587-019-0336-3)

## Quick comparison table

| Method | Linear/Nonlinear | Class | Preserves | Strengths | Limitations | Year |
| --- | --- | --- | --- | --- | --- | --- |
| PCA | Linear | Spectral (SVD) | Global variance | Fast, interpretable, whitening | Only linear structure | 1901/1933 |
| ICA | Linear mixing | Probabilistic/signal | Statistical independence | Blind source separation | Scale/permutation ambiguity; not manifold | 1994/1999 |
| MDS (classical) | Linear on distances | Spectral (double-centering) | Pairwise distances | Simple geometry from distances | Sensitive to distortions/outliers | 1964 |
| Kernel PCA | Nonlinear | Kernel eigen | Global variance in feature space | Captures nonlinear structure | Kernel choice; pre-image problem | 1998 |
| NMF | Nonlinear (constraints) | Matrix factorization | Parts-based structure | Additive, sparse parts | Nonconvex; scaling | 1999 |
| Isomap | Nonlinear | Graph + MDS | Global geodesics | Global manifold unfolding | Short-circuits; connectivity | 2000 |
| LLE | Nonlinear | Graph weights + eig | Local linearity | Simple, local preservation | Sensitive to K; curvature issues | 2000 |
| Hessian LLE | Nonlinear | Hessian-based | Local isometry (curvature) | Better on curved manifolds | Parameter sensitive | 2003 |
| Laplacian Eigenmaps | Nonlinear | Spectral graph | Local neighborhoods | Spectral foundation; clustering link | Graph params critical | 2003 |
| Diffusion Maps | Nonlinear | Markov diffusion | Diffusion distance (multi-scale) | Noise-robust; scale control | Kernel/epsilon choice | 2006 |
| Autoencoders | Nonlinear | Neural | Task-driven reconstruction | Flexible, scalable | Training/regularization sensitive | 2006 |
| t-SNE | Nonlinear | Probabilistic neighbor | Local neighborhoods (visual) | Cluster visualization | Poor global geometry | 2008 |
| UMAP | Nonlinear | Fuzzy simplicial set | Local+some global | Fast, preserves topology | Params tuning; theory assumptions | 2018 |
| PHATE | Nonlinear | Diffusion + potential | Transitions/trajectories | Branching/continuum structure | Parameter t/k choice | 2019 |

## Chronological development and influences

- 1901/1933: PCA (Pearson; Hotelling). Foundational linear variance maximization; basis for whitening and initialization across methods.
- 1964: MDS (Kruskal). Distance-preserving embeddings; classical MDS later used inside Isomap.
- 1994/1999: ICA (Comon; Hyvärinen). Independent components for source separation; complementary to PCA (second-order vs higher-order statistics).
- 1998: Kernel PCA (Schölkopf et al.). Introduces kernel trick to PCA; influences many kernel spectral methods.
- 1999: NMF (Lee & Seung). Parts-based factorization; later tied to sparse coding and topic models.
- 2000: Isomap (Tenenbaum et al.) builds on MDS with geodesic distances; LLE (Roweis & Saul) uses local linear reconstructions.
- 2003: Laplacian Eigenmaps (Belkin & Niyogi) formalize spectral graph embeddings; Hessian LLE (Donoho & Grimes) refines LLE by penalizing curvature.
- 2006: Diffusion Maps (Coifman & Lafon) introduce diffusion distances (Markov processes); Autoencoders (Hinton & Salakhutdinov) revive neural DR with nonlinear bottlenecks.
- 2008: t-SNE (van der Maaten & Hinton) improves SNE with Student-t in low-dim, focusing on neighborhood visualization.
- 2018: UMAP (McInnes et al.) connects Riemannian geometry and fuzzy simplicial sets; conceptually relates to spectral/diffusion methods and aims to improve t-SNE trade-offs.
- 2019: PHATE (Moon et al.) builds on diffusion processes with information-geometric potentials to capture both local and transitional structures.

Notes on influences:
- Spectral lineage: PCA → Kernel PCA; MDS → Isomap; Graph Laplacians → Laplacian Eigenmaps → Diffusion Maps (normalization variants).
- Local linear lineage: LLE → Hessian LLE; both inform manifold learning emphasizing locality.
- Probabilistic neighbor lineage: SNE/t-SNE → inspired UMAP’s focus on neighborhood preservation (with different mathematical framing).
- Diffusion lineage: Diffusion Maps → PHATE (via diffusion potentials), related in spirit to UMAP’s probabilistic graphs.

## 各手法の詳細解説：何をしているか、得意なこと、苦手なこと

### PCA（Principal Component Analysis）

**何をしているか**:
- データの分散が最大になる方向（主成分）を見つけて、その方向に射影する
- 共分散行列の固有値分解（またはSVD）により、データを低次元空間に線形変換
- データの「大域的な広がり」を保持

**得意なこと**:
- ✅ **高速**: 線形変換のみで計算が速い（$O(n d^2)$）
- ✅ **解釈可能**: 主成分の意味が明確（分散の寄与率が分かる）
- ✅ **新要素追加が容易**: 変換行列を保存しておけば、新しいデータポイントを即座に変換可能
- ✅ **ノイズ除去**: 小さな主成分を捨てることでノイズを除去
- ✅ **前処理**: 他の手法の前処理としても使える（whitening）

**苦手なこと**:
- ❌ **非線形構造**: 非線形な多様体（manifold）を捉えられない
- ❌ **局所構造**: 局所的なクラスタや細かい構造が潰れやすい
- ❌ **可視化**: 2D/3D可視化では非線形な特徴を捉えにくい

---

### ICA（Independent Component Analysis）

**何をしているか**:
- 観測データが独立な成分（ソース）の線形混合だと仮定し、その独立成分を分離
- 非ガウス性を最大化することで、独立な成分を抽出
- 信号の「独立性」を利用

**得意なこと**:
- ✅ **ブラインドソース分離**: 音声や画像の混合信号から元の信号を分離
- ✅ **特徴抽出**: 統計的に独立な特徴を抽出
- ✅ **ノイズ除去**: 独立成分のうち、ノイズ成分を除去可能

**苦手なこと**:
- ❌ **多様体学習**: 多様体構造を捉えるのには不向き
- ❌ **順序・スケールの曖昧性**: 成分の順序とスケールが一意に決まらない
- ❌ **ガウス分布**: ガウス分布の混合では分離できない

---

### MDS（Multidimensional Scaling）

**何をしているか**:
- データポイント間の距離（または非類似度）が与えられたとき、その距離関係を保持するように低次元空間に配置
- 距離行列から直接埋め込みを計算（double-centering + 固有値分解）

**得意なこと**:
- ✅ **距離の保存**: 距離情報のみから埋め込みを構築
- ✅ **非類似度データ**: 距離以外の非類似度（順序情報）も扱える（非計量MDS）
- ✅ **シンプル**: 数学的に分かりやすい

**苦手なこと**:
- ❌ **ノイズに弱い**: 距離の歪みや外れ値に敏感
- ❌ **構造の区別**: クラスタや軌道などの特徴的構造とノイズを区別しない
- ❌ **大規模データ**: 全対全の距離計算が必要で、計算コストが高い

---

### Kernel PCA

**何をしているか**:
- PCAを非線形に拡張。カーネル関数で高次元特徴空間に写像し、その空間でPCAを実行
- カーネルトリックにより、明示的な写像なしで非線形変換を実現

**得意なこと**:
- ✅ **非線形構造**: 非線形な多様体を捉えられる
- ✅ **柔軟性**: カーネル関数の選択により様々な構造に対応
- ✅ **理論的基盤**: PCAの理論を非線形に拡張

**苦手なこと**:
- ❌ **カーネル選択**: 適切なカーネル関数の選択が重要（RBF、多項式など）
- ❌ **Pre-image問題**: 低次元表現から元のデータ空間への逆写像が困難
- ❌ **計算コスト**: カーネル行列の計算と固有値分解が必要（$O(n^2)$）

---

### NMF（Non-negative Matrix Factorization）

**何をしているか**:
- 非負のデータ行列を、非負の基底行列と係数行列の積に分解
- 加法的な「部品」表現を学習（例：画像の部分、トピックの単語）

**得意なこと**:
- ✅ **解釈可能**: 基底が「部品」として解釈できる
- ✅ **スパース性**: スパースな表現を自然に学習
- ✅ **トピックモデル**: 文書のトピック抽出に適している
- ✅ **画像解析**: 画像の部分的な特徴を抽出

**苦手なこと**:
- ❌ **非凸最適化**: 局所最適解に陥りやすい（複数の初期値で試す必要）
- ❌ **スケーリング**: データのスケールに敏感
- ❌ **非負制約**: 負の値を持つデータには適用できない

---

### Isomap

**何をしているか**:
- データポイント間の測地距離（多様体上の最短経路）を近似し、その距離関係を保持するように埋め込み
- kNNグラフ上で最短経路を計算し、MDSで埋め込み

**得意なこと**:
- ✅ **大域的な多様体**: 非線形な多様体の大域的な構造を保持
- ✅ **測地距離**: 多様体上の「真の距離」を近似

**苦手なこと**:
- ❌ **短絡エッジ**: グラフの短絡エッジ（short-circuit）に敏感
- ❌ **連結性**: グラフが非連結だと失敗
- ❌ **計算コスト**: 全対全の最短経路計算が必要（$O(n^2 \log n)$）

---

### LLE（Locally Linear Embedding）

**何をしているか**:
- 各データポイントを近傍の線形結合で再構成する重みを計算し、その重み関係を低次元空間で保持
- 局所的な線形性を仮定し、それを多様体全体に拡張

**得意なこと**:
- ✅ **局所構造の保持**: 近傍の関係を正確に保持
- ✅ **シンプル**: 数学的に分かりやすい
- ✅ **計算効率**: 近傍グラフの構築と固有値分解のみ

**苦手なこと**:
- ❌ **パラメータKに敏感**: 近傍数Kの選択が重要
- ❌ **曲率の問題**: 曲がった多様体では局所線形性が成り立たない
- ❌ **外れ値**: 外れ値に敏感

---

### Hessian LLE

**何をしているか**:
- LLEを改良し、多様体の曲率（Hessian）を考慮して局所等距性を保持
- 曲がった多様体でも局所的な距離関係を保持

**得意なこと**:
- ✅ **曲がった多様体**: LLEより曲がった多様体に強い
- ✅ **局所等距性**: 局所的な距離関係をより正確に保持

**苦手なこと**:
- ❌ **パラメータに敏感**: 複数のパラメータの調整が必要
- ❌ **計算コスト**: Hessianの計算により、LLEより計算コストが高い

---

### Laplacian Eigenmaps

**何をしているか**:
- データポイント間の重み付きグラフを構築し、グラフラプラシアンの固有ベクトルで埋め込み
- 近傍の関係を保持するように低次元空間に配置

**得意なこと**:
- ✅ **スペクトラル理論**: グラフ理論に基づいた理論的基盤
- ✅ **クラスタリングとの関連**: スペクトラルクラスタリングと密接に関連
- ✅ **局所近傍の保持**: 近傍の関係を正確に保持

**苦手なこと**:
- ❌ **グラフパラメータ**: kNNや$\epsilon$の選択が重要
- ❌ **大域構造**: 大域的な構造の保持は限定的

---

### Diffusion Maps

**何をしているか**:
- データポイント間の遷移確率行列を構築し、拡散過程（ランダムウォーク）の固有ベクトルで埋め込み
- 拡散距離（diffusion distance）を保持するように低次元空間に配置

**得意なこと**:
- ✅ **ノイズに強い**: 拡散過程によりノイズに頑健
- ✅ **スケール調整**: パラメータ$t$により局所から大域までスケール調整可能
- ✅ **軌道構造**: 連続的な軌道（trajectory）構造を見つけやすい

**苦手なこと**:
- ❌ **次元の分散**: 異なる軌道を異なる次元に圧縮しがち
- ❌ **2D/3D可視化**: 可視化には不向きな場合がある
- ❌ **カーネル/$\epsilon$の選択**: パラメータの選択が重要

---

### Autoencoders

**何をしているか**:
- ニューラルネットワークで入力を低次元のボトルネック層を通して再構成
- 再構成誤差を最小化することで、低次元表現を学習

**得意なこと**:
- ✅ **柔軟性**: 様々な構造に対応可能（非線形、深層）
- ✅ **スケーラブル**: 大規模データにも適用可能
- ✅ **新要素追加が容易**: エンコーダーを適用するだけ
- ✅ **正則化**: Denoising、Sparse、Variationalなど様々な拡張が可能

**苦手なこと**:
- ❌ **学習の不安定性**: ハイパーパラメータや正則化の調整が必要
- ❌ **局所最適解**: 初期値に依存しやすい
- ❌ **解釈性**: 学習された表現の解釈が困難

---

### t-SNE

**何をしているか**:
- 高次元空間での近傍確率分布と低次元空間での確率分布のKLダイバージェンスを最小化
- Student-t分布を使用することで、低次元での「クラスタリング効果」を実現

**得意なこと**:
- ✅ **クラスタ可視化**: クラスタの可視化に非常に強い
- ✅ **局所構造**: 局所的な近傍関係を正確に保持
- ✅ **視覚的に分かりやすい**: クラスタが明確に分離される

**苦手なこと**:
- ❌ **大域構造**: 大域的な距離関係は保持されない
- ❌ **再現性**: 初期化により結果が変わりやすい
- ❌ **新要素追加が困難**: 全データを再計算する必要がある
- ❌ **パラメータ（perplexity）**: perplexityの選択が重要

---

### UMAP

**何をしているか**:
- 高次元でのファジー単体集合（fuzzy simplicial set）と低次元での確率分布の交差エントロピーを最小化
- リーマン幾何学とトポロジーに基づいた理論的基盤

**得意なこと**:
- ✅ **高速**: t-SNEより高速
- ✅ **大域+局所**: 局所構造だけでなく、ある程度の大域構造も保持
- ✅ **トポロジー**: データのトポロジー構造を保持
- ✅ **スケーラブル**: 大規模データにも適用可能

**苦手なこと**:
- ❌ **パラメータ調整**: n_neighbors、min_distの調整が必要
- ❌ **理論的仮定**: 一様分布の多様体という仮定が必ずしも成り立たない
- ❌ **新要素追加が困難**: 全データを再計算する必要がある

---

### PHATE

**何をしているか**:
- 拡散過程（Diffusion Maps）の情報幾何学的ポテンシャル（対数スケール）を計算し、遷移構造を保持
- 連続的な軌道や分岐構造を可視化

**得意なこと**:
- ✅ **軌道構造**: 連続的な軌道（trajectory）や分岐構造を見つけやすい
- ✅ **遷移の可視化**: 時間的な遷移や発達過程の可視化に適している
- ✅ **密度効果の除去**: 密度の影響を抑えて構造を捉える

**苦手なこと**:
- ❌ **パラメータ選択**: k（近傍数）、t（拡散時間）の選択が重要
- ❌ **計算コスト**: 拡散過程の計算が必要
- ❌ **特定用途**: 軌道構造がないデータには過剰な場合がある

---

## 比較メモ（日本語）

- PCA: 主成分分析。分散最大の軸を取り出す。他の手法にはない利点が多いが、可視化用途では非線形な特徴を捉えにくい。分布の大域的特徴をよく反映する一方、局所的構造がノイズとしてつぶれがち。
- t-SNE / UMAP: 各点の局所近傍との距離関係が保存されるように低次元配置を最適化。局所構造やクラスタの可視化に強い。連続的な「軌道」は分割されやすく、長距離の関係性はほぼ見ないため、クラスタ間の相対距離は乱数などで変わりやすい（再現性に注意）。
- MDS: 多次元尺度構成法。全対全の距離関係を保存するように配置。データの「構造」を直接は見ないので、ノイズに弱く、クラスタや軌道などの特徴的構造とノイズを区別しない。
- Diffusion Maps: 距離から遷移確率行列を構成し、グラフ上のランダムウォーク（拡散）を用いる。t ステップ後の分布（P^t）の違いをユークリッド距離として反映する座標を、P^t の固有ベクトルで得る。ノイズに強く、t により局所から大域までスケール調整可能。「軌道」構造を見つけやすい利点がある一方、異なる軌道を異なる次元に圧縮しがちで、2D/3D 可視化には不向きな場合がある。
- Trajectory inference（Monocle2 など）: 木構造を前提に軌道を推定する手法。前提が適切か事前にわかりにくく、推定が不安定になり得る（初期化・ハイパラ・前処理の影響が大きい）。
