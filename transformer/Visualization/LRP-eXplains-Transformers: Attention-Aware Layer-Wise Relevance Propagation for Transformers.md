### **[LRP-eXplains-Transformers (LXT)-2024](https://github.com/rachtibat/LRP-eXplains-Transformers)**

Reduan Achtibat ら（Fraunhofer Heinrich-Hertz-Institute）によって開発された、**Transformer モデルに対する Layer-wise Relevance Propagation (LRP) による説明可能 AI ツール**（ICML 2024）。

* AttnLRP（Attention-Aware LRP）による入力トークン・内部ニューロンへの忠実なアトリビューション
* 単一の backward pass で計算可能な効率的な実装
* LLaMA 2/3、Gemma 3、Qwen 2、BERT、GPT-2、Vision Transformers など幅広いモデルに対応
* 潜在特徴のアトリビューションと可視化（ニューロン・SAE 特徴のラベリング、生成プロセスの制御）
  勾配ベース手法よりも忠実でノイズの少ない説明を提供し、Transformer 全体のブラックボックス性を低減できる。
