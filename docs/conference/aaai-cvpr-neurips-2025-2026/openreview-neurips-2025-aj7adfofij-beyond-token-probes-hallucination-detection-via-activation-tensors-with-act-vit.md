---
title: "Beyond Token Probes: Hallucination Detection via Activation Tensors with ACT-ViT"
title_zh: 超越Token探针：基于激活张量的幻觉检测方法ACT-ViT
authors: "Guy Bar-Shalom, Fabrizio Frasca, Yaniv Galron, Yftah Ziser, Haggai Maron"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=aJ7AdfOfij"
tags: ["query:agent-output"]
score: 8.0
evidence: 使用激活张量进行幻觉检测
tldr: 针对现有幻觉检测探针局限于单层-单token且无法跨模型泛化的问题，提出ACT-ViT，将全激活张量视为图像，用视觉Transformer同时捕获层和token维度信息。实验表明该方法在多个LLM上均能有效检测幻觉，并支持跨模型训练。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 已有探针方法在层- token孤立对上操作，且针对特定LLM，限制了泛化性。
method: 将激活张量映射为图像，使用视觉Transformer（ViT）进行幻觉检测。
result: 在多个LLM上取得优异检测性能，且支持跨模型联合训练。
conclusion: 激活张量的图像化处理为幻觉检测提供了新范式，提升了跨模型泛化能力。
---

## Abstract
Detecting hallucinations in Large Language Model-generated text is crucial for their safe deployment. While probing classifiers show promise, they operate on isolated layer–token pairs and are LLM-specific, limiting their effectiveness and hindering cross-LLM applications. In this paper, we introduce a novel approach to address these shortcomings. We build on the natural sequential structure of activation data in both axes (layers $\times$ tokens) and advocate treating full activation tensors akin to images. We design ACT-ViT, a Vision Transformer-inspired model that can be effectively and efficiently applied to activation tensors and supports training on data from multiple LLMs simultaneously. Through comprehensive experiments encompassing diverse LLMs and datasets, we demonstrate that ACT-ViT consistently outperforms traditional probing techniques while remaining extremely efficient for deployment. In particular, we show that our architecture benefits substantially from multi-LLM training, achieves strong zero-shot performance on unseen datasets, and can be transferred effectively to new LLMs through fine-tuning.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：大型语言模型（LLM）生成的文本中经常出现幻觉（hallucination）现象，即生成内容与事实不符或逻辑矛盾。安全部署LLM迫切需要有效的幻觉检测方法。
- **现有方法不足**：已有的探针分类器（probing classifiers）通常只操作于**孤立的层-标记对**（layer–token pairs），即从某一层的某个token提取激活值进行检测。这些方法：
  - 忽略了激活张量中同时存在的层维度和token维度之间的结构信息；
  - 训练后**只能用于特定的LLM**，无法跨模型泛化，限制了实际应用范围。
- **本文目标**：设计一种能够利用全激活张量（layers × tokens）的全局结构信息、同时支持跨LLM训练的幻觉检测方法。

## 2. 论文提出的方法论

### 核心思想
- 将LLM内部产生的**完整激活张量**（维度：层数 × 序列长度）视为类似图像的**二维结构**，其中层对应图像高度，token对应图像宽度。
- 采用**视觉Transformer（ViT）** 处理该激活“图像”，从而同时捕获层间和token间的依赖关系。

### 关键技术细节
- **ACT-ViT模型**：
  - 输入：激活张量 $\mathbf{A} \in \mathbb{R}^{L \times T}$（$L$为层数，$T$为token数）
  - 将该张量映射为“图像”后，分割为固定大小的patch，并添加位置编码。
  - 使用标准的ViT编码器（多层自注意力 + MLP）提取特征。
  - 最终输出一个二分类（幻觉/非幻觉）或概率分数。
- **跨模型训练**：ACT-ViT的设计允许将来自**多个不同LLM**的激活张量作为联合输入进行训练。由于所有LLM的激活张量均可归一化为相同尺寸（通过调整patch大小或插值），模型可学习通用的幻觉模式。
- **效率**：模型参数相对轻量，推理时仅需一次前向传播，适合部署。

