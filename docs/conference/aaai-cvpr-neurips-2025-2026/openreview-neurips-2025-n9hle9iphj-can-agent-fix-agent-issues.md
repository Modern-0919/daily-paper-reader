---
title: Can Agent Fix Agent Issues?
title_zh: 智能体能修复智能体问题吗？
authors: "Alfin Wijaya Rahardja, Junwei Liu, Weitong Chen, Zhenpeng Chen, Yiling Lou"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=N9HLe9iPhj"
tags: ["query:agent-errors"]
score: 8.0
evidence: 自动修复LLM智能体系统中的问题
tldr: 本文研究自动修复LLM agent系统中的问题（bug报告和功能请求）。通过手动分析201个真实agent问题，揭示了agent系统与传统软件差异，并探索将SWE-agent等SE方法迁移到agent环境。其思路直接服务于agent自动错误纠正与自修复。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: LLM agent系统维护成本高，自动修复agent问题至关重要但尚未被充分探索。
method: 手动分析真实agent问题，并迁移软件工程agent方法进行自动修复。
result: 揭示了agent问题特性，实验表明现有SE agent在agent系统上效果有限。
conclusion: 为agent自动错误修复提供了基础分析和方向。
---

## Abstract
LLM-based agent systems are emerging as a new software paradigm and have been widely adopted across diverse domains such as medicine, robotics, and programming. However, maintaining these systems requires substantial effort, as they are inevitably prone to bugs and continually evolve to meet changing external requirements. Therefore, automatically resolving agent issues (i.e.,bug reports or feature requests) is a crucial and challenging task.  While recent software engineering (SE) agents (e.g., SWE-agent) have shown promise in addressing issues in traditional software systems, it remains unclear how effectively they can resolve real-world issues in agent systems, which differ significantly from traditional software. To fill this gap, we first manually analyze 201 real-world agent issues and identify common categories of agent issues. We then spend 500 person-hours constructing AgentIssue-bench, a reproducible benchmark comprising 50 agent issue resolution tasks (each with an executable environment and failure-triggering tests). We further evaluate state-of-the-art SE agents on AgentIssue-bench and reveal their limited effectiveness (\.e., with only 0.67% - 4.67% resolution rates). These results underscore the unique challenges of maintaining agent systems compared to traditional software, highlighting the need for further research to develop advanced SE agents for resolving agent issues.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：基于大语言模型（LLM）的智能体（agent）系统正成为一种新兴的软件范式，广泛应用于医疗、机器人、编程等领域。然而，这些系统不可避免地存在缺陷，并且需要持续演化以满足外部需求变化，导致维护成本高昂。因此，自动修复智能体问题（包括bug报告和功能请求）成为一项关键且具有挑战性的任务。
- **整体含义**：尽管现有软件工程智能体（如SWE-agent）在传统软件系统的问题修复中展现了潜力，但智能体系统与传统软件存在显著差异，现有方法能否有效处理真实世界的智能体问题尚不清楚。本文旨在填补这一空白，通过实证分析和基准构建，揭示智能体问题自动修复的独特挑战。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：分两步走：首先，通过手动分析真实世界智能体问题，归纳问题类别；其次，构建可复现的基准测试集，并评估现有最先进软件工程智能体的效果。
- **关键技术细节**：
  - **手动分析**：对201个真实智能体问题进行细致分类，识别常见问题类型（如逻辑错误、交互异常、环境依赖等）。
  - **基准构建**：从中选出50个任务组成 **AgentIssue-bench**，每个任务包括一个可执行的运行环境和用于触发失败的测试用例，保证实验的可复现性和可评估性。
  - **评估方法**：将现有最先进的SE智能体（例如SWE-agent）应用于AgentIssue-bench，计算问题解决率（resolution rate）。
- **算法/流程说明**：无显式公式或算法流程，核心为实证研究+基准评估。

### 3. 实验设计：数据集、基准、对比方法

- **数据集/场景**：来源于真实世界中的LLM智能体系统项目，共收集201个真实智能体问题（包括bug报告和功能请求）。
- **基准**：**AgentIssue-bench**，包含50个可复现的智能体问题解决任务，每个任务都配有可执行的环境和失败触发测试。
- **对比方法**：多种最先进的软件工程智能体（如SWE-agent），未列出具体名称，但强调是state-of-the-art SE agents。无自研新方法对比。

### 4. 资源与算力

- **文中未明确说明**：论文摘要及元数据中未提及使用的GPU型号、数量、训练时长等算力信息。仅提到“500 person-hours”的人工标注时间（用于构建基准），但未涉及模型训练或推理的硬件资源。

### 5. 实验数量与充分性

- **实验数量**：
  - 手动分析阶段：覆盖201个真实智能体问题。
  - 基准评估阶段：在AgentIssue-bench的50个任务上评估多个SE智能体。
- **充分性评估**：
  - 优点：基于真实问题，且每个任务配备了环境与测试，确保可复现。
  - 不足：基准规模较小（50个），可能不足以全面代表智能体问题的多样性；未进行消融实验或跨基准对比；仅报告了解决率单一指标，未分析失败原因或分类性能；未与人为修复基线对比。因此实验在广度上有限，但作为初步探索是合理的。

### 6. 论文的主要结论与发现

- **主要结论**：现有最先进的软件工程智能体在智能体问题修复上效果极其有限，解决率仅为 **0.67% 至 4.67%**，显著低于其在传统软件上的表现。
- **关键发现**：智能体系统与传统软件存在本质差异，导致现有SE方法迁移效果差，凸显了开发专门针对智能体问题自动修复方法的必要性。

### 7. 优点：方法或实验设计上的亮点

- **首次系统性研究**：首次对LLM智能体系统中的真实问题进行分类和分析，定义了该领域的研究基础。
- **可复现基准**：构建了带有可执行环境和触发测试的基准AgentIssue-bench，便于后续研究公平比较。
- **人工投入扎实**：投入500人-小时进行手动分析，保证了问题分类和基准构建的质量。

### 8. 不足与局限

- **基准规模较小**：仅50个任务，覆盖场景可能不够全面，泛化能力存疑。
- **未提出新方法**：仅评估现有方法，未提出针对智能体问题的专用修复框架或算法。
- **分析层面较浅**：虽进行了问题分类，但未深入挖掘每个类别失败的具体原因，也未给出改进方向。
- **评估指标单一**：仅报告解决率，缺乏对修复质量、开销、误修复率的分析。
- **未做消融实验**：无法判断不同组件对SE agent性能的影响。
- **人工标注偏差风险**：201个问题的选取和分析可能受标注者主观判断影响，存在选择偏差。

（完）
