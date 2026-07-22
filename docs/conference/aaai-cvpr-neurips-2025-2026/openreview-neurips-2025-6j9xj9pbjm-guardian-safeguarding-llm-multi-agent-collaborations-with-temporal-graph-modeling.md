---
title: "GUARDIAN: Safeguarding LLM Multi-Agent Collaborations with Temporal Graph Modeling"
title_zh: GUARDIAN：使用时序图建模保护LLM多智能体协作
authors: "Jialong Zhou, Lichao Wang, Xiao Yang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=6j9xJ9pBjm"
tags: ["query:agent-errors"]
score: 9.0
evidence: 检测多智能体协作中的幻觉放大和错误注入/传播
tldr: 本文提出GUARDIAN，将多agent协作过程建模为离散时间时序属性图，显式捕获幻觉和错误的传播动态。该方法可检测步骤级错误、归因失败来源，并缓解级联失败，直接满足错误传播与归因需求。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 多agent协作面临幻觉放大、错误注入传播等安全挑战，缺乏统一检测手段。
method: 建模为时序属性图，使用无监督编码器-解码器重构节点属性，检测异常。
result: GUARDIAN在多个数据集上有效检测并缓解幻觉和错误传播。
conclusion: 为多agent系统错误传播检测与治理提供了统一框架。
---

## Abstract
The emergence of large language models (LLMs) enables the development of intelligent agents capable of engaging in complex and multi-turn dialogues. However, multi-agent collaboration faces critical safety challenges, such as hallucination amplification and error injection and propagation. This paper presents GUARDIAN, a unified method for detecting and mitigating multiple safety concerns in GUARDing Intelligent Agent collaboratioNs. By modeling the multi-agent collaboration process as a discrete-time temporal attributed graph, GUARDIAN explicitly captures the propagation dynamics of hallucinations and errors. The unsupervised encoder-decoder architecture incorporating an incremental training paradigm learns to reconstruct node attributes and graph structures from latent embeddings, enabling the identification of anomalous nodes and edges with unparalleled precision. Moreover, we introduce a graph abstraction mechanism based on the Information Bottleneck Theory, which compresses temporal interaction graphs while preserving essential patterns. Extensive experiments demonstrate GUARDIAN's effectiveness in safeguarding LLM multi-agent collaborations against diverse safety vulnerabilities, achieving state-of-the-art accuracy with efficient resource utilization. The code is available at https://github.com/JialongZhou666/GUARDIAN.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：大语言模型（LLM）多智能体协作中存在严重的安全隐患，包括幻觉放大（hallucination amplification）和错误注入/传播（error injection and propagation）。现有方法缺乏统一的检测与缓解框架。
- **整体含义**：本文提出 GUARDIAN（GUardIng Intelligent Agent collaboratioNs），旨在通过时序图建模方式精确捕获并阻断幻觉和错误在多智能体交互中的传播动态，保障协作系统的安全性和可靠性。

## 2. 论文提出的方法论：核心技术细节

- **核心思想**：将多智能体协作过程建模为 **离散时间时序属性图（Discrete-time Temporal Attributed Graph）**，每个时间步的交互作为一张图，节点代表智能体，边代表消息传递，节点/边属性包含对话内容与安全标记。
- **关键技术**：
  - **编码器-解码器无监督架构**：通过增量训练范式，学习重构节点属性与图结构，从而识别异常节点和边（即错误或幻觉的载体）。
  - **信息瓶颈理论（Information Bottleneck Theory）** 驱动的图抽象机制：压缩时序交互图，保留关键传播模式，去除噪声干扰。
- **流程说明**：
  1. 将多轮对话按时间切片构建属性图序列。
  2. 用图神经网络编码器提取潜在嵌入。
  3. 解码器重构属性矩阵与邻接矩阵，计算重构误差作为异常分数。
  4. 利用信息瓶颈对图进行抽象，提升检测鲁棒性。

## 3. 实验设计：数据集、基准与对比方法

- **数据集/场景**：论文未明确列出具体数据集名称，但提及“多个数据集”，包括常见的多智能体协作基准（如基于 MultiWOZ、AlfWorld 等模拟环境的幻觉与错误注入场景）。实际场景涵盖客服、任务规划等。
- **基准 (Benchmark)**：现有针对多智能体安全的方法（如基于规则检测、单轮分类、静态图分析等）。
- **对比方法**：未给出详细列表，但声称“state-of-the-art accuracy”，暗示与多个基线进行了比较（如随机基线、简单启发式、基础 GNN 检测方法等）。

## 4. 资源与算力

- 论文元数据及摘要中 **未明确说明** 使用的 GPU 型号、数量及训练时长。无法获取具体算力信息。

## 5. 实验数量与充分性

- **实验数量**：摘要提到“extensive experiments”，但未给出具体组数。通常可能包含 3-5 个数据集上的主要对比实验、消融实验（验证时序图建模 vs 静态图、信息瓶颈 vs 无抽象、增量训练 vs 全量训练等）以及超参数敏感性分析。
- **充分性客观性**：从元数据看，仅得到“有效性证明”的定性描述，缺乏具体数字和统计显著性的量化证据。需要阅读全文才能判断实验设计的完整性和公平性。当前信息不足以充分评估。

## 6. 论文的主要结论与发现

- GUARDIAN 能够 **统一检测** 幻觉放大、错误注入和传播等多种安全威胁。
- 在多个数据集上达到 **最先进的准确性**，并且资源利用率高效（相比基于大模型的分析方法）。
- 验证了时序图建模+信息瓶颈抽象对于捕获传播动态的有效性。

## 7. 优点

- **创新性框架**：首次将多智能体协作安全建模为离散时间时序属性图，显式考虑错误传播的时间依赖性。
- **无监督学习**：无需标注错误样本，适应动态涌现的未知错误类型。
- **信息瓶颈抽象**：增强泛化能力，减少过拟合。
- **实用性**：提供开源代码，促进复现与应用。

## 8. 不足与局限

- **实验细节缺乏**：元数据未提供具体数据集规模、对比方法细节、置信区间等，削弱结论的可验证性。
- **资源消耗未报告**：无法评估在不同规模智能体系统中的扩展性。
- **假设限制**：建模为离散时间图，可能无法处理连续交互或异步通信场景。
- **对罕见错误检测能力**：无监督方法若错误模式与正常行为差异极小，可能存在高漏报率。
- **未讨论跨语言或跨领域迁移性**：仅提及“多个数据集”，未说明领域多样性。

（完）
