# Anchors: High-Precision Model-Agnostic Explanations

**著者**: Marco Tulio Ribeiro, Sameer Singh, Carlos Guestrin  
**公開**: AAAI 2018  
**DOI**: 10.1609/aaai.v32i1.11491  
**リンク**: [AAAI proceedings page](https://ojs.aaai.org/index.php/AAAI/article/view/11491)

本論文は、ブラックボックス分類器に対して、特定入力 \(x\) 近傍での**高精度**な局所規則（If-Then ルール）による説明 **Anchors** を提案します。Anchor は「その条件が満たされる限りモデルの予測がほぼ変わらない」という局所十分条件を提示します。

---

### 1. 新規性：この論文の貢献
- **高精度保証つき局所規則**: 規則の精度（precision）に確率的保証を付与。
- **モデル非依存**: 任意のブラックボックスに適用（予測クエリとサンプリングが可能なら可）。
- **効率的探索**: ルール探索に多腕バンディット/ビームサーチと統計的停止基準を用いる。

---

### 2. 理論/手法の核心：数式できっちり定式化

#### 記法
- ブラックボックス予測器 \(f(x)\) と入力 \(x\)。ターゲット予測 \(y = f(x)\)。
- 述語（条件）集合 \(\mathcal{P}=\{p_1, p_2, ...\}\)。Anchor は有限個の述語からなる集合 \(A \subset \mathcal{P}\)。
- 局所摂動分布 \(\mathcal{D}(\cdot\mid A)\): Anchor 条件 \(A\) を満たすサンプルを生成する分布（特徴ごとに固定/ランダム化など）。

#### Precision（精度）と Coverage（被覆）
- Anchor の精度（条件が成り立つとき同じ予測になる確率）
\[
\text{Prec}(A) \,=\, \Pr_{z\sim \mathcal{D}(\cdot\mid A)}\big[ f(z)=y \big].
\]
- Anchor の被覆（条件がどの程度現れやすいか）
\[
\text{Cov}(A) \,=\, \Pr_{z\sim \mathcal{D}}\big[ z \models A \big].
\]
- 目的: 最小限の述語で \(\text{Prec}(A)\ge 1-\epsilon\) を満たしつつ、\(\text{Cov}(A)\) を最大化。

#### 推定と保証（サンプリング）
- サンプル \(\{z_i\}_{i=1}^n \sim \mathcal{D}(\cdot\mid A)\) に対し、経験精度 \(\hat{p} = \tfrac{1}{n}\sum \mathbf{1}[f(z_i)=y]\)。
- Hoeffding 不等式で信頼区間を構成し、\(\Pr\big[ \text{Prec}(A) < \hat{p} - \beta(n,\delta) \big] \le \delta\)。
  ここで \(\beta(n,\delta) = \sqrt{\tfrac{1}{2n}\ln\tfrac{1}{\delta}}\)。
- 停止基準: \(\hat{p} - \beta(n,\delta) \ge 1-\epsilon\) になった時点で Anchor を受理（高精度保証）。

#### 探索（ビームサーチ＋バンディット）
- 候補の述語を逐次追加（ビーム幅 B）。
- 各候補 Anchor に対してサンプリング予算を割当て、上限信頼境界（UCB）で有望な候補に多くサンプルを配分（効率化）。
- 受理条件を満たす最小 Anchor を優先しつつ、被覆 \(\text{Cov}(A)\) を最大化する候補を選択。

---

### 3. 実装ノート
- **述語設計**: テキスト（n-gram の存在）、表形式（列=述語）、画像（領域/スーパーピクセル固定 など）。
- **局所分布**: 条件に含まれない特徴をランダム化/摂動。データドリブン（列の実分布からサンプル）で分布外を避ける。
- **ハイパラ**: \(\epsilon\)（許容誤差）、\(\delta\)（信頼度）、ビーム幅 B、最大述語長。精度保証を優先するなら \(\epsilon\) を小さく設定。

---

### 4. 「キモ」と重要性
- **キモ**: 「十分条件（Anchor）」を高確率で保証する局所規則として提示し、ユーザに“この条件下ではモデルはこう振る舞う”を伝える。
- **重要性**: 線形近似（LIME）と補完的。ユーザ研究で、未知サンプルのモデル挙動予測を高精度・低コスト化。

---

### 5. まとめ（要点）
- 精度: \(\text{Prec}(A)\ge 1-\epsilon\) を確率 \(1-\delta\) で保証（サンプリング＋Hoeffding）。
- 目的: 精度制約の下で被覆最大・規則を簡潔に。
- モデル非依存・局所的・高精度規則による説明。

参考: [AAAI 2018 Anchors 論文ページ](https://ojs.aaai.org/index.php/AAAI/article/view/11491)
