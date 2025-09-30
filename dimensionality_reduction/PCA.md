# Principal Component Analysis (PCA)

**Authors**: Karl Pearson (1901); Harold Hotelling (1933)
- Pearson: On lines and planes of closest fit to systems of points in space. Philosophical Magazine. DOI: https://doi.org/10.1080/14786440109462720
- Hotelling: Analysis of a complex of statistical variables into principal components. Journal of Educational Psychology. DOI: https://doi.org/10.1037/h0071325

---

### 1. Goal and setup
Given centered data matrix \(X \in \mathbb{R}^{n\times d}\) (rows = samples), find an orthonormal basis capturing maximal variance.

---

### 2. Core formulations
- Covariance: \(\Sigma = \tfrac{1}{n} X^\top X\).
- Eigen decomposition: \(\Sigma v_k = \lambda_k v_k\) with \(\lambda_1\ge\cdots\ge\lambda_d\). Principal components are \(v_k\), scores are \(X v_k\).
- Low-rank approximation: For \(r<d\), projection \(Z = X V_r\), reconstruction \(\hat{X}= Z V_r^\top\).
- Optimality: PCA minimizes \(\lVert X-\hat{X}\rVert_F^2\) among rank-\(r\) approximations (Eckart–Young–Mirsky).

---

### 3. SVD connection
\(X = U \Sigma_x V^\top\). Right singular vectors \(V\) are eigenvectors of \(\tfrac{1}{n}X^\top X\); singular values \(\sigma_k\) relate to eigenvalues by \(\lambda_k = \sigma_k^2/n\).

---

### 4. Whitening
Whitened components: \(Z_w = X V_r \Lambda_r^{-1/2}\), where \(\Lambda_r=\operatorname{diag}(\lambda_1,\dots,\lambda_r)\). Then \(\operatorname{Cov}(Z_w)=I\).

---

### 5. Notes
- Center data; optionally scale to unit variance for correlation-PCA.
- Explained variance ratio: \(\lambda_k / \sum_j \lambda_j\).
