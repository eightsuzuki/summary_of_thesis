# Sequential Integrated Gradients: a simple but effective method for explaining language models

**著者**: Joseph Enguehard (Babylon Health, Skippr)
**会議**: Findings of the Association for Computational Linguistics: ACL 2023 [cite: https://aclanthology.org/2023.findings-acl.477.pdf]

この論文は、言語モデルの説明性を向上させるために、Integrated Gradientsを逐次的に適用する「Sequential Integrated Gradients (SIG)」という手法を提案しています。

---

### 1. 新規性：この論文の最も大きな貢献と、既存研究と比べて何が新しいのか

この論文の最大の貢献は、言語モデルにおけるトークン間の依存関係を考慮して、Integrated Gradientsを**逐次的に適用**することで、より正確で解釈しやすいアトリビューションを生成する手法を提案した点です。

既存のIntegrated Gradients (IG)は、全てのトークンを同時に考慮してアトリビューションを計算しますが、言語モデルではトークンが順序依存であるため、この逐次的なアプローチがより自然で効果的であることが示されています。

### 2. 理論/手法の核心：提案されている理論や手法の要点

Sequential Integrated Gradients (SIG)の核心は、トークンを**順番に**処理し、各トークンのアトリビューションを計算する際に、それまでのトークンの情報を考慮することです。

#### Sequential Integrated Gradients (SIG) の基本的な考え方

- 従来のIG: 全てのトークンを同時にベースラインから実際の入力へと変化させる
- SIG: トークンを1つずつ順番に処理し、各ステップでそのトークンのアトリビューションを計算する

この逐次的なアプローチにより、言語モデルの順序依存性をより適切に反映したアトリビューションが得られます。

#### SIG、IDG、IGの比較表

| 項目 | IG (Integrated Gradients) | IDG (Integrated Directional Gradients) | SIG (Sequential Integrated Gradients) |
|------|---------------------------|----------------------------------------|---------------------------------------|
| **基本的なアプローチ** | ベースラインから入力までの直線経路上で勾配を積分 | 特徴量グループの方向への方向性微分を積分 | トークンを逐次的に処理しながらIGを適用 |
| **対象とする特徴量** | 個別の特徴量（単一特徴量） | 特徴量グループ（複数特徴量の相互作用） | 個別のトークン（順序依存） |
| **計算式** | $\text{IG}_i(x) = (x_i - x'_i) \times \int_{\alpha=0}^{1} \frac{\partial F(x' + \alpha(x - x'))}{\partial x_i} d\alpha$ | $IDG(S) = \int_{\alpha=0}^{1} \nabla_{S}f(b+\alpha(x-b)) d\alpha$ | トークンを順番に処理し、各ステップでIGを計算 |
| **主な適用領域** | 汎用的（画像、テキストなど） | NLP（構文解析木などの階層構造） | 言語モデル（トランスフォーマーなど） |
| **順序依存性の考慮** | なし（全ての特徴量を同時に処理） | なし（グループ間の相互作用を扱う） | あり（トークンを順番に処理） |
| **特徴量間の相互作用** | 個別特徴量のみ（相互作用は暗黙的） | 明示的にグループの相互作用を評価 | 逐次処理により前のトークンの影響を考慮 |
| **階層的説明** | 不可 | 可能（構文解析木などの階層構造に対応） | 不可（単一トークンレベル） |
| **公理的保証** | Sensitivity, Implementation Invariance, Completeness, Symmetry-Preserving | 8つの公理（非負性、単調性、超加法性など） | IGの公理を継承（逐次処理により言語モデルに適応） |
| **計算コスト** | 中（経路積分の近似計算） | 高（グループごとの方向性微分計算） | 高（各トークンごとにIGを計算） |
| **実装の複雑さ** | 低（シンプルな勾配積分） | 中（方向性微分と配当計算） | 低（IGを逐次的に適用するだけ） |
| **主な利点** | 公理に基づく理論的堅牢性、実装の簡潔さ | 特徴量グループの相互作用を明示的に評価、階層的説明が可能 | 言語モデルの順序依存性を適切に反映 |
| **主な制限** | 特徴量間の相互作用を明示的に扱えない | 計算コストが高い、グループの定義が必要 | 計算コストが高い（トークン数に比例） |

### 3. 「キモ」と重要性：この論文の核となるアイデアと分野への影響

* **論文の「キモ」 (核となるアイデア)**
    1. **逐次的な処理**: 言語モデルの本質的な特性である順序依存性を、アトリビューション計算に組み込むことで、より自然で正確な説明を生成
    2. **シンプルさ**: 既存のIG手法を拡張するだけで実装可能であり、複雑な新しい理論を必要としない実用的なアプローチ

* **分野への重要性と影響**
    1. **言語モデルへの適用性**: 特にトランスフォーマーなどの言語モデルにおいて、より解釈しやすいアトリビューションを提供
    2. **実用性**: シンプルな実装でありながら効果的であるため、実務での採用が容易

---

**参考文献**:
- Enguehard, J. (2023). Sequential Integrated Gradients: a simple but effective method for explaining language models. Findings of the Association for Computational Linguistics: ACL 2023. https://aclanthology.org/2023.findings-acl.477.pdf

