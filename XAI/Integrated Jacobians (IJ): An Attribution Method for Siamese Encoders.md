# Integrated Jacobians (IJ): An Attribution Method for Siamese Encoders

**著者**: Lucas Moeller, Dmitry Nikolaev, Sebastian Padó (University of Stuttgart)  
**会議**: EMNLP 2023  
**論文**: An Attribution Method for Siamese Encoders  
**DOI**: 10.18653/v1/2023.emnlp-main.980  
**arXiv**: https://arxiv.org/abs/2310.05703

一言でまとめるとこの論文は：

> **「Siamese（Sentence-BERT 系）モデルの予測を、*入力 a の要素 × 入力 b の要素* の“相互作用”として、数学的に破綻なく・厳密に分解して可視化した」**

そのための**道具が IJ（Integrated Jacobians）**である。以下、**IJ を軸にして**「何をしている論文か」を整理する。

---

## 1. そもそも何が問題だった？

Sentence-BERT / Siamese Encoder は

$$f(a,b) = e(a)^\top e(b)$$

- $a$ 単体の重要度 → 意味がない  
- $b$ 単体の重要度 → 意味がない  

**比較モデルなのに、既存の attribution は全部「単入力前提」**。

> 「どの単語が重要か？」ではなく  
> **「どの単語 *と* どの単語が対応したか？」**  
> が知りたいのに、手段がなかった。

---

## 2. IJ は何のために導入された？

### 目的

IG の良さ（完全性・厳密性）を壊さずに、

- 出力が **スカラー**
- 中間が **ベクトル（埋め込み）**
- 入力が **2つ**

という Siamese 構造を説明したい。

👉 そのために

- 勾配（gradient）では足りない  
- **埋め込みへの感度 ＝ Jacobian**  
- それを **IG と同じ発想で積分**  

→ **Integrated Jacobians (IJ)**

---

## 3. 相互作用だけを取り出す：inclusion–exclusion

IJ の式の「肝」は、次の差分である。

$$f(a,b) - f(a,r_b) - f(r_a,b) + f(r_a,r_b)$$

| 項 | 意味 |
|----|------|
| $f(a,b)$ | 普通の予測 |
| $f(a,r_b)$ | **$b$ を無効化**したときの $a$ の単独効果 |
| $f(r_a,b)$ | **$a$ を無効化**したときの $b$ の単独効果 |
| $f(r_a,r_b)$ | 両方無効（基準値、0 にしたい） |

これは **inclusion–exclusion** の形で、

- $f(a,b)$：全部入り  
- $-f(a,r_b)$：$a$ だけの効果を引く  
- $-f(r_a,b)$：$b$ だけの効果を引く  
- $+f(r_a,r_b)$：引きすぎた基準を戻す  

👉 **単独効果を全部消して、相互作用だけ残す**。

### 微分で見ると

この差分は積分で書くと（論文 Eq.8）：

$$\int_{r_b}^{b} \int_{r_a}^{a} \frac{\partial^2 f(x,y)}{\partial x_i \partial y_j} \, dx_i \, dy_j$$

- 1階微分 → 単独効果  
- **混合2階微分** $\dfrac{\partial^2}{\partial x_i \partial y_j}$ → **相互作用**  

つまり「$a$ をちょっと動かしたとき、その影響が $b$ にどれくらい依存するか」を積分して全部足している。

---

## 4. IJ で何が手に入る？

### (1) 各入力に対する「埋め込み感度」：Integrated Jacobian

$$J_a \in \mathbb{R}^{k \times d_a}, \quad J_b \in \mathbb{R}^{k \times d_b}$$

$(J_a)_{ki} = \int_0^1 \frac{\partial e_k(x(\alpha))}{\partial x_i} d\alpha$（$J_b$ も同様）

これは「入力の各成分が、埋め込み空間の各軸をどう動かすか」を **reference → 入力の全経路で積分したもの**。IG の「勾配の積分」を **Jacobian の積分**にしたもの。

