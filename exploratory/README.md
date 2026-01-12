# 探索的手法（Exploratory Methods）

このディレクトリは、**最適化問題を解くための探索アルゴリズム**をまとめます。XAI（説明可能なAI）や機械学習の様々な問題に応用される探索手法を分類しています。

---

## Quick comparison table

| Method | Category | Search Strategy | Optimality | Pros | Cons |
| --- | --- | --- | --- | --- | --- |
| Branch and Bound | 厳密探索 | 系統的探索 | 最適解保証 | 最適解が保証される | 計算コストが高い |
| Dynamic Programming | 厳密探索 | 部分問題の最適化 | 最適解保証 | 効率的な最適化 | 問題構造に依存 |
| Gradient Descent | 局所探索 | 勾配方向への移動 | 局所最適 | 実装が簡単、収束が速い | 局所最適解に陥る |
| Hill Climbing | 局所探索 | 近傍の最良解へ移動 | 局所最適 | シンプル | 局所最適解に陥る |
| GA (Genetic Algorithm) | メタヒューリスティック | 進化による探索 | 近似解 | 局所最適回避、並列化可能 | 収束が遅い、パラメータ調整が必要 |
| PSO (Particle Swarm) | メタヒューリスティック | 群れの行動 | 近似解 | 実装が簡単、収束が速い | 高次元で性能低下 |
| ACO (Ant Colony) | メタヒューリスティック | フェロモン情報 | 近似解 | 離散問題に適している | 収束が遅い |
| SA (Simulated Annealing) | メタヒューリスティック | 確率的受理 | 近似解 | 局所最適回避、実装が簡単 | 冷却スケジュール調整が必要 |
| Tabu Search | メタヒューリスティック | タブーリスト | 近似解 | 局所最適回避、多様性保持 | メモリ使用量が増える |

---

## サブディレクトリ

### 1. [Exact Search (厳密探索)](./exact_search/)
最適解を保証する探索手法。全解空間を系統的に探索します。

**主な手法**:
- Branch and Bound（分枝限定法）
- Dynamic Programming（動的計画法）
- Exhaustive Search（全探索）
- Linear Programming（線形計画法）

### 2. [Local Search (局所探索)](./local_search/)
現在の解の近傍を探索し、より良い解を見つける手法。

**主な手法**:
- Gradient Descent（勾配降下法）
- Hill Climbing（山登り法）
- Steepest Descent（最急降下法）
- Conjugate Gradient（共役勾配法）

### 3. [Metaheuristic Search (メタヒューリスティック探索)](./metaheuristic_search/)
汎用的な探索フレームワーク。局所最適解を回避し、複雑な問題に対して良い解を見つけます。

**主な手法**:
- [GA: Genetic Algorithm](./metaheuristic_search/GA:%20Genetic%20Algorithm.md)
- [PSO: Particle Swarm Optimization](./metaheuristic_search/PSO:%20Particle%20Swarm%20Optimization.md)
- [ACO: Ant Colony Optimization](./metaheuristic_search/ACO:%20Ant%20Colony%20Optimization.md)
- [SA: Simulated Annealing](./metaheuristic_search/SA:%20Simulated%20Annealing.md)
- [Tabu: Tabu Search](./metaheuristic_search/Tabu:%20Tabu%20Search.md)

---

## Chronology and influences

### 厳密探索
- **1950s**: 動的計画法（Bellman）– 最適化問題の系統的解法
- **1960s**: 分枝限定法（Land & Doig）– 組合せ最適化の厳密解法

### 局所探索
- **1847**: 最急降下法（Cauchy）– 勾配ベースの最適化
- **1950s**: 共役勾配法（Hestenes & Stiefel）– 効率的な勾配法

### メタヒューリスティック探索
- **1975**: Genetic Algorithm（Holland）– 進化計算の基礎
- **1983**: Simulated Annealing（Kirkpatrick et al.）– 焼きなまし法の提案
- **1986**: Tabu Search（Glover）– タブー探索の提案
- **1991**: Ant Colony Optimization（Dorigo）– アリコロニー最適化の提案
- **1995**: Particle Swarm Optimization（Kennedy & Eberhart）– 粒子群最適化の提案

---

## XAIへの応用

探索手法は、XAIの様々な問題に応用されています：

- **特徴選択**: どの特徴が予測に重要かを見つける
- **説明の最適化**: 最適な説明を生成する
- **ルール抽出**: 高精度で簡潔なルールセットを見つける
- **ハイパーパラメータ最適化**: XAI手法のパラメータを最適化する
- **説明の多様性**: 多様な説明を生成する

---

## 参考文献

### 主要論文

**Genetic Algorithm**:
- Holland, J. H. (1975). *Adaptation in Natural and Artificial Systems*. University of Michigan Press.

**Particle Swarm Optimization**:
- Kennedy, J., & Eberhart, R. (1995). Particle swarm optimization. *Proceedings of IEEE International Conference on Neural Networks*, 1942-1948.

**Ant Colony Optimization**:
- Dorigo, M. (1992). Optimization, Learning and Natural Algorithms. *PhD Thesis, Politecnico di Milano*.
- Dorigo, M., Maniezzo, V., & Colorni, A. (1996). Ant system: optimization by a colony of cooperating agents. *IEEE Transactions on Systems, Man, and Cybernetics*, 26(1), 29-41.

**Simulated Annealing**:
- Kirkpatrick, S., Gelatt, C. D., & Vecchi, M. P. (1983). Optimization by simulated annealing. *Science*, 220(4598), 671-680.

**Tabu Search**:
- Glover, F. (1986). Future paths for integer programming and links to artificial intelligence. *Computers & Operations Research*, 13(5), 533-549.

---

See individual markdown files in each subdirectory for detailed algorithms and equations.