# Knowledge Graph Embedding (KGE)

This directory collects summaries of KGE methods for link prediction and reasoning.

## Quick comparison table

| Method | Scoring function | Relation modeling | Pros | Cons |
| --- | --- | --- | --- | --- |
| TransE | \(-\lVert h + r - t \rVert\) | Translations (1-to-1) | Simple; fast | Struggles with 1-to-N/N-to-1; symmetric |
| TransH | \(-\lVert P_r h + r - P_r t \rVert\) | Relation-specific hyperplanes | Handles multi-mapping better | Extra params per relation |
| TransR | \(-\lVert M_r h + r - M_r t \rVert\) | Relation spaces | More expressive | Heavier training |
| TransD | Dynamic mapping | Entity–relation interaction | Fewer params than TransR | Complexity trade-offs |
| DistMult | \(\langle h, r, t \rangle\) | Bilinear (symmetric) | Efficient | Cannot model antisymmetry |
| ComplEx | Re(\(\langle h, r, \bar{t} \rangle\)) | Complex bilinear | Antisymmetry capable | Complex-valued ops |
| RotatE | \(-\lVert h \circ r - t \rVert\) | Rotations in C | Inversion, composition | Phase constraints |
| ConvE | \(f(vec(\text{conv}(h,r)) W)\) | Convolutional | Nonlinear interactions | Architecture tuning |
| ConvKB | Concat+conv | Convolution on [h;r;t] | Simplicity | Limited relation algebra |
| HAKE | Modulus/phase | Hierarchical | Hierarchy modeling | Specialized design |
| KG-BERT | BERT scoring | Textual encoding | Rich semantics | Expensive; scalability |
| SimKGC | Contrastive | Dense retrieval | Scalable training | Needs negatives |
| Poincaré | Hyperbolic | Hierarchical | Efficient hierarchy | Less relational algebra |
| KERMIT | Morphisms | Logical morphisms | Structure-aware | Model complexity |

## Chronology (high-level)

- 2013–2015: Translation/bilinear (TransE/H/R/D; DistMult; ComplEx).
- 2018: Convolutional (ConvE/ConvKB), rotational (RotatE), hyperbolic (Poincaré) advances; hierarchy-aware (HAKE).
- 2019–2021: PLM-based scoring (KG-BERT), contrastive retrieval (SimKGC), structure-aware (KERMIT).

See per-method markdowns in this folder for equations and details.
