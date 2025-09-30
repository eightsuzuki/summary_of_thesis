# Dimensionality Reduction

This directory collects summaries and notes on dimensionality reduction methods (e.g., PCA, t-SNE, UMAP, ICA, Isomap).

- Add method-specific markdown files here, following the existing summary style in this repository.

## Methods and seminal references

- PCA — Pearson (1901); Hotelling (1933)
  - Pearson: On lines and planes of closest fit to systems of points in space. Philosophical Magazine. [DOI](https://doi.org/10.1080/14786440109462720)
  - Hotelling: Analysis of a complex of statistical variables into principal components. Journal of Educational Psychology. [DOI (Part I)](https://doi.org/10.1037/h0071325), [DOI (Part II)](https://doi.org/10.1037/h0070888)
- ICA — Comon (1994); Hyvärinen (1999)
  - Comon: Independent component analysis, a new concept? Signal Processing. [DOI](https://doi.org/10.1016/0165-1684(94)90029-9)
  - Hyvärinen: Fast and robust fixed-point algorithms for independent component analysis. IEEE TNN. [DOI](https://doi.org/10.1109/72.761722)
- t-SNE — van der Maaten & Hinton (2008)
  - Visualizing Data using t-SNE. JMLR. [Link](http://www.jmlr.org/papers/v9/vandermaaten08a.html)
- UMAP — McInnes, Healy, Melville (2018)
  - UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction. arXiv:1802.03426. [arXiv](https://arxiv.org/abs/1802.03426)
- Isomap — Tenenbaum, de Silva, Langford (2000)
  - A Global Geometric Framework for Nonlinear Dimensionality Reduction. Science. [DOI](https://doi.org/10.1126/science.290.5500.2319)
- LLE — Roweis & Saul (2000)
  - Nonlinear Dimensionality Reduction by Locally Linear Embedding. Science. [DOI](https://doi.org/10.1126/science.290.5500.2323)
- Hessian LLE — Donoho & Grimes (2003)
  - Hessian Eigenmaps: Locally Linear Embedding Techniques for High-Dimensional Data. PNAS. [DOI](https://doi.org/10.1073/pnas.0930874100)
- Laplacian Eigenmaps — Belkin & Niyogi (2003)
  - Laplacian Eigenmaps for Dimensionality Reduction and Data Representation. Neural Computation. [DOI](https://doi.org/10.1162/089976603321780317)
- Diffusion Maps — Coifman & Lafon (2006)
  - Diffusion Maps. PNAS. [DOI](https://doi.org/10.1073/pnas.0500334103)
- Kernel PCA — Schölkopf, Smola, Müller (1998)
  - Nonlinear Component Analysis as a Kernel Eigenvalue Problem. Neural Computation. [DOI](https://doi.org/10.1162/089976698300017467)
- MDS — Kruskal (1964)
  - Nonmetric multidimensional scaling: a numerical method. Psychometrika. [DOI](https://doi.org/10.1007/BF02289694)
- NMF — Lee & Seung (1999)
  - Learning the parts of objects by non-negative matrix factorization. Nature. [DOI](https://doi.org/10.1038/44565)
- Autoencoders (nonlinear DR) — Hinton & Salakhutdinov (2006)
  - Reducing the Dimensionality of Data with Neural Networks. Science. [DOI](https://doi.org/10.1126/science.1127647)
- PHATE — Moon, van Dijk, Wang et al. (2019)
  - Visualizing structure and transitions in high-dimensional biological data. Nature Biotechnology. [DOI](https://doi.org/10.1038/s41587-019-0336-3)

## Quick comparison table

| Method | Linear/Nonlinear | Class | Preserves | Strengths | Limitations | Year |
| --- | --- | --- | --- | --- | --- | --- |
| PCA | Linear | Spectral (SVD) | Global variance | Fast, interpretable, whitening | Only linear structure | 1901/1933 |
| ICA | Linear mixing | Probabilistic/signal | Statistical independence | Blind source separation | Scale/permutation ambiguity; not manifold | 1994/1999 |
| MDS (classical) | Linear on distances | Spectral (double-centering) | Pairwise distances | Simple geometry from distances | Sensitive to distortions/outliers | 1964 |
| Kernel PCA | Nonlinear | Kernel eigen | Global variance in feature space | Captures nonlinear structure | Kernel choice; pre-image problem | 1998 |
| NMF | Nonlinear (constraints) | Matrix factorization | Parts-based structure | Additive, sparse parts | Nonconvex; scaling | 1999 |
| Isomap | Nonlinear | Graph + MDS | Global geodesics | Global manifold unfolding | Short-circuits; connectivity | 2000 |
| LLE | Nonlinear | Graph weights + eig | Local linearity | Simple, local preservation | Sensitive to K; curvature issues | 2000 |
| Hessian LLE | Nonlinear | Hessian-based | Local isometry (curvature) | Better on curved manifolds | Parameter sensitive | 2003 |
| Laplacian Eigenmaps | Nonlinear | Spectral graph | Local neighborhoods | Spectral foundation; clustering link | Graph params critical | 2003 |
| Diffusion Maps | Nonlinear | Markov diffusion | Diffusion distance (multi-scale) | Noise-robust; scale control | Kernel/epsilon choice | 2006 |
| Autoencoders | Nonlinear | Neural | Task-driven reconstruction | Flexible, scalable | Training/regularization sensitive | 2006 |
| t-SNE | Nonlinear | Probabilistic neighbor | Local neighborhoods (visual) | Cluster visualization | Poor global geometry | 2008 |
| UMAP | Nonlinear | Fuzzy simplicial set | Local+some global | Fast, preserves topology | Params tuning; theory assumptions | 2018 |
| PHATE | Nonlinear | Diffusion + potential | Transitions/trajectories | Branching/continuum structure | Parameter t/k choice | 2019 |

## Chronological development and influences

- 1901/1933: PCA (Pearson; Hotelling). Foundational linear variance maximization; basis for whitening and initialization across methods.
- 1964: MDS (Kruskal). Distance-preserving embeddings; classical MDS later used inside Isomap.
- 1994/1999: ICA (Comon; Hyvärinen). Independent components for source separation; complementary to PCA (second-order vs higher-order statistics).
- 1998: Kernel PCA (Schölkopf et al.). Introduces kernel trick to PCA; influences many kernel spectral methods.
- 1999: NMF (Lee & Seung). Parts-based factorization; later tied to sparse coding and topic models.
- 2000: Isomap (Tenenbaum et al.) builds on MDS with geodesic distances; LLE (Roweis & Saul) uses local linear reconstructions.
- 2003: Laplacian Eigenmaps (Belkin & Niyogi) formalize spectral graph embeddings; Hessian LLE (Donoho & Grimes) refines LLE by penalizing curvature.
- 2006: Diffusion Maps (Coifman & Lafon) introduce diffusion distances (Markov processes); Autoencoders (Hinton & Salakhutdinov) revive neural DR with nonlinear bottlenecks.
- 2008: t-SNE (van der Maaten & Hinton) improves SNE with Student-t in low-dim, focusing on neighborhood visualization.
- 2018: UMAP (McInnes et al.) connects Riemannian geometry and fuzzy simplicial sets; conceptually relates to spectral/diffusion methods and aims to improve t-SNE trade-offs.
- 2019: PHATE (Moon et al.) builds on diffusion processes with information-geometric potentials to capture both local and transitional structures.

Notes on influences:
- Spectral lineage: PCA → Kernel PCA; MDS → Isomap; Graph Laplacians → Laplacian Eigenmaps → Diffusion Maps (normalization variants).
- Local linear lineage: LLE → Hessian LLE; both inform manifold learning emphasizing locality.
- Probabilistic neighbor lineage: SNE/t-SNE → inspired UMAP’s focus on neighborhood preservation (with different mathematical framing).
- Diffusion lineage: Diffusion Maps → PHATE (via diffusion potentials), related in spirit to UMAP’s probabilistic graphs.
