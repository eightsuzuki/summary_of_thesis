# t-SNE: t-distributed Stochastic Neighbor Embedding

**Authors**: Laurens van der Maaten, Geoffrey Hinton (2008)
- Visualizing Data using t-SNE. JMLR. http://www.jmlr.org/papers/v9/vandermaaten08a.html

---

### 1. Idea
Match high-dimensional neighbor probabilities with low-dimensional ones using KL divergence; heavy-tailed Student-t in low-dim to avoid crowding.

---

### 2. High-dimensional similarities
Conditional probabilities:
\[
 p_{j|i} = \frac{\exp(-\lVert x_i-x_j\rVert^2 / 2\sigma_i^2)}{\sum_{k\neq i} \exp(-\lVert x_i-x_k\rVert^2 / 2\sigma_i^2)},\quad P_{ij}=\frac{p_{j|i}+p_{i|j}}{2n}
\]
Perplexity controls \(\sigma_i\).

---

### 3. Low-dimensional similarities
Student-t with 1 d.o.f.:
\[
 q_{ij} = \frac{(1+\lVert y_i-y_j\rVert^2)^{-1}}{\sum_{k\neq l}(1+\lVert y_k-y_l\rVert^2)^{-1}}
\]

---

### 4. Objective and gradients
Minimize KL divergence: \(\mathrm{KL}(P\,\Vert\,Q)=\sum_{i,j} P_{ij}\log\frac{P_{ij}}{Q_{ij}}\).
Gradient:
\[
 \frac{\partial C}{\partial y_i} = 4 \sum_j (P_{ij}-Q_{ij}) (y_i-y_j) (1+\lVert y_i-y_j\rVert^2)^{-1}
\]

---

### 5. Notes
- Use early exaggeration on \(P\), momentum, and learning-rate tuning.
- Sensitive to perplexity; not faithful for global geometry.
