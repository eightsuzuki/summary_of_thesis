# A Rigorous Study of Integrated Gradients Method and Extensions to Internal Neuron Attributions

**著者**: Mukund Sundararajan (Google Research), Ankur Taly (Google Research), Qiqi Yan (Google Research)  
**arXiv**: [arXiv:1703.01365](https://arxiv.org/abs/1703.01365)

### 概要
この論文は、深層学習（DL）モデルの解釈手法として広く利用されている**Integrated Gradients (IG)**の理論的基盤を厳密に再検証し、その正当性を強化すると同時に、実用的な拡張を提案する研究です。論文は主に、IGの優位性の根拠とされてきた**「唯一性」の主張に潜む理論的な欠陥を初めて明確に指摘**し、それを修正するための**新しい公理「NDP」を導入**することで、その理論基盤を再構築しました [cite: 4, 5, 23, 27]。さらに、実践で広く使われている「分布ベースライン」手法に対する公理を初めて定式化し [cite: 7, 32]、内部ニューロンの貢献度を「画像の特定領域」に絞って分析する、計算効率の高い新手法を提案・検証しています [cite: 8, 33]。

---

### 論文の核心と最大の新規性：IGの理論的基盤の欠陥修正

この論文の最も重要かつ独創的な貢献は、IGの理論的正当性の中核であった「唯一性の主張」を根本から見直し、その欠陥を修正した点にあります。

#### 1. 従来の主張とその理論的背景
IGの元論文は、優れた説明手法が満たすべき4つの公理（**Completeness, Linearity, Sensitivity(b), Implementation Invariance**）を提示し、「**これら4つの公理を常に満たすのはパスメソッドだけである**」と主張しました [cite: 68]。これは、ゲーム理論の費用分担問題に関する研究を引用したものでした [cite: 104, 106]。

#### 2. 論文が指摘した理論的な「穴」
本論文の著者らは、この論理展開に決定的な「穴」があると指摘しました。それは、**理論を借用した「費用分担問題」と、適用先の「DLモデル」とで、関数の前提条件が根本的に異なる**という点です [cite: 21, 109, 130]。
* **費用分担問題の前提**: コスト関数は**非減少**、アトリビューションは**非負** [cite: 114, 117]。
* **DLモデルの現実**: 関数は一般に**単調ではなく**、アトリビューションは予測を下げる**負の貢献**を表現できなければならない。

この前提の違いにより、元の証明はDLの文脈に適用できず、IGが「唯一」であるという主張は**厳密には偽であった**と結論付けました [cite: 23, 131]。

#### 3. 解決策としての新公理「NDP」の導入
この理論的な欠陥を修正するため、著者らは新しい公理**NDP (Non-Decreasing Positivity)**を導入しました [cite: 162]。
* **NDPの定義**: もし関数 $F$ が、ベースライン $x'$ から入力 $x$ への経路で常に**非減少**であるならば、そのアトリビューション $A(x, x', F)$ は**非負（$\ge 0$）**でなければならない、というルールです [cite: 165, 166]。
* **NDPの役割**: この公理は、DLの関数が費用分担問題のコスト関数のように「非減少」で振る舞う特殊なケースに限って制約を課すことで、元の証明が抱えていた論理的なギャップを埋める「橋渡し」の役割を果たします [cite: 167, 169]。
* **理論の再構築**: 元の4公理にNDPを加えた5つの公理を満たす手法は、「単調なパスメソッドのアンサンブル」と等価であることを厳密に証明しました [cite: 172, 173]。これにより、IGの理論的基盤をより強固なものに再構築することに成功しました。

---

### その他の主要な貢献と新規性

#### 1. 分布ベースラインの公理化と理論的整合性の確保
実践では、単一のベースライン（例：黒画像）ではなく、複数のベースライン画像の分布を用いて平均を取る「分布ベースラインIG」がノイズ対策などのためによく使われます [cite: 81, 82, 83]。しかし、元のアトリビューション公理は単一ベースラインを前提としていました [cite: 31]。

本論文では、この実践で広く使われている手法に対し、初めて理論的な裏付けを与えました。
* 分布ベースラインの手法に対応する公理（例：**Completeness (分布版)**）を新たに定式化しました [cite: 199]。
* 具体的には、アトリビューションの合計は、元の出力と**ベースライン分布上の期待値**との差に等しい、と定義されます [cite: 202]。
    $$\sum_{i=1}^{n}E_{i}(x,X^{\prime},F)=F(x)-\mathbb{E}[F(X^{\prime})]$$
* そして、分布ベースラインIGがこれらの新しい公理を満たすことを示し、理論と実践の間の溝を埋めました [cite: 200]。

#### 2. 画像パッチに対する内部ニューロンアトリビューションと高速化
IGを応用し、入力特徴量だけでなく「モデル内部のどのニューロンが重要か」を説明する研究があります [cite: 205]。本論文はこれをさらに一歩進め、「**入力画像の特定領域（パッチ）のアトリビューションに対して、どの内部ニューロンが貢献しているか**」を特定する効率的な手法を提案しました [cite: 220]。

* ある入力パッチ S に対する内部ニューロン j の貢献度 $IG_{S,j}$ は、以下の式で計算されます [cite: 221]。
    $$IG_{S,j}=\sum_{i\in S}(x_{i}-x_{i}^{\prime})\int_{0}^{1}\frac{\partial G}{\partial H_{j}}(H(\gamma))\frac{\partial H_{j}}{\partial x_{i}}(\gamma)dt$$
