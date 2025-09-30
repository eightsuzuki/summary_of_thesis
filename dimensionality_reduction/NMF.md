# Non-negative Matrix Factorization (NMF)

**Authors**: Daniel D. Lee, H. Sebastian Seung (1999)
- Learning the parts of objects by non-negative matrix factorization. Nature. https://doi.org/10.1038/44565

---

### 1. Idea
Factorize a nonnegative data matrix \(X\in\mathbb{R}_{\ge 0}^{m\times n}\) into \(W\in\mathbb{R}_{\ge 0}^{m\times r}\), \(H\in\mathbb{R}_{\ge 0}^{r\times n}\) with \(X\approx WH\), yielding additive parts-based representations.

---

### 2. Objectives
Frobenius: \(\min_{W,H\ge 0} \lVert X-WH\rVert_F^2\)
KL-divergence: \(\min_{W,H\ge 0} \sum_{ij} X_{ij}\log\frac{X_{ij}}{(WH)_{ij}} - X_{ij} + (WH)_{ij}\)

---

### 3. Multiplicative updates (Lee & Seung)
For Frobenius objective:
\[
H \leftarrow H \odot \frac{W^\top X}{W^\top W H},\qquad W \leftarrow W \odot \frac{X H^\top}{W H H^\top}
\]
For KL objective:
\[
H \leftarrow H \odot \frac{W^\top (X\oslash (WH))}{\mathbf{1} W^\top},\qquad W \leftarrow W \odot \frac{(X\oslash (WH)) H^\top}{W \mathbf{1}}
\]
with elementwise operations and small epsilons for stability.

---

### 4. Notes
- Nonconvex; use multiple inits; add sparsity/regularization if needed.
- Useful for topic models, image parts, gene expression.
