# SimKGC: Simple Contrastive Knowledge Graph Completion with Pre-trained Language Models

**著者**: Liang Wang (Microsoft Research), Wei Zhao (Microsoft Research), Zhuoyu Wei (Microsoft Research), Jingming Liu (Microsoft Research)  
**Submitted**: 4 Mar 2022  
**arXiv**: [arXiv:2203.02167](https://arxiv.org/abs/2203.02167)



この論文「SimKGC: Simple Contrastive Knowledge Graph Completion with Pre-trained Language Models」では、知識グラフの完成（KGC）を目指すために、事前学習済み言語モデルとコントラスト学習を組み合わせたSimKGCという新しい手法を提案しています。以下はその概要と理論について説明します。

### 概要

知識グラフは、質問応答システムやレコメンドシステムなどで重要な役割を果たしますが、不完全であることが多いため、KGCの技術が必要です。既存のKGC手法には埋め込みベースとテキストベースの方法があり、埋め込みベースの方法（TransE、RotatEなど）が一般的に高性能ですが、SimKGCでは効率的なコントラスト学習を通じてテキストベースの方法の性能を向上させています。

### SimKGCのアプローチ

SimKGCは、以下の3つのネガティブサンプルを使用することで、モデルの効率を向上させています。

1. **インバッチネガティブ**：同じバッチ内の他のエンティティをネガティブサンプルとして利用。
2. **プリバッチネガティブ**：前のバッチで使用したエンティティをキャッシュし、現在のバッチでネガティブサンプルとして再利用。
3. **セルフネガティブ**：ハードネガティブとして、入力エンティティ自体をネガティブサンプルとして使用。

さらに、モデルの学習を効率化するため、損失関数としてInfoNCE（Information Noise Contrastive Estimation）を使用しています。これにより、ハードネガティブにフォーカスしやすくなり、トリプルの予測性能が向上します。

### 数式

SimKGCの計算では、関係エンティティの埋め込みを取得するために、以下のようなコサイン類似度が用いられます：
$$
\text{cos}(e_{hr}, e_t) = e_{hr} \cdot e_t
$$
ここで、$e_{hr}$は関係を考慮したヘッドエンティティの埋め込み、$e_t$はテイルエンティティの埋め込みです。