# Multidimensional Scaling (MDS)

**Author**: Joseph B. Kruskal (1964)
- Nonmetric multidimensional scaling: a numerical method. Psychometrika. https://doi.org/10.1007/BF02289694

---

### 1. Idea
Given pairwise dissimilarities \(\{\delta_{ij}\}\), find points in low dimension whose interpoint distances \(d_{ij}(Y)=\lVert y_i-y_j\rVert\) preserve these dissimilarities (metric MDS) or their rank order (nonmetric MDS).

---

### 2. Metric/classical MDS
With Euclidean distances, double-center \(B=-\tfrac{1}{2} H D^{\circ 2} H\) and eigendecompose to obtain coordinates \(Y=V_r \Lambda_r^{1/2}\).

---

### 3. Stress minimization (Kruskal)
Minimize stress
\[
\text{Stress}(Y) = \sqrt{\frac{\sum_{i<j} (f(\delta_{ij}) - d_{ij}(Y))^2}{\sum_{i<j} d_{ij}(Y)^2}}
\]
where \(f\) is a monotone transformation (nonmetric). Optimize via SMACOF (majorization).

---

### 4. Notes
- Choice of dissimilarity scale affects embedding.
- Nonmetric MDS is robust to monotone distortions.
