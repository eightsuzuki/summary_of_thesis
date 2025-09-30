# Diffusion Maps

**Authors**: Ronald R. Coifman, Stéphane Lafon (2006)
- Diffusion Maps. PNAS. https://doi.org/10.1073/pnas.0500334103

---

### 1. Idea
Define a Markov diffusion process on the data graph; use eigenvectors of the diffusion operator to embed data so that diffusion distances are preserved.

---

### 2. Diffusion operator
Affinity \(k_{ij}=\exp(-\lVert x_i-x_j\rVert^2/\epsilon)\). Row-normalize to obtain Markov matrix \(P=D^{-1}K\) with \(D_{ii}=\sum_j k_{ij}\). Multi-step transitions \(P^t\).

---

### 3. Diffusion distance
\[
D_t^2(i,j) = \sum_{k} \frac{(P^t_{ik}-P^t_{jk})^2}{\phi_0(k)}
\]
where \(\phi_0\) is the stationary distribution. Spectral decomposition \(P \psi_l = \lambda_l \psi_l\) yields embedding
\[
\Psi_t(i) = (\lambda_1^t \psi_1(i),\, \lambda_2^t \psi_2(i),\, \dots)
\]
so that Euclidean distance in \(\Psi_t\) approximates \(D_t\).

---

### 4. Notes
- Parameter \(t\) controls scale (coarse-to-fine geometry).
- Robust to noise; related to Laplacian Eigenmaps via normalization choices.
