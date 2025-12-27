# Gemma Scope: Open Sparse Autoencoders Everywhere All At Once on Gemma 2

**著者**: Tom Lieberum, Senthooran Rajamanoharan, Arthur Conmy, Lewis Smith, Nicolas Sonnerat, Vikrant Varma, János Kramár, Anca Dragan, Rohin Shah, Neel Nanda  
**公開**: arXiv 2024（v2: 2024-08-19）  
**DOI**: 10.48550/arXiv.2408.05147  
**リンク**: [arXiv:2408.05147](https://arxiv.org/abs/2408.05147)

本論文は、Gemma 2モデルの全層およびサブ層に対してトレーニングされた**JumpReLU Sparse Autoencoders (SAEs)**のオープンスイートである**Gemma Scope**を提案します。SAEの訓練コストが高いため、研究コミュニティ向けに包括的なSAEスイートを公開することで、より野心的な安全性と解釈可能性研究を促進することを目的としています。

---

### 1. 新規性：この論文の貢献

- **包括的なSAEスイート**: Gemma 2 2B、9B、27Bの全層・サブ層に対してJumpReLU SAEを訓練
- **オープンリリース**: 訓練済みSAEの重みを公開し、研究コミュニティの参入障壁を低減
- **評価とベンチマーク**: 各SAEの品質を標準的な指標で評価し、結果を公開
- **Instruction-tunedモデルへの拡張**: Gemma 2 9Bのinstruction-tuned版にもSAEを訓練

---

### 2. 理論/手法の核心

#### 2.1 Sparse Autoencoders (SAEs) とは

**Sparse Autoencoders (SAEs)**は、ニューラルネットワークの潜在表現を**スパースな特徴の分解**に変換する教師なし手法です。

**基本的な構造**:
- **エンコーダ**: 潜在表現 \(h \in \mathbb{R}^d\) を特徴空間 \(f \in \mathbb{R}^m\) に写像
- **デコーダ**: 特徴 \(f\) から潜在表現 \(\hat{h}\) を再構成
- **スパース性制約**: 特徴 \(f\) の多くが0になるように正則化

**目的関数**:
\[
\mathcal{L} = \|h - \hat{h}\|^2 + \lambda \|f\|_1 + \text{他の正則化項}
\]

ここで、\(\lambda\) はスパース性の強さを制御するハイパーパラメータ。

#### 2.2 JumpReLU SAE

**JumpReLU**は、ReLUの変種で、よりスパースな活性化を促進する活性化関数です。

**特徴**:
- 通常のReLUよりもスパースな表現を学習しやすい
- 特徴の解釈可能性を向上させる可能性がある

#### 2.3 Gemma Scopeの構成

**訓練対象**:
- **Gemma 2 2B**: 全層・サブ層
- **Gemma 2 9B**: 全層・サブ層（base + instruction-tuned）
- **Gemma 2 27B**: 選択された層

**各層でのSAE**:
- 各Transformer層の各サブ層（attention、FFNなど）に対して個別にSAEを訓練
- これにより、層ごとの特徴分解が可能

---

### 3. 評価と結果

**評価指標**:
- **再構成誤差**: 元の潜在表現をどれだけ正確に再構成できるか
- **スパース性**: 特徴のスパース性の程度
- **特徴の解釈可能性**: 各特徴が意味のある概念に対応しているか

**主な結果**:
- 各SAEは標準的な指標で高品質な性能を示す
- 層ごとに異なる特徴が学習される
- 公開された重みにより、研究コミュニティが容易に利用可能

---

### 4. 「キモ」と重要性

- **キモ**: 高コストなSAE訓練を一度行い、その結果をオープンに公開することで、研究コミュニティ全体の解釈可能性研究を加速させる
- **重要性**: 
  - SAEは解釈可能性研究の重要なツールだが、訓練コストが高い
  - Gemma Scopeにより、多くの研究者がSAEを使った研究を開始できる
  - 安全性研究や解釈可能性研究の進展に貢献

---

### 5. まとめ（要点）

- **Gemma Scope**: Gemma 2モデルの全層・サブ層に対して訓練されたJumpReLU SAEのオープンスイート
- **目的**: 研究コミュニティの参入障壁を低減し、解釈可能性研究を促進
- **公開**: 重みとチュートリアルが公開されている
- **評価**: 標準的な指標で高品質な性能を確認

**リソース**:
- 重みとチュートリアル: [提供URL](https://arxiv.org/abs/2408.05147)
- インタラクティブデモ: [提供URL](https://arxiv.org/abs/2408.05147)

参考: [arXiv:2408.05147](https://arxiv.org/abs/2408.05147)

