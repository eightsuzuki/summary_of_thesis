# Explainable AI (XAI)

This directory collects summaries of explanation methods for machine learning and deep learning models (vision- and model-agnostic methods).

## Quick comparison table

| Method | Family | Input needed | Faithfulness target | Pros | Cons |
| --- | --- | --- | --- | --- | --- |
| Saliency (Simonyan & Zisserman) | Gradient | Gradients | Local sensitivity | Simple baseline; fast | Noisy; saturation issues |
| DeconvNet (Zeiler & Fergus) | Inversion/visualization | Switches, conv weights | Mid-level features | Interprets conv features | Not class-discriminative by default |
| Guided Backprop | Gradient variant | Gradients | Local sensitivity | Sharper maps | Not strictly faithful |
| Grad-CAM | Gradient × feature | Gradients at last conv | Localization | Class-discriminative regions | Coarse; depends on conv layers |
| LRP | Relevance propagation | Model internals | Conservation of relevance | Completeness; less noisy | Rule choices; impl. variance |
| Deep Taylor Decomposition | Taylor expansion | Model internals | Conservation via Taylor | Theoretical foundation for LRP | Reference point choice; local approximation |
| DeepLIFT | Reference-based propagation | Reference input | Completeness on deltas | Handles saturation; fast | Baseline choice sensitive |
| IG (Integrated Gradients) | Path integral | Gradients + baseline | Implementation invariance | Axiomatic guarantees | Baseline, path choice |
| LIME | Local surrogate | Black-box access | Local fidelity (weighted) | Model-agnostic; flexible | Sampling/instability |
| LORE | Local rule-based | Black-box access | Local fidelity (rule-based) | Decision + counterfactual rules; GA-based sampling | GA parameters; rule extraction complexity |
| SHAP | Additive Shapley | Black-box or model-specific | Shapley axioms | Unified theory; TreeSHAP exact | Costly; background choice |
| Influence Functions | Data influence | Grad/HVP access | Param/data sensitivity | Points to causal training data | H^{-1} approx; assumptions |
| RAP | Relevance (bi-polar) | Model internals | Relative priority | Clear separation of (ir)relevance | Hyperparameters; variants |
| Gemma Scope (SAEs) | Sparse decomposition | Model activations | Sparse feature decomposition | Interpretable features; open weights | Training cost; feature interpretability |
| Attribution Graphs | Graph-based attribution | Model internals | Information flow | Visualizes internal flow | Computational cost; interpretation |

## Chronology and influences

- 2013: DeconvNet (Zeiler & Fergus) – feature visualization; saliency groundwork.
- 2013/2014: Saliency maps (Simonyan & Zisserman) – gradient-based class saliency; weakly supervised localization.
- 2015: LRP – layer-wise relevance with conservation.
- 2018: Deep Taylor Decomposition – theoretical foundation for LRP via Taylor expansion.
- 2016/2017: IG (axiomatic path), DeepLIFT (reference deltas), LIME (local surrogates).
- 2017: SHAP – additive Shapley unification; Tree/Kernel/Deep variants.
- 2018: LORE – local rule-based explanations with decision and counterfactual rules; genetic algorithm for neighborhood generation.
- 2016–2019: Grad-CAM series – class-discriminative localization in CNNs.
- 2017: Influence Functions – training-point responsibility analysis.
- 2019/2020: RAP – relative (bi-polar) relevance propagation.
- 2024: Gemma Scope – open sparse autoencoders for interpretability research.
- 2025: Attribution Graphs – graph-based visualization of information flow in Transformers.

See individual markdown files in this folder for equations and details.
