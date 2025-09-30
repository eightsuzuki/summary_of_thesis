# Hessian Locally Linear Embedding (Hessian LLE)

**Authors**: David L. Donoho, Carrie Grimes (2003)
- Hessian Eigenmaps: Locally Linear Embedding Techniques for High-Dimensional Data. PNAS. https://doi.org/10.1073/pnas.0930874100

---

### 1. Idea
Improve LLE by penalizing local curvature via Hessian estimates on the manifold, yielding embeddings that better preserve isometry for curved manifolds.

---

### 2. Local tangent estimation
For each point, compute neighbors and estimate a local tangent basis via PCA, yielding coordinates \(\tilde{x}\) in \(\mathbb{R}^{d_{tan}}\).

---

### 3. Discrete Hessian operator
Construct a discrete Hessian estimator \(H\) acting on functions over points using second-order finite differences in local coordinates. The embedding minimizes
\[
\sum_{i} \lVert H y \rVert^2 \;=\; y^\top L_H \, y
\]
for an operator \(L_H\) assembled from local Hessian stencils.

---

### 4. Eigenproblem
Find the bottom non-trivial eigenvectors of \(L_H\) (excluding those corresponding to affine functions), which form the low-dimensional embedding.

---

### 5. Notes
- More robust than LLE on curved manifolds; requires careful neighbor size to estimate Hessians reliably.
