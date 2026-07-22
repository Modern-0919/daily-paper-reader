---
title: "TAI3: Testing Agent Integrity in Interpreting User Intent"
title_zh: TAI3：测试智能体在解释用户意图时的完整性
authors: "Shiwei Feng, Xiangzhe Xu, Xuan Chen, Kaiyuan Zhang, Syed Yusuf Ahmed, Zian Su, Mingwei Zheng, Xiangyu Zhang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=Gf4oPoluAV"
tags: ["query:agent-errors"]
score: 9.0
evidence: 针对LLM智能体意图完整性违反的API中心压力测试
tldr: 本文提出TAI3框架，以API文档为中心生成真实任务，并通过目标变异暴露LLM agent的意图完整性违反，包括工具选择错误和参数错误。该方法直接检测工具调用中的错误类型，与用户需求高度吻合。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: LLM agent在工具调用中常误解用户意图，传统测试方法无法处理自然语言歧义。
method: 基于工具文档生成任务并应用定向变异，系统性地发现agent意图违背。
result: TAI3能有效检测工具选择、参数等错误，超越固定基准方法。
conclusion: 为agent工具调用错误检测提供了可扩展的测试框架。
---

## Abstract
LLM agents are increasingly deployed to automate real-world tasks by invoking APIs through natural language instructions. While powerful, they often suffer from misinterpretation of user intent, leading to the agent’s actions that diverge from the user’s intended goal, especially as external toolkits evolve. Traditional software testing assumes structured inputs and thus falls short in handling the ambiguity of natural language. We introduce TAI3, an API-centric stress testing framework that systematically uncovers intent integrity violations in LLM agents. Unlike prior work focused on fixed benchmarks or adversarial inputs, TAI3 generates realistic tasks based on toolkits’ documentation and applies targeted mutations to expose subtle agent errors while preserving user intent. To guide testing, we propose semantic partitioning, which organizes natural language tasks into meaningful categories based on toolkit API parameters and their equivalence classes. Within each partition, seed tasks are mutated and ranked by a lightweight predictor that estimates the likelihood of triggering agent errors. To enhance efficiency, TAI3 maintains a datatype-aware strategy memory that retrieves and adapts effective mutation patterns from past cases. Experiments on 80 toolkit APIs demonstrate that TAI3 effectively uncovers intent integrity violations, significantly outperforming baselines in both error-exposing rate and query efficiency. Moreover, TAI3 generalizes well to stronger target models using smaller LLMs for test generation, and adapts to evolving APIs across domains.

---

## 论文详细总结（自动生成）

# TAI3：测试智能体在解释用户意图时的完整性 — 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

LLM agent（大语言模型智能体）越来越多地被部署来自动化真实世界任务，其工作方式是通过自然语言指令调用外部API。然而，这些agent经常误解用户意图，导致其行为偏离用户的真实目标——随着外部工具包的演进，这一问题愈发严重。传统的软件测试方法假设结构化输入，无法处理自然语言的歧义性。为此，论文提出了一种以API为中心的应力测试框架 **TAI3**（Testing Agent Integrity in Interpreting User Intent），旨在系统地揭露LLM agent在工具调用过程中的意图完整性违反（intent integrity violations），包括工具选择错误和参数错误。

## 2. 论文提出的方法论

### 核心思想
以工具包的API文档为中心生成真实任务，并通过**定向变异**（targeted mutations）来暴露agent的细微错误，同时保持用户意图不变。该方法直接检测工具调用中的错误类型，与用户需求高度吻合。

### 关键技术细节
- **语义分区**（Semantic Partitioning）：将自然语言任务基于API参数及其等价类划分为有意义的类别，形成不同的测试分区。
- **种子任务变异与排序**：在每个分区内，对种子任务进行变异，并通过一个轻量级预测器估算触发agent错误的可能性，按概率排序后优先测试。
- **数据类型感知策略记忆**（Datatype-aware Strategy Memory）：维护一个记忆模块，存储过往案例中有效的变异模式，并在新测试用例中检索和适配这些模式，提升测试效率。
- 测试生成过程不使用大规模LLM，而是用小模型生成测试，从而降低计算成本。

## 3. 实验设计

- **数据集/场景**：使用了 **80个工具包API** 作为测试对象，涵盖多个领域。
- **基准方法**：对比了固定基准（固定任务集）和传统对抗输入方法，但未具体列出对比方法名称（论文摘要提及显著超越baseline in error-exposing rate and query efficiency）。
- **实验设置**：未详细说明具体的数据集划分，但强调生成测试任务基于每个工具包的API文档。

## 4. 资源与算力

论文中**未明确说明**使用了多少算力（GPU型号、数量、训练时长）。仅提到用较小的LLM（smaller LLMs）作为测试生成模型，且TAI3能够泛化到更强的目标模型，但具体计算资源细节缺失。

## 5. 实验数量与充分性

- **实验数量**：基于80个API进行了测试，并与多个基线进行了对比。但摘要中未提及消融实验的具体数量（如是否包括分组消融、不同变异策略的对比等）。
- **充分性**：实验覆盖了多种API，且验证了TAI3在错误暴露率和查询效率上的显著优势。然而，缺乏对大规模agent模型（如GPT-4、Claude等）跨架构的详细测试报告；也未说明是否进行了用户研究或真实场景部署验证。整体实验设计较合理，但公开细节有限，公平性依赖于隐藏的基准设置。

## 6. 论文的主要结论与发现

- TAI3能**有效检测**LLM agent在工具选择、参数等方面的意图完整性错误。
- 相比固定基准方法，TAI3在**错误暴露率**和**查询效率**上均有显著提升。
- TAI3能够泛化到更强的目标模型，使用**小模型生成测试**即可适用于不同规模的agent。
- 框架还能自适应地适应不同领域演进的API，具备**可扩展性**。

## 7. 优点

- **以API文档为中心**：生成真实、自然任务，更贴近实际使用场景，避免了人工构造的偏见。
- **语义分区+定向变异**：系统性地覆盖工具调用错误类型（工具选择错、参数错等），而非随机扰动。
- **轻量级预测器 + 策略记忆**：降低了测试生成的计算开销，同时利用历史有效模式提升了测试效率。
- **跨模型泛化**：用小模型生成测试即可测试强LLM agent，实用性强。
- 测试结果直接对应错误的自然语言理解环节，便于开发者定位问题。

## 8. 不足与局限

- **实验覆盖**：仅测试了80个API，未在大规模生态（如数千个API）上验证；也未公开API列表，难以复现。
- **偏差风险**：任务变异基于API文档，可能遗漏文档不完整或隐含语境导致的错误。
- **应用限制**：框架假设用户意图仅通过API调用体现，未考虑多轮交互或隐含意图；且策略记忆的通用性可能受限于领域差异。
- **算力信息缺失**：无法评估其实用成本，也未与现有自动测试框架（如基于模糊测试的方法）进行严谨对比。
- **消融分析不透明**：未详细说明各模块（语义分区、预测器、策略记忆）的独立贡献，削弱了归因可信度。

（完）
