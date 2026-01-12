# Transformer

This directory collects summaries on transformers, including attention mechanisms, interpretability, visualization tools, positional encoding, and related topics.

## Directory Structure

- **Visualization/**: Tools and frameworks for visualizing transformer models (bertviz, ecco, TransformerLens, etc.)
- **Interpretability/**: Methods and papers on transformer interpretability (AttnLRP, Circuit Tracing, etc.)
- **Feature Analysis/**: Analysis of features and representations in transformers
- **In-Context Learning/**: Research on in-context learning in transformers
- **Model Analysis/**: Analysis of transformer models at scale
- **Positional Encoding/**: Various positional encoding methods
- **Political Ideology/**: Research on political biases and ideology in language models

## Quick comparison table (topics)

| Topic | Core idea | Methods/papers | Notes |
| --- | --- | --- | --- |
| Attention attribution | Attribute outputs to tokens/heads | Quantifying Attention Flow; AttnLRP | Causality vs correlation |
| Integrated gradients in transformers | Path-integral attributions | A Rigorous Study of Integrated Gradients | Internal neuron attributions |
| Feature geometry | Linear vs nonlinear features | Not All LM Features Are Linear; ICA in embeddings | Emergent structure |
| Circuit analysis | Tracing computation | Circuit Tracing; Transformer Interpretability Beyond Attention | Mechanistic interp. |
| Model maps | Landscape of LMs | Mapping 1,000+ LMs via Log-Likelihood | Taxonomy |
| In-context learning | Representation formation | ICLR In-Context Learning of Representations | Dynamics of ICL |
| Brain-LLM alignment | Computational path similarity | Scaling and context steer LLMs along the same computational path as the human brain | Layer depth ↔ temporal brain response |

## Chronology (illustrative)

- 2017–2019: Early attention interpretability; attribution baselines adapted to transformers.
- 2020–2022: Circuit analysis and attention flow methods; probing linearity of features; ICA-based views.
- 2023–2025: Scaling studies and model maps; refined attributions and mechanistic tracing; brain-LLM computational path alignment.

See individual markdowns for equations and experimental details.
