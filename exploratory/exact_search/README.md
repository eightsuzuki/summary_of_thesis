# 厳密探索（Exact Search）

このディレクトリは、**厳密探索アルゴリズム**を用いたXAI手法をまとめます。

## 厳密探索とは

厳密探索は、**最適解を保証する**探索手法です。問題の全解空間を体系的に探索し、最適解を見つけることができます。

## 特徴

- **最適性保証**: 最適解を見つけることが保証される
- **計算コスト**: 問題サイズが大きい場合、計算コストが高い
- **完全性**: 解が存在する場合、必ず見つけることができる

## XAIへの応用

- 最適な説明の探索
- 特徴選択の最適化
- ルール抽出の最適化
- 説明の最小化問題

## 主な手法

### 実装済み

- [Branch and Bound](./Branch%20and%20Bound.md)
- [Dynamic Programming](./Dynamic%20Programming.md)
- [Linear Programming](./Linear%20Programming.md)

### その他の手法

- 全探索（Exhaustive Search）
- バックトラッキング（Backtracking）
- 分岐法（Branching）

