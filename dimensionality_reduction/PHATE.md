# PHATE: Potential of Heat-diffusion for Affinity-based Transition Embedding

**Authors**: Kevin R. Moon, David van Dijk, Zhicheng Wang, et al. (2019)
- Visualizing structure and transitions in high-dimensional biological data. Nature Biotechnology. https://doi.org/10.1038/s41587-019-0336-3

---

### 1. Idea
Construct a diffusion operator and take its information-geometric potential (log-scale of transition probabilities) to obtain an embedding that preserves both local and global transitions.

---

### 2. Diffusion and potential
Build kNN graph and compute diffusion operator \(P\). Take \(t\)-step transitions \(P^t\). Define diffusion potential
\[
U_t = -\log(P^t)
\]
Row-wise distances in \(U_t\) reflect manifold transitions while de-emphasizing density effects.

---

### 3. Embedding
Apply metric MDS (e.g., classical MDS on pairwise distances from \(U_t\)) to obtain low-dimensional coordinates.

---

### 4. Notes
- Parameters: k (neighbors), \(t\) (diffusion time), potential smoothing.
- Effective for continuous trajectories (e.g., developmental biology) and branching structures.
