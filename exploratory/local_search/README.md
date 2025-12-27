# 局所探索（Local Search）

このディレクトリは、**局所探索アルゴリズム**を用いたXAI手法をまとめます。

## 局所探索とは

局所探索は、**現在の解の近傍を探索**し、より良い解を見つける手法です。最適解の保証はありませんが、効率的に良い解を見つけることができます。

## 特徴

- **効率性**: 計算コストが比較的低い
- **局所最適**: 局所最適解に収束する可能性がある
- **初期解依存**: 初期解の選択が結果に影響する

## XAIへの応用

- 局所的な説明の生成
- 特徴の重要度の推定
- ルールの局所的最適化
- 近傍サンプリングによる説明

## 主な手法

### 実装済み

- [Gradient Descent](./Gradient%20Descent.md)
- [Hill Climbing](./Hill%20Climbing.md)
- [Steepest Descent](./Steepest%20Descent.md)
- [Conjugate Gradient](./Conjugate%20Gradient.md)

### その他の手法

- 確率的勾配降下法（Stochastic Gradient Descent）
- モメンタム法（Momentum）
- Adam法（Adaptive Moment Estimation）

