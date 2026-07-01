---
title: "SWE-eval: Trajectory-Enhanced Evaluation for Agentic Issue Resolution"
title_zh: SWE-eval：面向代理问题解决的轨迹增强评估
authors: "Tongwei Deng, Xutian Li, Runbang Yan, Wanning Li, Yifeng Zhu, Yue Wang, Jinyu Zhu, Yanzhen Zou, Zexiong Ma, Zhixuan Liu, Bing Xie"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=aPeeUApKtW"
tags: ["query:agent-traj"]
score: 9.0
evidence: 针对编码代理的轨迹增强评估框架
tldr: 现有代码修复基准只评估最终补丁正确性，缺乏对代理工作轨迹的量化评估。本文提出SWE-eval框架，从效率、逻辑一致性等维度评估编码代理的推理轨迹。实验显示该框架能揭示代理改进的关键点，为代理轨迹评估提供了细粒度方法。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有基准仅评估最终结果，无法量化代理解决问题的轨迹过程。
method: 提出SWE-eval，在三个维度（效率、内部逻辑一致性、跨步逻辑一致性）上评估编码代理的轨迹。
result: 能有效识别代理工作过程中的改进点，优于仅检查最终结果的评估。
conclusion: SWE-eval为代理轨迹量化评估提供了新范式，促进代理开发过程的可视化。
---

## Abstract
Agents and Language Models (LMs) demonstrate significant advancements in software engineering, particularly in issue resolution. Current benchmarks can qualitatively assess the correctness of generated patches. However, they lack mechanisms for quantitatively evaluating the trajectory, which is important to reveal the point of improvement. To obtain understanding of issue-resolving agents' working processes, we propose SWE-eval, a trajectory-augmented evaluation framework. SWE-eval additionally assesses a coding agent's reasoning trajectory across three dimensions: (1) Efficiency, measured by resource consumption; (2) Logical Consistency, where Intra-turns measures the logical consistency within a single turn and Inter-turns measures logical consistency across multiple conversation turns; (3) Tool Utilization, for which we design a metric Info-gain to assess how much new information the tool provides for solving problems. Our experiments on three agents and nine LMs demonstrate that SWE-eval effectively reveals underlying interpretations of agent performance and can guide development of more effective agents. First, our evaluations show that elevating trajectory-aware metrics is crucial for enhancing the % Resolved. Second, we trace divergent agent behaviors to shallow exploration, missing backtracking, and loop entrapment. We also show that fine-tuning on agents risks overfitting and scaling LMs improves trajectories. Third, LLM-based evaluations align closely with expert judgments and exhibit consistent stability, serving as reliable proxies.

---

## 论文详细总结（自动生成）

# 论文总结：SWE-eval：面向代理问题解决的轨迹增强评估

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：现有代码修复基准（如SWE-bench）仅评估最终生成的补丁是否正确，缺乏对编码代理（coding agent）解决任务过程中推理轨迹的量化评估。这导致难以定位代理失败的根本原因，也无法指导代理的改进方向。
- **核心问题**：如何设计一个能够对编码代理的工作轨迹进行细粒度、多维度量化评估的框架？
- **整体含义**：提出SWE-eval，通过评估效率、逻辑一致性和工具利用三个维度，揭示代理性能差异的根源，为代理的可视化开发与调优提供新范式。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：在传统最终补丁正确性评估（% Resolved）之外，增加对代理整个推理轨迹的定量评价，从而发现提升点。
- **三个评估维度**：
  1. **效率（Efficiency）**：通过资源消耗（如推理步数、令牌数、时间等）衡量代理的工作开销。
  2. **逻辑一致性（Logical Consistency）**：
     - **内部逻辑一致性（Intra-turns）**：评估单次交互（turn）内部推理步骤之间的逻辑连贯性。
     - **跨步逻辑一致性（Inter-turns）**：评估多次交互（conversation turns）之间，代理推理逻辑的连贯性。
  3. **工具利用（Tool Utilization）**：设计指标**信息增益（Info-gain）**，衡量每个工具调用为解决问题提供的新信息量。
- **算法流程（文字说明）**：
  - 收集编码代理在解决GitHub Issue过程中的完整轨迹（包含LM推理、工具调用、环境反馈等）。
  - 按上述三个维度计算指标，并结合最终补丁正确性进行综合分析。
  - 使用LLM作为评估器（LLM-based evaluation）自动计算逻辑一致性等指标，并与人工专家判断校准。

## 3. 实验设计：使用的数据集/场景、benchmark、对比方法
- **数据集/场景**：基于SWE-bench（或类似代码修复基准）中的GitHub Issue解决任务。实验涉及三个不同的编码代理（agents）和九种语言模型（LMs）。
- **Benchmark**：以最终补丁正确率（% Resolved）作为传统基线，SWE-eval作为新增轨迹评估。
- **对比方法**：主要对比仅使用最终补丁评估（无轨迹分析）与引入SWE-eval后的性能揭示能力。此外，还对比了不同代理、不同LM（包括经过微调的模型与基础模型）在轨迹指标上的差异。

## 4. 资源与算力
- **文中未明确说明**：摘要和元数据中没有提及使用的GPU型号、数量、训练时长等具体算力信息。仅提到使用了9种语言模型，但未给出具体规模或训练成本。

## 5. 实验数量与充分性
- **实验数量**：共涉及3个代理 × 9种LM，以及分维度（效率、逻辑一致性、工具利用）的指标计算，还包含了消融分析（如对比不同代理行为：浅层探索、缺少回溯、循环陷阱）。此外，还进行了LLM评估与专家判断的一致性分析。
- **充分性评价**：实验覆盖了多种代理架构和主流语言模型，基于公开基准任务，且进行了行为归因分析，实验设计较为充分。但缺少跨更多数据集（如其他代码仓库）的验证，以及缺少对指标尺度、权重设定的消融实验。

## 6. 论文的主要结论与发现
- 提升轨迹感知指标（efficiency, logical consistency, info-gain）对提高最终补丁正确率至关重要。
- 代理失败可追溯到三种典型行为：**浅层探索**（不深入分析代码）、**缺少回溯**（发现错误不返回修正）、**循环陷阱**（陷入重复调用同一工具而无进展）。
- 在代理上进行微调可能带来过拟合风险；而扩大语言模型规模有助于改善轨迹质量。
- 基于LLM的评估与专家判断高度一致，且具有良好的稳定性，可作为可靠的替代评估手段。

## 7. 优点：方法或实验设计上的亮点
- **多维细粒度评估**：弥补了现有基准仅关注最终结果的缺陷，提供了可解释的分解指标。
- **工具利用的信息增益指标**：创新性地量化工具调用对问题求解的实际贡献。
- **LLM作为评估代理的自动化方法**：降低人工标注成本，并通过验证其与专家判断的一致性保证了可靠性。
- **实用导向**：通过轨迹分析定位具体失败模式，为改进代理提供明确指导。

## 8. 不足与局限
- **评估成本**：轨迹收集和LLM评估本身需要额外计算资源，可能增加评估开销。
- **指标定义的主观性**：逻辑一致性、信息增益等指标的定义和计算方式可能存在一定的设计偏差，未充分讨论替代方案。
- **数据集单一**：仅基于SWE-bench（代码Issue修复），未在更广泛的天软件工程任务（如重构、测试生成）上验证。
- **未考虑环境差异**：不同代理的工具集配置、超参数等可能影响指标可比性。
- **算力未公开**：缺乏计算资源的详细信息，不利于复现和公平比较。

（完）