* ここで H は内部層までのネットワーク、G はそれ以降のネットワークです。この計算、特にヤコビアン行列の項（$\frac{\partial H_{j}}{\partial x_{i}}$）は、全ニューロンに対して行うと計算コストが非常に高くなります [cite: 215, 216, 222]。
* 著者らは、これを方向微分として近似することで、計算コストの高いヤコビアン行列の計算を回避する**高速な計算手法**を導入しました [cite: 223]。
はい、承知いたしました。ご提示の数式について、その目的、各項の意味、そして全体の計算の流れを詳細に解説します。

この数式は、**「入力画像のある特定領域（パッチ S）が持つ重要度（アトリビューション）のうち、どれだけが特定の内部ニューロン j を『経由』して生み出されたのか？」**を定量化するためのものです。

---

### **数式の目的：貢献の経路を特定する**

まず、この式が答えようとしている問いを理解することが重要です。

* **通常のIG**: 「どの**ピクセル**が最終予測に重要か？」
* **通常の内部ニューロンアトリビューション**: 「どの**ニューロン**が（画像全体の）最終予測に重要か？」
* **この式の問い**: 「**パッチS**の重要度は、どの**ニューロン j** を経由しているか？」

つまり、単にピクセルやニューロンの重要度を測るだけでなく、**「ピクセル → ニューロン → 最終出力」という貢献の経路**を、特定の領域に絞って分析するための計算式です。

---

### **数式の詳細な分解解説**

$$IG_{S,j} = \int_{0}^{1} \underbrace{\frac{\partial G}{\partial H_{j}}(H(\gamma))}_{\text{② ニューロンから出力への感度}} \times \underbrace{\left( \sum_{i\in S} \frac{\partial H_{j}}{\partial x_{i}}(\gamma) (x_{i}-x_{i}^{\prime}) \right)}_{\text{① パッチSからニューロンへの影響力}} dt$$

この式を理解するために、積分の内部を構成する2つの主要なパーツに分けて解説します。

#### **パーツ①： パッチSからニューロンjへの影響力**
$$\sum_{i\in S} \frac{\partial H_{j}}{\partial x_{i}}(\gamma) (x_{i}-x_{i}^{\prime})$$

この部分は、積分経路上の ある一点 $\gamma = \gamma(t)$ において、「**画像パッチS全体がニューロンjにどれだけの影響を与えているか**」を計算しています。この中身をさらに分解します。

* **$(x_{i}-x_{i}^{\prime})$**: パッチS内のピクセル $i$ が、ベースラインから入力値までどれだけ変化したかを表す「変化量」です。 [cite: 210]
* **$\frac{\partial H_{j}}{\partial x_{i}}(\gamma)$**: ニューロン $j$ の活性値が、ピクセル $i$ の値の変化に対してどれだけ敏感かを示す「感度」です。 [cite: 210] これは、ピクセル $i$ とニューロン $j$ の間の結合の強さと考えることができます。
* **$\sum_{i\in S}$**: パッチS内の**すべてのピクセル**について、上記の「変化量 × 感度」を合計します。 [cite: 221] これにより、個々のピクセルの影響を統合し、パッチS全体としてのニューロン $j$ への瞬間的な「押し込み（Push）量」を算出します。

#### **パーツ②： ニューロンjから最終出力への感度**
$$\frac{\partial G}{\partial H_{j}}(H(\gamma))$$

この部分は、同じく積分経路上の ある一点 $\gamma$ において、「**ニューロンjの活性値が少し変化したとき、最終的なモデルの出力がどれだけ変化するか**」を示します。 [cite: 210]

つまり、その瞬間における**ニューロンjの最終予測に対する「重要度」や「影響力」**を表します。この値が大きいほど、ニューロンjはその時点でのモデルの意思決定において重要な役割を担っていると言えます。

---

### **全体の計算の流れ**

この数式は、以下のステップで計算を実行しています。

1.  ベースラインから入力への経路を細かく分割し、各ステップ（各 $t$ ）で以下の計算を行います。
    a.  **パッチSからニューロンjへの影響力**（パーツ①）を計算します。
    b.  **ニューロンjから最終出力への重要度**（パーツ②）を計算します。
    c.  これら2つを掛け合わせます。これにより、「**パッチSから来て、重要度の高いニューロンjを経由した、その瞬間の貢献フロー**」が算出されます。

2.  最後に、この「貢献フロー」を経路全体（$t=0$ から $1$ まで）で**合計（積分）**します。

これにより、経路全体を通した、パッチSからニューロンjを経由して最終出力に至るトータルの貢献度 $IG_{S,j}$ が得られます。

#### **補足：高速化のポイント**

前回の説明の通り、この計算式のパーツ①は、**ヤコビアン行列**（$\frac{\partial H_{j}}{\partial x_{i}}$）の計算を必要とするため、非常に高コストです。論文の技術的な新規性は、この部分を**方向微分**として捉え、ヤコビアンを直接計算せずに済む効率的な近似手法を導入した点にあります。これにより、この詳細な分析が現実的な計算時間で可能になりました。
#### 3. 実験による検証
提案した「画像パッチに対する内部ニューロンアトリビューション」手法の有効性を検証するため、枝刈り（Pruning）実験を行いました [cite: 228, 229]。画像中の特定の物体（例：信号機）に貢献していると特定されたニューロンを不活性化させると、実際にその物体のIGアトリビューションだけが選択的に変化し、モデルの注目が別の物体へ移ることを示しました [cite: 251, 269, 274, 275]。これは、提案手法がニューロンの役割を正確に特定できていることを実証するものです。