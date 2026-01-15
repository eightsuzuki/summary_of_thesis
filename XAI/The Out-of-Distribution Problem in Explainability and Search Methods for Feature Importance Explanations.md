# The Out-of-Distribution Problem in Explainability and Search Methods for Feature Importance Explanations

**著者**: Peter Hase (University of North Carolina at Chapel Hill), Harry Xie (University of North Carolina at Chapel Hill), Mohit Bansal (University of North Carolina at Chapel Hill)  
**会議**: NeurIPS 2021  
**URL**: [NeurIPS 2021](https://papers.neurips.cc/paper_files/paper/2021/file/1def1713ebf17722cbe300cfc1c88558-Paper.pdf)  
**arXiv**: [arXiv:2106.03647](https://arxiv.org/abs/2106.03647)

---

### 概要

この論文は、**特徴重要度（Feature Importance, FI）説明におけるOOD（学習分布外、Out-of-Distribution）問題を分析し、3つの主要な貢献を提供**した重要な研究です。第一に、**counterfactual inputsがOODであることがsocial misalignment（社会的非整合性）を引き起こす**ことを指摘し、モデルをcounterfactual inputsで訓練することでこの問題を解決する手法を提案しました。第二に、**5つの異なるReplace関数（特徴置換関数）を体系的に比較**し、OOD counterfactualsを生成しにくい方法を推奨しました。第三に、**4つの新しいsearch-based methods（探索ベース手法）を導入**し、SufficiencyやComprehensivenessなどの評価指標に基づいて最適な説明を探索する手法を提案しました。6つの多様なテキスト分類データセットでの実験により、提案手法の有効性を実証しています。

---

### 論文の核心：3つの主要な貢献

#### 1. OOD問題とSocial Misalignment（社会的非整合性）

##### 問題の背景

FI説明の評価では、重要とされた特徴を入力から除去（ablation）し、モデルの予測への影響を測定します。しかし、このcounterfactual inputs（反事実的入力）が**OOD（学習分布外）**であることが問題となります：

- **OOD counterfactuals**: 特徴を除去した入力は、モデルが訓練時に見たことのない分布外の入力となる
- **Social misalignment**: モデルの事前分布やランダムな重み初期化が説明に意図しない影響を与える
- **説明の歪み**: 説明が実際に伝えたい情報とは異なる情報を伝えてしまう

##### 解決策：Counterfactual Training

論文では、**モデルをcounterfactual inputsで訓練する**ことで、この問題を解決します：

- **訓練時の暴露**: モデルを訓練時にcounterfactual inputsに暴露することで、テスト時にcounterfactualsがOODではなくなる
- **ロバスト性の向上**: モデルがcounterfactualsに対してよりロバストになる
- **説明指標の改善**: 説明指標（Sufficiency、Comprehensiveness）が大幅に改善される

#### 2. Replace関数の体系的比較

##### 5つのReplace関数の比較

論文では、特徴を除去する際の**Replace関数**を5つのアプローチで比較しています：

1. **Token Removal**: トークンを完全に削除（シーケンスから除去）
2. **Zero Embedding**: トークン埋め込みをゼロベクトルに置換
3. **Special Token**: 特殊トークン（例：<PAD>、<UNK>）に置換
4. **Marginalization**: 可能なcounterfactualsの予測を周辺化
5. **Attention Mask Editing**: 入力テキストではなく、attention maskを編集

##### 比較結果

- **OOD度の測定**: 各Replace関数が生成するcounterfactualsのOOD度を測定
- **推奨事項**: OOD counterfactualsを生成しにくいReplace関数を推奨
- **実践的指針**: 実践的な指針を提供

#### 3. Search-based Methods for FI Explanations

##### 4つの新しい探索手法

論文では、**SufficiencyやComprehensivenessなどの評価指標を最適化するための4つの新しいsearch-based methods**を提案しています：

1. **Random Search**: ランダムに説明を探索（ベースライン）
2. **Greedy Search**: 貪欲的に最良の特徴を選択
3. **Beam Search**: ビームサーチで探索
4. **Parallel Local Search (PLS)**: 提案する並列局所探索

##### 評価指標

- **Sufficiency**: 重要とされた特徴を保持した場合のモデル信頼度
  $$\text{Suff}(f,x,e)=f(x)_{\hat{y}}-f(\texttt{Replace}(x,e))_{\hat{y}}$$
- **Comprehensiveness**: 重要とされた特徴を除去した場合のモデル信頼度の変化

##### 実験結果

- **PLSの優位性**: Parallel Local Search (PLS)が唯一、ランダム探索を一貫して上回る
- **大幅な改善**: 2番目に良い手法に対して、Sufficiencyで最大5.4ポイント、Comprehensivenessで最大17ポイントの改善

---

### 技術的な詳細

#### 1. OOD問題とSocial Misalignment

##### Social Misalignmentの定義

Jacovi and Goldberg (2021)が導入した「social misalignment」は、**説明が人々が期待する種類の情報とは異なる種類の情報を伝える状況**を指します。本論文では、以下の問題を指摘しています：

- **モデル事前分布の影響**: モデルの事前分布がFI推定に意図しない影響を与える
- **ランダム初期化の影響**: ランダムな重み初期化が説明に影響を与える
- **期待との不一致**: FI説明が実際に伝えたい情報とは異なる情報を伝える

##### Counterfactual Trainingの実装

モデルをcounterfactual inputsで訓練する具体的な方法：

- **訓練データの拡張**: 訓練データに対してReplace関数を適用し、counterfactual inputsを生成
- **混合訓練**: 元のデータとcounterfactual inputsを混合して訓練
- **ロバスト性の向上**: モデルがcounterfactualsに対してよりロバストになる

#### 2. Replace関数の詳細な比較

##### 各Replace関数の特性

1. **Token Removal**: 
   - トークンを完全に削除し、シーケンスの長さが変わる
   - OOD度が高い可能性

2. **Zero Embedding**: 
   - 埋め込みをゼロベクトルに置換
   - OOD問題が発生しやすい

3. **Special Token**: 
   - <PAD>や<UNK>などの特殊トークンに置換
   - モデルが訓練時に見たトークンを使用

4. **Marginalization**: 
   - 可能なcounterfactualsの予測を周辺化
   - より統計的に妥当

5. **Attention Mask Editing**: 
   - 入力テキストではなく、attention maskを編集
   - 入力自体は変更しない

##### OOD度の測定方法

- **分布距離**: counterfactual inputsと訓練データの分布距離を測定
- **モデルの不確実性**: counterfactual inputsでのモデルの予測不確実性を測定
- **予測の変化**: counterfactual inputsでの予測の変化を測定

#### 3. Search-based Methodsの詳細

##### Parallel Local Search (PLS)

提案するPLSアルゴリズムの特徴：

- **並列探索**: 複数の初期解から並列に探索
- **局所最適化**: 各解の近傍で最適化
- **多様性の維持**: 探索の多様性を維持

##### 最適化問題

Sufficiencyを最大化する問題：

$$\arg\max_{e} \text{Suff}(f,x,e)$$

ここで：
- $e \in \{0,1\}^d$: $k$-sparse binary vector（説明）
- $d$: 特徴空間の次元数
- $k$: スパース性レベル

##### 計算予算の制御

各手法に対して同じ計算予算（compute budget）を使用し、公平に比較：

- **評価回数**: モデルの評価回数を制限
- **時間制限**: 計算時間を制限
- **公平な比較**: 計算予算を統一することで公平に比較

---

### 実験と評価

#### 1. 実験設定

##### データセット

論文では、**6つの多様なテキスト分類データセット**を使用しています：

- **FEVER**: Fact Extraction and VERification
- **SNLI**: Stanford Natural Language Inference
- **その他4つのデータセット**: 様々なテキスト分類タスク

##### モデル

- **Transformerモデル**: 2つのTransformerモデルを使用
- **評価指標**: SufficiencyとComprehensiveness

##### 比較手法

論文では、以下の手法と比較しています：

- **LIME**: 局所的な線形モデルで近似
- **Anchors**: 高精度なモデル非依存説明
- **Integrated Gradients**: パス属性法
- **Random Search**: ランダム探索（ベースライン）
- **提案手法**: 4つのsearch-based methods（Greedy、Beam、PLSなど）

#### 2. 主要な結果

##### Counterfactual Trainingの効果

- **ロバスト性の向上**: モデルがcounterfactualsに対してよりロバストになる
- **説明指標の大幅な改善**: SufficiencyとComprehensivenessが大幅に改善される
- **Social misalignmentの解決**: モデル事前分布やランダム初期化の影響が減少

##### Replace関数の比較結果

- **OOD度の違い**: 異なるReplace関数が異なるレベルのOOD counterfactualsを生成
- **推奨事項**: OOD counterfactualsを生成しにくいReplace関数を推奨
- **実践的指針**: 実践的な指針を提供

##### Search-based Methodsの比較結果

- **PLSの優位性**: Parallel Local Search (PLS)が唯一、ランダム探索を一貫して上回る
- **大幅な改善**: 2番目に良い手法に対して、Sufficiencyで最大5.4ポイント、Comprehensivenessで最大17ポイントの改善
- **計算効率**: 同じ計算予算でより良い結果を達成

#### 3. 実践的な示唆

##### ベースライン選択の推奨事項

- **分布内の選択**: 可能であれば、学習分布内のベースラインを選択する
- **探索の活用**: 固定ベースラインではなく、探索ベースのアプローチを検討する
- **情報量の考慮**: ベースラインの情報量を考慮する

##### 実装時の注意点

- **計算コスト**: 探索には計算コストがかかるため、効率的な実装が必要
- **分布の推定**: 学習分布を適切に推定する必要がある
- **収束の保証**: 探索アルゴリズムの収束を保証する必要がある

---

### 論文の意義と影響

#### 1. OOD問題の理論的分析

この論文は、**OOD問題を理論的に分析**し、説明可能性研究に重要な貢献をしました：

- **理論的基盤**: OOD問題の理論的基盤を提供
- **問題の明確化**: OOD問題が説明に与える影響を明確化
- **解決策の提案**: 探索ベースのアプローチを提案

#### 2. 探索ベースアプローチの提案

この論文は、**探索ベースのベースライン選択を提案**し、固定ベースラインの限界を克服しようとしました：

- **適応的な選択**: 入力に応じて適切なベースラインを選択
- **分布内の保証**: 学習分布内のベースラインを保証
- **情報量の最適化**: 情報量を最適化することで、より適切なベースラインを選択

#### 3. 説明可能性研究への影響

この論文は、説明可能性研究に以下のような影響を与えました：

- **ベースライン選択の見直し**: 固定ベースラインの見直しを促した
- **OOD問題の認識**: OOD問題の重要性を認識させた
- **新しいアプローチ**: 探索ベースのアプローチを提供

---

### 技術的な革新点

#### 1. OOD問題の理論的分析

この論文の最大の革新点は、**OOD問題を理論的に分析**したことです：

- **分布の不一致**: ベースラインと学習分布の不一致を理論的に分析
- **影響の定量化**: OOD問題が説明に与える影響を定量化
- **解決策の理論的基盤**: 探索ベースアプローチの理論的基盤を提供

#### 2. 探索ベースのベースライン選択

探索ベースのベースライン選択を提案しました：

- **適応的な選択**: 入力に応じて適切なベースラインを選択
- **分布内の保証**: 学習分布内のベースラインを保証
- **情報量の最適化**: 情報量を最適化することで、より適切なベースラインを選択

#### 3. 実装の提供

実装を提供し、再現性を確保しました：

- **アルゴリズムの詳細**: 探索アルゴリズムの詳細を記述
- **実装の公開**: 実装を公開し、再現性を確保
- **実験の詳細**: 実験の詳細を記述

---

### 限界と今後の課題

#### 1. 計算コスト

- **探索のコスト**: 探索には計算コストがかかる
- **効率化**: より効率的な探索アルゴリズムの開発
- **近似手法**: 近似手法の開発

#### 2. 分布の推定

- **密度推定**: 学習分布の密度を適切に推定する必要がある
- **高次元データ**: 高次元データでの分布推定の困難さ
- **近似手法**: 分布推定の近似手法の開発

#### 3. 評価方法

- **適切な評価指標**: 探索ベースアプローチを評価する適切な指標の開発
- **比較方法**: 他のベースライン選択手法との比較方法の確立
- **人間による評価**: 人間による評価との整合性

---

### 実践的な示唆

#### 1. ベースライン選択の推奨事項

論文の結果から、以下の推奨事項が導き出されます：

- **分布内の選択**: 可能であれば、学習分布内のベースラインを選択する
- **探索の活用**: 固定ベースラインではなく、探索ベースのアプローチを検討する
- **情報量の考慮**: ベースラインの情報量を考慮する

#### 2. 実装時の注意点

- **計算コスト**: 探索には計算コストがかかるため、効率的な実装が必要
- **分布の推定**: 学習分布を適切に推定する必要がある
- **収束の保証**: 探索アルゴリズムの収束を保証する必要がある

#### 3. 研究への応用

- **OOD問題の認識**: OOD問題の重要性を認識し、適切に対処する
- **探索ベースアプローチ**: 探索ベースのアプローチを検討する
- **評価方法の改善**: 適切な評価方法を開発する

---

### まとめ

「The Out-of-Distribution Problem in Explainability and Search Methods for Feature Importance Explanations」は、特徴重要度（FI）説明におけるOOD問題を分析し、3つの主要な貢献を提供した重要な研究です。第一に、counterfactual inputsがOODであることがsocial misalignmentを引き起こすことを指摘し、モデルをcounterfactual inputsで訓練することでこの問題を解決する手法を提案しました。第二に、5つの異なるReplace関数を体系的に比較し、OOD counterfactualsを生成しにくい方法を推奨しました。第三に、4つの新しいsearch-based methodsを導入し、SufficiencyやComprehensivenessなどの評価指標に基づいて最適な説明を探索する手法を提案しました。6つの多様なテキスト分類データセットでの実験により、Parallel Local Search (PLS)が唯一、ランダム探索を一貫して上回り、大幅な改善を示すことを実証しています。

この論文の影響は大きく、多くの研究でOOD問題が認識され、counterfactual trainingやsearch-based methodsが検討されるようになりました。今後の研究では、計算効率の向上、Replace関数のさらなる改善、評価方法の確立などが重要な課題となるでしょう。
