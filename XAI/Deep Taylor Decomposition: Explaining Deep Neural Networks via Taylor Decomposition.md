# Deep Taylor Decomposition: Explaining Deep Neural Networks via Taylor Decomposition

**著者**: Grégoire Montavon, Sebastian Bach, Alexander Binder, Wojciech Samek, Klaus-Robert Müller  
**公開**: Pattern Recognition 2018  
**DOI**: 10.1016/j.patcog.2018.01.038  
**リンク**: [Pattern Recognition](https://www.sciencedirect.com/science/article/pii/S0031320318300220)

本論文は、**Deep Taylor Decomposition (DTD)** を提案し、深層ニューラルネットワークの出力を入力特徴の寄与度に分解する理論的枠組みを提供します。テイラー展開を用いて各層の活性化関数を局所的に線形化し、関連性（relevance）を下位層へ伝播させることで、LRP（Layer-Wise Relevance Propagation）の理論的基盤を確立します。

---

### 1. 新規性：この論文の貢献

- **テイラー展開による理論的基盤**: ニューラルネットワークの各層でテイラー展開を適用し、関連性伝播の理論的正当性を提供
- **LRPとの統一的枠組み**: LRPの伝播ルールをDeep Taylor Decompositionから導出
- **参照点の選択**: 各層で適切な参照点（root point）を選択することで、安定した関連性伝播を実現
- **層別の伝播ルール**: 全結合層、畳み込み層、プーリング層など、各層タイプに応じた伝播ルールを導出

---

### 2. 理論/手法の核心：数式できっちり定式化

#### 記法
- 入力特徴ベクトル \(x = (x_1, \ldots, x_d) \in \mathbb{R}^d\)
- ニューラルネットワークの出力 \(f(x) \in \mathbb{R}\)
- 層 \(l\) のニューロン \(j\) の活性化 \(f_j^{(l)}(x)\)
- ニューロン \(j\) の関連性スコア \(R_j\)
- 参照点（root point）\(\tilde{x}\)

#### 2.1 基本的なアイデア

**Deep Taylor Decomposition**の目標は、出力 \(f(x)\) を入力特徴の寄与度 \(R_i\) の和として分解すること：
\[
f(x) = \sum_{i=1}^{d} R_i + \Delta
\]

ここで、\(\Delta\) は分解誤差（通常は小さく無視できる）。

#### 2.2 テイラー展開による線形化

ニューロン \(j\) の関数 \(f_j(x)\) を参照点 \(\tilde{x}\) で一次テイラー展開：
\[
f_j(x) = f_j(\tilde{x}) + \sum_{i=1}^{d} \frac{\partial f_j}{\partial x_i}\Big|_{\tilde{x}} (x_i - \tilde{x}_i) + \mathcal{O}(|x-\tilde{x}|^2)
\]

ヤコビアン \(J_{ji}(\tilde{x}) = \frac{\partial f_j}{\partial x_i}\Big|_{\tilde{x}}\) を用いて：
\[
f_j(x) = \sum_{i=1}^{d} J_{ji}(\tilde{x}) x_i + \underbrace{f_j(\tilde{x}) - \sum_{i=1}^{d} J_{ji}(\tilde{x}) \tilde{x}_i}_{b_j} + \mathcal{O}(|x-\tilde{x}|^2)
\]

#### 2.3 関連性の伝播ルール

上位層のニューロン \(j\) の関連性 \(R_j\) を、下位層のニューロン \(i\) に分配：
\[
R_{i \leftarrow j} = \frac{J_{ji}(\tilde{x}) x_i}{f_j(x) + \epsilon \cdot \text{sign}(f_j(x))} R_j
\]

下位層のニューロン \(i\) の関連性は、全ての上位層ニューロンからの寄与の和：
\[
R_i = \sum_{j} R_{i \leftarrow j} = \sum_{j} \frac{J_{ji}(\tilde{x}) x_i}{f_j(x) + \epsilon \cdot \text{sign}(f_j(x))} R_j
\]

ここで、\(\epsilon > 0\) は数値安定化のための小さな値。

#### 2.4 参照点（Root Point）の選択

参照点 \(\tilde{x}\) の選択が重要。一般的な選択方法：

**1. ゼロ参照点**:
\[
\tilde{x} = 0
\]

**2. 最適参照点**（ニューロンが非活性化される点）:
\[
\tilde{x} = \arg\min_{\tilde{x}} |f_j(\tilde{x})| \quad \text{s.t. } f_j(\tilde{x}) \approx 0
\]

**3. ワーストケース参照点**（予測が反転する点）:
\[
\tilde{x} = \arg\min_{\tilde{x}} f_j(\tilde{x}) \quad \text{s.t. } \text{sign}(f_j(\tilde{x})) \neq \text{sign}(f_j(x))
\]

#### 2.5 層別の伝播ルール

**全結合層**:
\[
R_i = \sum_{j} \frac{w_{ij} x_i}{\sum_{k} w_{kj} x_k + \epsilon \cdot \text{sign}(\sum_{k} w_{kj} x_k)} R_j
\]

**畳み込み層**:
各空間位置での関連性を畳み込みカーネルに基づいて分配。

**ReLU層**:
\[
R_i = \begin{cases}
R_j & \text{if } x_i > 0 \\
0 & \text{if } x_i \leq 0
\end{cases}
\]

---

### 3. LRPとの関係

**Deep Taylor DecompositionとLRP**:
- **DTD**: テイラー展開による理論的基盤を提供
- **LRP**: DTDから導出される実用的な伝播ルール
- **関係**: LRPの伝播ルールは、DTDの特定の参照点選択に対応

**具体的な対応**:
- LRPの \(\epsilon\)-ルールは、DTDのゼロ参照点での展開に対応
- LRPの \(\alpha, \beta\)-ルールは、DTDの正負の寄与を分離した展開に対応

---

### 4. XAIへの応用

#### 4.1 画像分類

**問題**: 画像のどの部分が予測に寄与したか

**DTDの適用**:
- 各ピクセルの関連性スコア \(R_i\) を計算
- ヒートマップとして可視化
- モデルの判断根拠を理解

#### 4.2 テキスト分類

**問題**: テキストのどの単語が予測に寄与したか

**DTDの適用**:
- 各単語の関連性スコアを計算
- 重要単語を強調表示
- モデルの判断根拠を理解

#### 4.3 モデルデバッグ

**問題**: モデルが正しい特徴を見ているか確認

**DTDの適用**:
- 関連性スコアから、モデルが注目している領域を特定
- 不適切な特徴への依存を検出
- モデルの改善に活用

---

### 5. 利点と欠点

**利点**:
- **理論的基盤**: テイラー展開による数学的に厳密な枠組み
- **完全性**: 関連性保存則により、出力が入力に完全に分解される
- **柔軟性**: 参照点の選択により、様々な説明を生成可能
- **計算効率**: 単一の逆伝播で計算可能

**欠点**:
- **参照点の選択**: 適切な参照点の選択が難しい場合がある
- **線形近似**: テイラー展開の一次項のみを使用するため、非線形性が強い場合に誤差が大きい
- **局所性**: 参照点の近傍でのみ有効な近似

---

### 6. 実装上の注意点

- **参照点の選択**: 問題に応じて適切な参照点を選択
- **数値安定性**: \(\epsilon\) の値を適切に設定
- **層別のルール**: 各層タイプに応じた伝播ルールを適用
- **正規化**: 関連性スコアのスケールを調整

---

### 7. 批判的検討

近年の研究（Sixt & Landgraf, 2022）では、以下の点が指摘されています：
- DTDが入力×勾配法と同等である可能性
- 選択されたテイラー展開の基点が局所的に一定の場合、説明がモデルの深い層に依存しない可能性
- 参照点の選択が説明の品質に大きく影響する

---

### 8. 「キモ」と重要性

- **キモ**: テイラー展開を用いてニューラルネットワークの各層を局所的に線形化し、関連性伝播の理論的基盤を提供。これにより、LRPなどの実用的な手法の理論的正当性が確立された。
- **重要性**: 
  - LRPの理論的基盤として機能
  - XAI分野における理論的研究の基礎
  - 関連性伝播手法の統一的な理解を提供

---

### 9. まとめ（要点）

- **基本式**: \(f_j(x) = f_j(\tilde{x}) + \sum_i J_{ji}(\tilde{x})(x_i - \tilde{x}_i) + \mathcal{O}(|x-\tilde{x}|^2)\)
- **伝播ルール**: \(R_i = \sum_j \frac{J_{ji}(\tilde{x}) x_i}{f_j(x) + \epsilon \cdot \text{sign}(f_j(x))} R_j\)
- **参照点**: ゼロ参照点、最適参照点、ワーストケース参照点など
- **LRPとの関係**: LRPの伝播ルールはDTDから導出される

参考: [Pattern Recognition 2018](https://www.sciencedirect.com/science/article/pii/S0031320318300220)
