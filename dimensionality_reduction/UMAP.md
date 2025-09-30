# UMAP: Uniform Manifold Approximation and Projection

**Authors**: Leland McInnes, John Healy, James Melville (2018)
- UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction. arXiv:1802.03426. https://arxiv.org/abs/1802.03426

---

### 1. Idea
Construct a weighted kNN graph as a fuzzy simplicial set in high-dim; optimize a low-dim embedding by minimizing cross-entropy between high/low fuzzy sets.

---

### 2. High-dimensional graph
For each point \(i\), set local connectivity scale \(\sigma_i\) and define directed weights
\[
 w_{ij} = \exp\!\left(-\max(0, \tfrac{d(x_i,x_j)-\rho_i}{\sigma_i})\right)
\]
then symmetrize: \(w^{(sym)}_{ij}=w_{ij}+w_{ji}-w_{ij}w_{ji}\).

---

### 3. Low-dimensional graph and loss
Low-dim probability: \(\hat{w}_{ij} = (1+a\lVert y_i-y_j\rVert^{2b})^{-1}\) with fixed \(a,b\).
Cross-entropy loss:
\[
 L = \sum_{i<j} \Big[ w^{(sym)}_{ij} \log \hat{w}_{ij} + (1-w^{(sym)}_{ij}) \log (1-\hat{w}_{ij}) \Big]
\]
Optimize via SGD with negative sampling.

---

### 4. Notes
- Parameters: n_neighbors (local vs global), min_dist (cluster tightness).
- Faster and more structure-preserving globally than t-SNE in many tasks.
