# Locally Linear Embedding (LLE)

**Authors**: Sam T. Roweis, Lawrence K. Saul (2000)
- Nonlinear Dimensionality Reduction by Locally Linear Embedding. Science. https://doi.org/10.1126/science.290.5500.2323

---

### 1. Idea
Assume each data point lies on a manifold and can be reconstructed from its neighbors with linear weights that are invariant under the embedding. Find weights in high-dim, then find low-dim coordinates preserving those weights.

---

### 2. Neighbor reconstruction weights
For each \(i\), choose neighbor set \(\mathcal{N}(i)\) of size \(K\). Solve
\[
\min_{\{w_{ij}\}} \left\lVert x_i - \sum_{j\in \mathcal{N}(i)} w_{ij} x_j \right\rVert^2 \quad \text{s.t. } \sum_{j\in \mathcal{N}(i)} w_{ij}=1,\; w_{ij}=0 \text{ if } j\notin\mathcal{N}(i)
\]
Closed form via local covariance \(C\) with Tikhonov regularization if needed.

---

### 3. Low-dimensional embedding
Find \(Y\in\mathbb{R}^{n\times d'}\) minimizing
\[
\Phi(Y) = \sum_i \left\lVert y_i - \sum_{j} w_{ij} y_j \right\rVert^2 = \operatorname{tr}(Y^\top M Y),\quad M = (I-W)^\top (I-W)
\]
Subject to centering and scale constraints (e.g., \(Y^\top \mathbf{1}=0\), \(\tfrac{1}{n}Y^\top Y = I\)). Solution: bottom \(d'+1\) eigenvectors of \(M\), discard the smallest (zero) eigenvector.

---

### 4. Notes
- Sensitive to neighborhood selection; use K close neighbors on the manifold (beware of short-circuits).
- Non-convex manifolds can cause failures; Hessian LLE improves curvature handling.
