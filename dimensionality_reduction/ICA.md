# Independent Component Analysis (ICA)

**Authors**: Pierre Comon (1994); Aapo Hyvärinen (1999)
- Comon: Independent component analysis, a new concept? Signal Processing. DOI: https://doi.org/10.1016/0165-1684(94)90029-9
- Hyvärinen: Fast and robust fixed-point algorithms for ICA (FastICA). IEEE TNN. DOI: https://doi.org/10.1109/72.761722

---

### 1. Model
Observed \(x \in \mathbb{R}^d\) is a linear mixture of independent sources \(s\): \(x = A s\). Goal: estimate unmixing \(W\) such that \(\hat{s}=Wx\) have maximally independent components.

---

### 2. Preprocessing
Center and whiten: \(x_w = V x\) with \(\operatorname{Cov}(x_w)=I\). Then effective mixing is orthogonal.

---

### 3. Estimation criteria
- Maximize non-Gaussianity (kurtosis or negentropy): \(J(y)=H(y_{gauss})-H(y)\).
- Maximum likelihood under super-/sub-Gaussian priors.

---

### 4. FastICA fixed-point
For one component (unit-norm \(w\)):
\[
 w \leftarrow \mathbb{E}[x_w g(w^\top x_w)] - \mathbb{E}[g'(w^\top x_w)] w;\quad w \leftarrow \frac{w}{\lVert w\rVert}
\]
with nonlinearity \(g(u)=\tanh(u)\), etc. Decorrelate multiple components via symmetric orthogonalization.

---

### 5. Notes
- ICA identifies components up to permutation and scaling.
- Whitening is essential for stable estimation.
