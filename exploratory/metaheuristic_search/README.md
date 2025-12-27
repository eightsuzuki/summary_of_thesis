# メタヒューリスティック探索（Metaheuristic Search）

このディレクトリは、**メタヒューリスティック探索アルゴリズム**を用いたXAI手法をまとめます。

## メタヒューリスティック探索とは

メタヒューリスティック探索は、**汎用的な探索フレームワーク**で、特定の問題に依存しない探索手法です。最適解の保証はありませんが、複雑な問題に対して良い解を見つけることができます。

## 特徴

- **汎用性**: 様々な問題に適用可能
- **柔軟性**: 問題に応じて調整可能
- **探索能力**: 局所最適解を回避する能力がある
- **計算コスト**: 厳密探索より低いが、局所探索より高い場合がある

## XAIへの応用

- 複雑な説明の探索
- 多目的最適化による説明
- 特徴選択の最適化
- 説明の多様性の確保

## 主な手法

### 実装済み

- [GA: Genetic Algorithm](./GA:%20Genetic%20Algorithm.md)
- [PSO: Particle Swarm Optimization](./PSO:%20Particle%20Swarm%20Optimization.md)
- [ACO: Ant Colony Optimization](./ACO:%20Ant%20Colony%20Optimization.md)
- [SA: Simulated Annealing](./SA:%20Simulated%20Annealing.md)
- [Tabu: Tabu Search](./Tabu:%20Tabu%20Search.md)

### その他の手法

- 差分進化（Differential Evolution）
- 進化戦略（Evolution Strategy）
- 人工蜂コロニー（Artificial Bee Colony）
- ホタルアルゴリズム（Firefly Algorithm）

