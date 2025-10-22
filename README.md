# Summary of Thesis Notes

本リポジトリは、論文サマリーと調査ノートをフォルダ別に整理しています。各フォルダの `README.md` に比較表や年代順の流れを掲載しています。

## ディレクトリ構成（概要）

- `XAI/`
  - 画像系・モデル非依存の説明手法のサマリー（Saliency/CAM/Grad-CAM/IG/LRP/DeepLIFT/LIME/SHAP/Influence Functions/RAP ほか）
  - 比較表と簡易年表: `XAI/README.md`
- `dimensionality_reduction/`
  - 次元削減（PCA/ICA/t-SNE/UMAP/Isomap/LLE/HLLE/Laplacian Eigenmaps/Diffusion Maps/Kernel PCA/MDS/NMF/Autoencoders/PHATE）
  - 比較表と年表: `dimensionality_reduction/README.md`
- `knowledge_graph_embedding/`
  - KGE（TransE系・Bilinear系・複素数/回転/畳み込み/階層/PLM連携・コントラスト学習など）
  - 比較表と年表: `knowledge_graph_embedding/README.md`
- `word_embedding/`
  - 単語埋め込み（Word2Vec/GloVe/FastText/Word2GM/Word2Box/ELMo）
  - 点表現・確率分布表現・幾何学的領域表現・文脈依存表現
  - 比較表と年表: `word_embedding/README.md`
- `transformer_attention/`
  - アテンション解析・回路トレース・特徴幾何・注意フロー・表現学習の可視化/帰属
  - トピック比較と流れ: `transformer_attention/README.md`
- `LLM_political_ideology/`
  - LLM の政治的傾向・バイアス・意見反映の計測と分析
  - 比較表と流れ: `LLM_political_ideology/README.md`

## 命名規則（XAI など）

- 論文サマリーのファイル名:
  - 手法名が明確な場合: `{手法名}: {論文タイトル}.md`
  - 手法名が明確でない場合: `{論文タイトル}.md`

## 使い方

- まず各フォルダの `README.md` を確認し、比較表で手法の特徴を俯瞰 → 年代順の流れで位置づけを把握。
- 個別ファイルで数式・実装ノート・参考リンク（DOI/arXiv）を確認してください。
