# Isomap

**Authors**: Joshua B. Tenenbaum, Vin de Silva, John C. Langford (2000)
- A Global Geometric Framework for Nonlinear Dimensionality Reduction. Science. https://doi.org/10.1126/science.290.5500.2319

---

### 1. Idea
Approximate geodesic distances on a manifold via kNN/\(\epsilon\)-graph shortest paths; embed by classical MDS on the geodesic distance matrix.

---

### 2. Steps
1) Build neighborhood graph \(G\) with edge weights \(d(x_i,x_j)\).
2) Compute geodesic distances \(D_{ij}\) via all-pairs shortest paths (Dijkstra/Floyd–Warshall).
3) Apply classical MDS: Double-center \(B=-\tfrac{1}{2} H D^{\circ 2} H\), eigendecompose \(B=V\Lambda V^\top\), embedding \(Y=V_r \Lambda_r^{1/2}\).

---

### 3. Notes
- Sensitive to graph connectivity; use sufficient neighbors to avoid disconnects.
- Preserves global geodesic structure; can fail with short-circuit edges.
