# Transformer Attention & Interpretability

This directory collects summaries on attention mechanisms and interpretability in transformers (attribution, circuits, attention flow, feature geometry, etc.).

## Quick comparison table (topics)

| Topic | Core idea | Methods/papers | Notes |
| --- | --- | --- | --- |
| Attention attribution | Attribute outputs to tokens/heads | Quantifying Attention Flow; AttnLRP | Causality vs correlation |
| Integrated gradients in transformers | Path-integral attributions | A Rigorous Study of Integrated Gradients | Internal neuron attributions |
| Feature geometry | Linear vs nonlinear features | Not All LM Features Are Linear; ICA in embeddings | Emergent structure |
| Circuit analysis | Tracing computation | Circuit Tracing; Transformer Interpretability Beyond Attention | Mechanistic interp. |
| Model maps | Landscape of LMs | Mapping 1,000+ LMs via Log-Likelihood | Taxonomy |
| In-context learning | Representation formation | ICLR In-Context Learning of Representations | Dynamics of ICL |

## Chronology (illustrative)

- 2017–2019: Early attention interpretability; attribution baselines adapted to transformers.
- 2020–2022: Circuit analysis and attention flow methods; probing linearity of features; ICA-based views.
- 2023–2025: Scaling studies and model maps; refined attributions and mechanistic tracing.

See individual markdowns for equations and experimental details.
