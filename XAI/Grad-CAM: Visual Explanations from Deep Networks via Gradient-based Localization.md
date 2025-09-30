# Grad-CAM: Visual Explanations from Deep Networks via Gradient-based Localization

**著者**: Ramprasaath R. Selvaraju, Michael Cogswell, Abhishek Das, Ramakrishna Vedantam, Devi Parikh, Dhruv Batra  
**公開**: arXiv 2016（IJCV 2019 拡張版）  
**DOI**: 10.48550/arXiv.1610.02391  
**リンク**: [arXiv:1610.02391](https://arxiv.org/abs/1610.02391)

本論文は、CNN系モデルの幅広いクラスに対して、クラス判別的な粗い局在ヒートマップを生成する **Grad-CAM**（Gradient-weighted Class Activation Mapping）を提案します。最終畳み込み層の特徴マップに対するクラススコアの勾配を用いて、重要な空間領域を強調します。アーキテクチャ変更や再学習は不要で、分類・キャプション・VQA・強化学習など多様な設定に適用可能です。

---

### 1. 新規性：この論文の貢献と既存研究との差分

- **CAMの一般化**: Global Average Pooling（GAP）と線形分類器に依存するCAMを、最終畳み込み層の勾配情報で一般化し、任意のCNNに適用可能に。
- **クラス判別的ローカライゼーション**: クラス依存の勾配重みで特徴マップを線形合成し、重要領域を特定。
- **高解像度化（Guided Grad-CAM）**: Guided Backpropagationと組み合わせて、クラス判別性と高周波詳細の両立を実現。
- **人間実験による検証**: 失敗解析・頑健性・バイアス同定・ユーザ信頼の改善を実証。

---

### 2. 理論/手法の核心：数式できっちり定式化

#### 記法
- 最終畳み込み層のチャネル別特徴マップを \(A^{k} \in \mathbb{R}^{H\times W}\)（\(k=1,\dots,K\)）。
- ターゲットクラス \(c\) のスコア（ロジット）を \(y^{c}(x)\) とする。
- 画素和の規格化定数 \(Z = H\times W\)。

#### 2.1 勾配重みとクラスマップ
勾配を空間平均したチャネル重み：
\[
\alpha_k^{c} \;=\; \frac{1}{Z} \sum_{i=1}^{H} \sum_{j=1}^{W} \frac{\partial y^{c}}{\partial A^{k}_{ij}}.
\]
クラス \(c\) に対するGrad-CAMマップ（粗い局在マップ）：
\[
L^{c}_{\text{Grad-CAM}} \;=\; \operatorname{ReLU}\Bigl( \sum_{k=1}^{K} \alpha_k^{c} \, A^{k} \Bigr).
\]
- ReLUは、クラスに正の影響を与える部位を強調（負貢献を抑制）。
- 実装上、\(L^{c}_{\text{Grad-CAM}}\) を入力画像解像度へ双一次補間でアップサンプルして可視化。

#### 2.2 Guided Grad-CAM（高解像度化）
Guided Backpropagationで得た高周波・エッジ情報の勾配マップ \(G\) と要素積：
\[
\text{Guided-Grad-CAM}^{c} \;=\; \text{Upsample}\bigl(L^{c}_{\text{Grad-CAM}}\bigr) \;\odot\; G.
\]
これにより、クラス判別性（Grad-CAM）と高分解能（Guided BP）の長所を統合。

---

### 3. 実装ノート（重要ポイント）
- **層の選択**: 最終畳み込み層を推奨（高次抽象＋空間分解能のバランス）。
- **ターゲット**: ロジット（前ソフトマックス）に対する勾配を計算。
- **正規化**: 可視化時に \(L^{c}_{\text{Grad-CAM}}\) をmin-max正規化しカラーマップ重畳。
- **多タスク/マルチモーダル**: 出力タスク（例: キャプションの単語ロジット、VQAの回答ロジット）に応じて \(y^{c}\) を選択すれば同様に適用可。

---

### 4. 「キモ」と重要性：本論文の核と影響
- **キモ**: 「クラス勾配の空間平均＝チャネル重み」で特徴マップを線形合成し、ReLUで正の寄与に限定するだけの簡潔な式で、広範なCNNにクラス判別的局在を実現。
- **重要性/影響**: CAMの制約を取り払い、多種タスクへ拡張。失敗モード解析・バイアス検出・対人評価で有用性を示し、以降のGrad-CAM++/Score-CAM等の系譜を牽引。

---

### 5. まとめ（要点）
- 重み：\(\alpha_k^{c} = \tfrac{1}{Z}\sum_{i,j} \partial y^{c}/\partial A^{k}_{ij}\)。
- マップ：\(L^{c}_{\text{Grad-CAM}} = \operatorname{ReLU}(\sum_k \alpha_k^{c} A^{k})\)。
- 高解像度化：Guided BPと要素積でGuided Grad-CAM。
- 再学習不要・モデル非依存（CNN最終畳み込み層の勾配が取れれば適用可能）。

参考: [arXiv:1610.02391](https://arxiv.org/abs/1610.02391)
