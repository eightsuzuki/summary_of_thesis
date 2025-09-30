# Kernel PCA

**Authors**: Bernhard Schölkopf, Alexander Smola, Klaus-Robert Müller (1998)
- Nonlinear Component Analysis as a Kernel Eigenvalue Problem. Neural Computation. https://doi.org/10.1162/089976698300017467

---

### 1. Idea
Perform PCA in an implicit feature space via the kernel trick; eigen-decompose the centered kernel matrix to obtain nonlinear principal components.

---

### 2. Kernel matrix and centering
Kernel \(k(x_i,x_j)=\langle \phi(x_i),\phi(x_j)\rangle\). Centered kernel:
\[
K_c = K - \mathbf{1} K - K \mathbf{1} + \mathbf{1} K \mathbf{1},\quad \mathbf{1}=\tfrac{1}{n}\mathbf{e}\mathbf{e}^\top
\]

---

### 3. Eigenproblem
Solve \(K_c \alpha = n \lambda \alpha\). The projection of a new sample \(x\) onto component \(\ell\) is
\[
 z_\ell(x) = \sum_{i=1}^n \alpha_{\ell i} \, \tilde{k}(x_i,x)
\]
where \(\tilde{k}\) is kernel centered with respect to training data.

---

### 4. Notes
- Common kernels: RBF, polynomial, sigmoid.
- Pre-image problem: approximate inverse mapping to input space is nontrivial.
