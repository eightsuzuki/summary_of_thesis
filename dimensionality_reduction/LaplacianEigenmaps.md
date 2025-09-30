# Laplacian Eigenmaps

**Authors**: Mikhail Belkin, Partha Niyogi (2003)
- Laplacian Eigenmaps for Dimensionality Reduction and Data Representation. Neural Computation. https://doi.org/10.1162/089976603321780317

---

### 1. Idea
Construct a weighted graph over data and use the graph Laplacian eigenvectors to obtain a low-dimensional embedding that preserves local neighborhood relationships.

---

### 2. Graph construction
kNN graph with weights, e.g., heat kernel \(W_{ij}=\exp(-\lVert x_i-x_j\rVert^2/\epsilon)\) if \(i\sim j\), else 0. Degree \(D_{ii}=\sum_j W_{ij}\), Laplacian \(L=D-W\).

---

### 3. Optimization
Minimize
\[
\sum_{i,j} \lVert y_i - y_j \rVert^2 W_{ij} = \operatorname{tr}(Y^\top L Y)
\]
subject to \(Y^\top D Y = I\) and \(Y^\top D \mathbf{1} = 0\). Solution: generalized eigenproblem \(L v = \lambda D v\); take the bottom nontrivial \(d'\) eigenvectors for embedding.

---

### 4. Notes
- Closely related to spectral clustering (normalized cuts).
- Sensitive to graph parameters (k, \(\epsilon\)).
