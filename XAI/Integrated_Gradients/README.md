# Integrated Gradients (IG) 系の手法

このディレクトリには、Integrated Gradients (IG) およびその拡張手法に関する論文をまとめています。

## 主要な論文

### 基礎論文

1. **Integrated Gradients: Axiomatic Attribution for Deep Networks** (ICML 2017)
   - IGの原典。公理的なアプローチに基づく特徴重要度の計算手法を提案。

2. **A Rigorous Study of Integrated Gradients Method and Extensions to Internal Neuron Attributions** (arXiv)
   - IGの理論的基盤を再検証し、内部ニューロンへの拡張を提案。

### 拡張手法

3. **Expected Gradients**
   - **Visualizing the Impact of Feature Attribution Baselines** (Distill 2020)
     - ベースライン選択の重要性を視覚的に証明。Expected Gradientsを紹介。
   - **Improving performance of deep learning models with axiomatic attribution priors and expected gradients** (Nature Machine Intelligence 2021)
     - Expected Gradientsの提案論文。分布ベースラインを使用。

4. **Neuron Integrated Gradients**
   - **Computationally Efficient Measures of Internal Neuron Importance** (arXiv 2018)
     - Total ConductanceとPath Integrated Gradientsの同等性を示し、Neuron Integrated Gradientsを提案。

5. **Sequential Integrated Gradients**
   - **Sequential Integrated Gradients: a simple but effective method for explaining language models**
     - 言語モデル向けのSequential IGを提案。

6. **Integrated Directional Gradients (IDG)**
   - **IDG:Integrated Directional Gradients: Feature Interaction Attribution for Neural NLP Models**
     - NLPモデルにおける特徴間相互作用のアトリビューションを計算。

## 関連トピック

- **ベースライン選択**: ゼロベクトル、<PAD>トークン、分布ベースラインなど
- **OOD問題**: ベースラインが学習分布外であることによる問題
- **内部ニューロン**: モデル内部のニューロンの重要度評価
- **特徴間相互作用**: 複数の特徴が組み合わさった場合の寄与
