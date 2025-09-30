# Understanding Black-box Predictions via Influence Functions

**著者**: Pang Wei Koh, Percy Liang  
**公開**: arXiv 2017（ICML 2017 採択）  
**DOI**: 10.48550/arXiv.1703.04730  
**リンク**: [arXiv:1703.04730](https://arxiv.org/abs/1703.04730)

この論文は、ロバスト統計で古典的な**影響関数 (Influence Functions)** を現代の機械学習に適用し、ブラックボックスモデルのある予測を「どの訓練データがどれだけ効いたか」に遡って定量化する枠組みを提示します。勾配とヘッセ行列-ベクトル積（HVP）へのアクセスだけでスケーラブルに実装し、モデルのデバッグ、データセット誤り検出、データ汚染の発見等に有用であることを示します。

---

### 1. 新規性：この論文の貢献と既存研究との差分

- **訓練点の寄与を厳密に定量化**: 予測（損失）の変化を、一点の重み付け（微小変更）に対する感度として解析解で与える。
- **スケーラブルな実装**: ヘッセ逆行列の明示計算なしに、HVPと反復法（LiSSA/CG）で \(H^{-1}v\) を近似。
- **非凸モデルでも有益**: 理論仮定が破れる状況（非凸・非滑らか）でも、実務上有用な近似情報を提供。
- **用途の多様性**: モデル挙動の理解、誤ラベル検出、トレーニングセット攻撃の検知等を実証。

---

### 2. 理論/手法の核心：数式できっちり定式化

#### 2.1 セットアップ
- 訓練分布からのサンプル \(z_i=(x_i,y_i)\), \(i=1,\dots,n\)。損失 \(L(z,\theta)\)。経験リスク
\[
R(\theta) = \frac{1}{n}\sum_{i=1}^n L(z_i,\theta)\quad (\text{必要に応じてダンピング }\frac{\lambda}{2}\lVert\theta\rVert^2 \text{を付与})
\]
- 推定量 \(\hat{\theta} = \arg\min_\theta R(\theta)\)。ヘッセ行列 \(H_{\hat{\theta}} = \nabla^2_{\theta} R(\hat{\theta})\)。

#### 2.2 訓練点の微小重み付け（上方影響）
訓練点 \(z\) を \(\varepsilon\) だけ上方重み付け：
\[
\hat{\theta}_{\varepsilon,z} = \arg\min_\theta \bigl\{ R(\theta) + \varepsilon\, L(z,\theta) \bigr\}.
\]
暗黙関数定理より、\(\varepsilon=0\) 周りの一次感度：
\[
\frac{\mathrm{d}\hat{\theta}_{\varepsilon,z}}{\mathrm{d}\varepsilon}\Big|_{\varepsilon=0} \;=\; -\, H_{\hat{\theta}}^{-1} \, \nabla_{\theta} L\bigl(z,\hat{\theta}\bigr).
\]

#### 2.3 テスト損失への影響（上方影響 on loss）
テスト点 \(z_{\text{test}}\) の損失変化：
\[
\mathcal{I}_{\text{up, loss}}(z, z_{\text{test}})
= \frac{\mathrm{d}}{\mathrm{d}\varepsilon} L\bigl(z_{\text{test}},\hat{\theta}_{\varepsilon,z}\bigr)\Big|_{\varepsilon=0}
= -\, \nabla_{\theta} L\bigl(z_{\text{test}},\hat{\theta}\bigr)^{\top} 
\, H_{\hat{\theta}}^{-1} 
\, \nabla_{\theta} L\bigl(z,\hat{\theta}\bigr).
\]
この値が大きい（正）ほど、\(z\) は \(z_{\text{test}}\) の損失を増やす方向に影響、負なら減らす方向に影響。

#### 2.4 訓練点の削除近似（上方影響の再スケーリング）
\(z\) を削除した推定量 \(\hat{\theta}_{-z}\) による損失変化は、\(\varepsilon=-\tfrac{1}{n}\) とみなす一次近似で：
\[
L\bigl(z_{\text{test}}, \hat{\theta}_{-z}\bigr) 
\;\approx\; L\bigl(z_{\text{test}}, \hat{\theta}\bigr)
\; -\; \tfrac{1}{n} \, \mathcal{I}_{\text{up, loss}}(z, z_{\text{test}}).
\]

#### 2.5 学習データ摂動の影響（入力・ラベル）
入力を \(x \mapsto x + \delta\) と無限小摂動したときのテスト損失影響：
\[
\mathcal{I}_{\text{pert, loss}}(z, z_{\text{test}}; \delta)
= -\, \nabla_{\theta} L\bigl(z_{\text{test}},\hat{\theta}\bigr)^{\top}
\, H_{\hat{\theta}}^{-1} 
\, \nabla_{\theta} \nabla_{x} L\bigl(z,\hat{\theta}\bigr)\, \delta.
\]
ラベル摂動も同様に \(\nabla_{\theta} \nabla_{y} L\) を用いて定式化可能。

---

### 3. 計算と実装上の要点
- **HVP（ヘッセ×ベクトル）**: Pearlmutterのトリックで自動微分により \(v\mapsto H v\) を計算。
- **逆ヘッセ作用素の近似**: 反復法で \(u \approx H^{-1} v\)。具体的には、
  - LiSSA（確率的反復近似）や共役勾配法（CG）で収束良く計算。
  - **ダンピング**: \(\tilde{H} = H + \lambda I\) として正定値化し安定化。
- **スケーリング**: 全訓練点に対する影響はベクトル化して効率的に算出。テスト点ごとに \(u = H^{-1} \nabla_{\theta} L(z_{\text{test}},\hat{\theta})\) を一度求め、各 \(z\) について \(-u^{\top} \nabla_{\theta} L(z,\hat{\theta})\) を評価。

---

### 4. 「キモ」と重要性：本論文の核と影響
- **キモ**: 予測の説明を「影響の大きい訓練事例の同定」という形で提供し、例ベースの説明を理論的に裏付けた点。解析解に基づくため、単なる相関ではなく最適化問題における因果的感度を捉える。
- **重要性/影響**: 誤ラベル検出・データ汚染検知・モデルデバッグを系統化。CNNを含む非凸モデルでも、近似が現実的に有用であることを示し、実務での採用を後押し。

---

### 5. まとめ（要点）
- 上方影響：\(\tfrac{\mathrm{d}\hat{\theta}_{\varepsilon,z}}{\mathrm{d}\varepsilon}|_{0} = - H^{-1} \nabla_{\theta} L(z,\hat{\theta})\)。
- テスト損失影響：\(\mathcal{I}_{\text{up, loss}}(z, z_{\text{test}}) = - \nabla_{\theta} L(z_{\text{test}})^{\top} H^{-1} \nabla_{\theta} L(z)\)。
- 削除近似：\(\Delta L \approx -\tfrac{1}{n}\, \mathcal{I}_{\text{up, loss}}\)。
- 実装：HVP + 反復近似（LiSSA/CG）、ダンピングで安定化。

参考: [arXiv:1703.04730](https://arxiv.org/abs/1703.04730)