### (2) それを“掛け算”して相互作用にする

$$J_a^\top J_b \in \mathbb{R}^{d_a \times d_b}$$

- 次元 $k$（埋め込み）を潰して  
- 入力 $a$ と入力 $b$ の **成分ペア空間**に落とす  

👉 **feature-pair interaction map**

### (3) 完全性を保ったままペア寄与へ

$$A_{ij} = (a-r_a)_i \, (J_a^\top J_b)_{ij} \, (b-r_b)_j$$

- 各 $(i,j)$：**ペア貢献度**  
- 全部足すと：$\displaystyle \sum_{i,j} A_{ij} = f(a,b)$  

👉 **「説明の合計 ＝ 予測」が厳密に成り立つ**

---

## 5. つまり、この論文がやったこと（IJ 視点）

### 技術的に

- IG を **2入力・ベクトル中間表現**に拡張  
- 勾配 → **IJ（積分ヤコビアン）**  
- 混合2階微分を **Siamese 構造で閉じた形に分解**  
- Siamese 特有の $f(x,y)=\sum_k e_k(x)e_k(y)$ をフル活用  

### 方法論的に

- Sentence-BERT の予測を **token × token の行列**として説明可能にした  
- Attention でも LRP でもない **“prediction-faithful” な attribution**  

### 実験的に

IJ ベースの attribution を使って、

- どの token ペアが支配的か  
- noun–noun / verb–verb が強い  
- 深い層ほど attribution が安定  
- 出力層は情報を潰しすぎる  

などを分析している。

---

## 6. 一文で言うと（研究的立ち位置）

> **「Siamese モデルの“比較”という本質を、数学的に正しい attribution として初めて扱えた論文」**

IJ は主役というより、

> **“比較モデルを説明するために必要だった最小限で正しい道具”**

という位置づけ。

---

## 7. VAIG との違い（1入力・ベクトル出力の場合）

**VAIG**（本論付録で扱う Vector Attribution Integrated Gradients）は、

- **1つの**ベクトル値ブロック（例：$z \to \mathrm{ATT} \to u$）を考える  
- 入力は1組 $\{z_i\}$、出力はベクトル $u_j$  
- **第二の入力は存在しない**  

そのため VAIG では、

- inclusion–exclusion の差分 $f(a,b)-f(a,r_b)-f(r_a,b)+f(r_a,r_b)$ は**使わない**  
- 混合2階微分 $\partial^2 f/\partial x_i \partial y_j$ も**使わない**  
- 出力の各次元 $d$ について **1入力・スカラー出力の通常の IG**（1階微分の積分）を実行するだけ  

👉 **VAIG は IJ の数式を拡張したものではなく、対象が異なるため数式の形が根本的に異なる**。共通するのは「経路に沿ったヤコビ（あるいは出力次元ごとの勾配）の積分」というアイデアのみ。

| | **IJ** | **VAIG** |
|---|--------|----------|
| 入力 | 2つ $a$, $b$（Siamese） | 1組 $\{z_i\}$（1ブロック） |
| 出力 | スカラー $f(a,b)$ | ベクトル $u_j$ |
| 使う式 | inclusion–exclusion ＋ 混合2階 | 使わない（1階のみ） |
| 結果の形 | ペア行列 $A_{ij}$ | 入力ごとの寄与ベクトル |

---

## 8. 研究視点に引き寄せると

- IJ $\approx$ **埋め込み空間上の局所線形化**  
- $J_a^\top J_b$ $\approx$ **低ランク interaction kernel**  
- 混合 Hessian を **構造制約付きで分解したもの**  

XAI / 表現解析 / 幾何の文脈ではかなり美しい定式化である。

---

## 参考文献

- Moeller, L., Nikolaev, D., & Padó, S. (2023). An Attribution Method for Siamese Encoders. In *Proceedings of EMNLP 2023*, pp. 15818–15827.  
  https://aclanthology.org/2023.emnlp-main.980/
