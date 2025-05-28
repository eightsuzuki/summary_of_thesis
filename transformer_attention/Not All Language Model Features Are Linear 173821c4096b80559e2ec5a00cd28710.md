# Not All Language Model Features Are Linear

### 概要（数式を含む詳細な説明）

この論文は、言語モデルが内部でどのように特徴を表現しているのかを明らかにし、多次元かつ還元不可能な特徴（non-separable and irreducible features）の存在を提案しています。以下に各セクションの主要内容を数式を用いて説明します。

---

### **1. 背景**

![image.png](<image/Not All Language Model Features Are Linear 173821c4096b80559e2ec5a00cd28710/image.png>)

### **従来の仮説: 線形表現仮説**

- **線形表現仮説**では、特徴は一次元の線形空間に埋め込まれると考えられていました。例えば、次のような線形操作が可能です：
    $$
    f(king)−f(man)+f(woman)=f(queen)f(\text{king}) - f(\text{man}) + f(\text{woman}) = f(\text{queen})
    $$
    
- 本研究では、この一次元仮説を拡張し、多次元特徴の存在を検討します。

---

### **2. 定義**

### **多次元特徴の定義**

- 特徴 $\mathbf{f}$ は、入力空間の部分集合から $\mathbb{R}^{d_f}$ への写像：
    $$
    \mathbf{f} : T \to \mathbb{R}^{d_f}
    $$
    
- 特徴のスパース性 ： $s$
    $$
    s = 1 - P(\text{feature is active})
    $$
    
- 特徴 $\mathbf{f}$ が分離不可能である場合、以下を満たさない：

（ $\mathbf{f}$ は統計的に独立な部分特徴 $\mathbf{a}, \mathbf{b}$ に分解できない。）
    $$    
    p(\mathbf{a}, \mathbf{b}) = p(\mathbf{a}) \cdot p(\mathbf{b})
    $$
    
### **還元不可能な特徴**

- 特徴 $\mathbf{f}$ が還元可能である条件：

ここで、$p_1, p_2$ は互いに重ならない分布。
    $$
    p(\mathbf{a}, \mathbf{b}) = w \cdot p_1(\mathbf{a}, \mathbf{b}) + (1-w) \cdot p_2(\mathbf{a}, \mathbf{b})
    $$
    
- 還元不可能とは、上述の条件を満たさない特徴を指します。

---

### **3. メソッド**

### **スパースオートエンコーダ (SAE)**

スパースオートエンコーダは、過完備な基底を用いて特徴を分解します。損失関数は以下のように定義されます：
$$
DL(X_{i,l}) = \arg\min_{\mathbf{E}, \mathbf{D}} \sum_{\mathbf{x}_{i,l} \in X_{i,l}} \left[ \|\mathbf{x}_{i,l} - \mathbf{D} \cdot \text{ReLU}(\mathbf{E} \cdot \mathbf{x}_{i,l})\|_2^2 + \lambda \|\text{ReLU}(\mathbf{E} \cdot \mathbf{x}_{i,l})\|_0 \right]
$$
- **第1項**: 再構成誤差。隠れ状態 $\mathbf{x}_{i,l}$ を辞書行列 $\mathbf{D}$ で再構成する際の誤差。
    
- **第2項**: スパース性ペナルティ。非ゼロの活性化を抑制。

### **円形表現の発見**

- 曜日や月を円形にエンコード：
    $$
    \text{circle}(\alpha) = \left[ \cos\left(\frac{2\pi \alpha}{m}\right), \sin\left(\frac{2\pi \alpha}{m}\right) \right]
    $$
    - 曜日では $m = 7$、月では $m = 12$。
        
- 円形プローブの学習：
    $$
    \mathbf{P} = \arg\min_{\mathbf{P}'} \sum_{\mathbf{x}_{i,l}^j} \|\mathbf{P}' \cdot \mathbf{W}_{i,l} \cdot \mathbf{x}_{i,l}^j - \text{circle}(\alpha)\|_2^2
    $$
    ここで、$\mathbf{W}_{i,l}$ は主成分方向、$\mathbf{P}'$ は円形表現を学習する線形変換行列。
    

---

### **4. 実験結果**

### **曜日・月タスク**

- タスク例：
    - 「Mondayから2日後は？」（Weekdaysタスク）。
    - 「Januaryから4ヶ月後は？」（Monthsタスク）。
- モジュラー算術に基づくこれらのタスクで、多次元表現が活用されていることを確認。

### **介入実験**

- **Activation Patching**:
特定のサブスペース（円形表現部分）の隠れ状態を「クリーンな実行」の値に置換し、タスク性能への影響を評価。
- **結果**:
    - 曜日・月の計算で、円形サブスペースが計算結果に重要であることを確認。

---

### **5. 議論と結論**

1. **多次元表現の必要性**:
    - 還元不可能な多次元表現は、特定のタスク（例: 曜日や月の計算）で計算の基本単位となっている。
2. **新しい手法の提案**:
    - スパースオートエンコーダや円形プローブを用いた多次元特徴の発見は、言語モデルの内部表現を解明するための重要な手段。
3. **将来への展望**:
    - より複雑なモデルやタスクへの適用を通じて、AIシステムの透明性と解釈可能性を高める。