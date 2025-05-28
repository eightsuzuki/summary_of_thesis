# KERMIT: Knowledge Graph Completion of Enhanced Relation Modeling with Inverse Transformation

**著者**: 
- Haotian Li¹ (哈尔滨工業大学)
- Bin Yu² (哈尔滨工業大学(威海))
- Yuliang Wei²,³ (哈尔滨工業大学(威海)/山東省産業ネットワークセキュリティ重点研究所) 
- Kai Wang² (哈尔滨工業大学(威海))
- Richard Yi Da Xu⁴ (香港バプティスト大学)
- Bailing Wang¹ (哈尔滨工業大学)

¹ Research Institute of Cyberspace Security, Harbin Institute of Technology, China  
² School of Computer Science and Technology, Harbin Institute of Technology at Weihai, China  
³ Shandong Key Laboratory of Industrial Network Security, China  
⁴ Department of Mathematics, Hong Kong Baptist University, Hong Kong, China


---

### 1. **記述の予測（Predictive Description）**

### 問題設定

知識グラフ $G = (E, R, T, D)$ を次の要素で定義します:

- $E$: エンティティの集合
- $R$: 関係の集合
- $T = \{(h, r, t)\}$: 事実の三重項集合
- $D$: 各エンティティや関係のテキスト記述

知識グラフ完成（KGC）のタスクでは、クエリ $(h, r, ?)$ に対して最も適切なテイルエンティティ $t$ を見つけることを目指します。

### 記述生成

大規模言語モデル（LLM）を用いて、クエリ $(h, r, ?)$ に基づいた予測記述 $t_{\text{pred}}$ を生成します。LLMへの入力テンプレートを $P_{\text{desc}}$ とすると、予測記述は次式で得られます:
$$
t_{\text{pred}} = \text{LLM}(g_k(h, r) | P_{\text{desc}})
$$
ここで、

- $g_k(h, r)$: $r$ に関連する $k$ 個の事例（few-shot examples）を含むクエリの事前処理
- $P_{\text{desc}}$: 記述生成のためのプロンプトテンプレート

ヘッドエンティティの予測では、$(t, r', ?)$ という形式で逆関係 $r'$ を利用します。

---

### 2. **逆関係のモデリング**

### 逆関係の生成

関係 $r$ に対して逆関係 $r'$ を生成します。これもLLMを用いて以下の式で計算します:
$$
r' = \text{LLM}(g_k(r) | P_{\text{rel}})
$$
ここで、$P_{\text{rel}}$ は逆関係生成用のプロンプトテンプレートです。

---

### 3. **テキストエンコーディングと埋め込み生成**

BERTを用いて、テキスト情報を埋め込み空間にマッピングします。三重項 $(h, r, t)$ に対して、ヘッドと関係のペア $(h, r)$ の埋め込み $e_{hr}$ とテイルエンティティの埋め込み $e_t$ を次式で計算します:
$$
e_{hr} = \text{Pool}(\text{BERT}_{hr}(X_{hr}))
$$
$$
e_t = \text{Pool}(\text{BERT}_t(X_t))
$$
ここで、

- $X_{hr} = [x_{\text{CLS}}, H_{\text{desc}}, x_{\text{SEP}}, R_{\text{desc}}, x_{\text{SEP}}, T_{\text{pred}}, x_{\text{SEP}}]$: ヘッド、関係、予測記述のトークン列
- $X_t = [x_{\text{CLS}}, T_{\text{desc}}, x_{\text{SEP}}]$: テイルエンティティのトークン列
- $\text{Pool}(\cdot)$: 平均プーリング（CLSトークンではなくシーケンス全体を平均）

---

### 4. **スコアリング関数**

三重項 $(h, r, t)$ の妥当性をスコアリングするために、コサイン類似度を用います:
$$
f(h, r, t) = \cos(e_{hr}, e_t) = \frac{e_{hr} \cdot e_t}{\|e_{hr}\| \|e_t\|}
$$

---

### 5. **教師付きコントラスト学習**

### 損失関数

クエリ $(h, r, ?)$ に対して、ポジティブサンプル $t$ とネガティブサンプル $t'$ を用いて損失関数を次のように定義します:
$$
\mathcal{L} = -\frac{1}{|N^+|} \sum_{t \in N^+} \log \frac{\exp((f(h, r, t) - \gamma) / \tau)}{\exp((f(h, r, t) - \gamma) / \tau) + \sum_{t' \in N^-} \exp((f(h, r, t') - \gamma) / \tau)}
$$
ここで、

- $N^+$: ポジティブサンプル集合
- $N^-$: ネガティブサンプル集合
- $\gamma$: 加算マージン
- $\tau$: 温度パラメータ

この損失関数は、ポジティブサンプル間の類似度を最大化し、ネガティブサンプルとの類似度を最小化します。

---

### 6. **推論**

推論では、すべての候補エンティティの埋め込みを事前計算し、以下のスコアを用いてランキングします:
$$
t^* = \arg\max_{t \in E} f(h, r, t)
$$

---

### 結論

提案手法KERMITは、LLMを用いた予測記述と逆関係の生成、教師付きコントラスト学習を組み合わせることで、知識グラフ完成タスクの性能を向上させることを目指しています。この理論と数式は、ミスマッチ記述や擬似逆関係の問題に対処し、従来手法を超える結果を達成しました。