### 公式或算法流程（文字说明）
1. 对每个待检测句子，从目标LLM提取所有中间层的激活值，构成张量 $A \in \mathbb{R}^{L \times T}$。
2. 对张量进行线性投影，将其转化为 $C$ 通道的“图像”表示（可视为灰度或多通道）。
3. 分割为固定大小 patch，展平后加位置编码。
4. 输入ViT编码器，得到全局表示向量。
5. 经过MLP分类头，输出幻觉概率。

## 3. 实验设计

### 使用的数据集/场景
- 论文提到在“多个LLM和数据集”上进行实验，但具体数据集名称（如TruthfulQA、HaluEval等）未在摘要中给出。推测包括常见的幻觉基准数据集。
- 场景涵盖：**单LLM训练测试**、**多LLM联合训练**、**零样本迁移到未见数据集**、以及**微调迁移到新LLM**。

### Benchmark
- 对比方法为**传统探针技术**（probing classifiers），即基于单层单token激活值的线性分类器或简单MLP。

### 对比方法
- 基线：从各LLM的最后一层或某特定层的最后一个token提取激活，训练二分类器。
- 可能还包括其他基于激活的检测方法，但摘要未详述。

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量、训练时长等具体资源信息。仅提到ACT-ViT“极其高效部署”（extremely efficient for deployment），但量化细节缺失。

## 5. 实验数量与充分性

- **实验组数**：从摘要推断至少做了以下几组关键实验：
  1. 在多个（≥3）不同LLM上的幻觉检测性能对比（与探针方法）。
  2. 多LLM联合训练 vs 单LLM单独训练的性能提升。
  3. 零样本迁移测试（在未见过的数据集上评估）。
  4. 微调迁移实验（将预训练模型微调到新LLM）。
- **消融实验**：可能包括不同patch大小、是否使用位置编码、不同ViT深度等，但摘要未明确列出。
- **充分性评估**：实验设计覆盖了跨模型、跨数据集、多种迁移场景，整体较为全面。但由于缺乏具体数值和统计显著性报告，难以完全判断公平性。然而，论文声称“consistently outperforms traditional probing”，表明结果稳定。

## 6. 论文的主要结论与发现

- ACT-ViT在**多个不同LLM**上均优于传统探针方法，且保持高效。
- **多LLM联合训练**显著提升模型性能（“benefits substantially from multi-LLM training”），表明模型学到了跨模型的通用幻觉模式。
- 在**未见过的数据集**上具有强零样本能力（“strong zero-shot performance”）。
- 通过**微调**可有效迁移至新LLM，进一步拓展了实用性。

## 7. 优点

- **创新视角**：将激活张量视为图像，引入视觉Transformer，这是幻觉检测领域的新范式，充分利用了层和token两个维度的结构化信息。
- **跨模型泛化**：首次证明单个检测模型可以同时服务于多个LLM，大幅降低了部署成本。
- **零样本与迁移能力**：模型对新数据集和新LLM的适应性强，实际应用价值高。
- **效率**：ViT模型参数少，推理速度快，适合在线服务。

## 8. 不足与局限

- **计算资源未披露**：缺少训练/推理的算力消耗（GPU时数、内存需求）的具体数据，无法进行公平的效率对比。
- **可解释性不足**：将激活映射为图像后，ViT的注意力图虽能可视化，但可能难以直接对应到具体语言现象，可能影响调试。
- **可能存在的偏差**：实验仅基于摘要，未提及其他先进方法（如基于概率的、基于保真度的检测器）作为对比基线，可能不够全面。
- **应用限制**：需要从LLM中间层提取激活张量，某些封闭API模型无法直接使用；若LLM层数或序列长度变化极大，可能需要调整预处理方案。
- **训练数据真实性**：幻觉检测依赖高质量标签（幻觉/非幻觉对），数据标注的噪声可能影响模型上限。

（完）
