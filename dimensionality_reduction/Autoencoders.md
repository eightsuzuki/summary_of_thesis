# Autoencoders for Nonlinear Dimensionality Reduction

**Authors**: Geoffrey E. Hinton, Ruslan R. Salakhutdinov (2006)
- Reducing the Dimensionality of Data with Neural Networks. Science. https://doi.org/10.1126/science.1127647

---

### 1. Idea
Train a neural network to reconstruct inputs through a low-dimensional bottleneck; the bottleneck activations provide a nonlinear embedding.

---

### 2. Model
Encoder \(z = f_\theta(x)\in\mathbb{R}^{d'}\), decoder \(\hat{x}=g_\phi(z)\). Loss: \(\min_{\theta,\phi} \sum_i \ell(x_i, g_\phi(f_\theta(x_i)))\) with \(\ell\) e.g., MSE or cross-entropy.

---

### 3. Variants
- Denoising AE: corrupt \(x\) to \(\tilde{x}\), reconstruct \(x\) to learn robust manifolds.
- Sparse AE: sparsity penalty on \(z\).
- Variational AE: probabilistic latent \(z\sim q_\phi(z|x)\), optimize ELBO.

---

### 4. Notes
- Nonlinear generalization of PCA; regularization shapes the manifold.
- Use tied weights, batch norm, modern optimizers for stability.
