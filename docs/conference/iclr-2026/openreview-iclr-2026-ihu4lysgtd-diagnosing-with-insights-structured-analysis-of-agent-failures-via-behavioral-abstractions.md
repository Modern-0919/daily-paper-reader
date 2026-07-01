---
title: "Diagnosing with Insights: Structured Analysis of Agent Failures via Behavioral Abstractions"
title_zh: 洞察式诊断：通过行为抽象对代理故障进行结构化分析
authors: "Jiayi Bi, Yanjie Gao, Tianyin Xu, Fan Yang, Mao Yang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=iHU4LYSgTD"
tags: ["query:agent-traj"]
score: 9.0
evidence: 基于轨迹行为抽象的代理故障诊断方法
tldr: LLM代理故障常隐藏于复杂轨迹中，传统诊断方法难以适用。本文提出AGENTSCOPE，一种神经符号方法，将代理轨迹抽象为结构化行为表示，并利用这些表示进行故障模式诊断。实验证明该方法能有效定位轨迹中的故障点，提升代理评估的可靠性，为代理轨迹分析提供了新工具。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: LLM代理故障难以通过传统软件调试手段发现，而完全依赖LLM判断又不可靠，需要结构化的故障诊断方法。
method: 提出AGENTSCOPE，将代理轨迹抽象为结构化行为表示，通过神经符号推理进行故障模式诊断。
result: 在多个代理任务上验证，能准确诊断轨迹中的故障，优于基线方法。
conclusion: AGENTSCOPE为LLM代理的轨迹级故障诊断提供可靠手段，促进代理的可信评估。
---

## Abstract
With the proliferation of LLM agents, the ability to understand and diagnose failures in agents is essential to achieving superior effectiveness and trustworthiness. As agent failures often manifest via long and complex trajectories, manually finding the needles in the haystack is untenable. However, traditional diagnosis techniques for software bugs can hardly address LLM agent failures, while completely relying on LLMs as the judge yields unreliable diagnosis results. To overcome these challenges, this paper presents AGENTSCOPE, a new neuro-symbolic approach for agent failure mode diagnosis. The key principle of AGENTSCOPE is to abstract agent behavior, based on its trajectories, into structured representations. Furthermore, AGENTSCOPE introduces the concept of neural invariants to specify agent behavior properties. AGENTSCOPE leverages LLM-guided reasoning atop the structured representation against neural invariants to pinpoint both the failure step and its type in the trajectory. We show the effectiveness of AGENTSCOPE on publicly available agent failure datasets (Who&When) and a more comprehensive dataset created by us (AgentErrata), where AGENTSCOPE significantly outperforms the current art in fault localization and attribution accuracy. Our work shows that integrating structured abstractions with LLM-guided reasoning enables effective, reliable, and interpretable diagnosis for agent failures.

---

## 论文详细总结（自动生成）

# 洞察式诊断：通过行为抽象对代理故障进行结构化分析

## 1. 核心问题与整体含义（研究动机与背景）

- **核心问题**：LLM代理（LLM Agent）在复杂任务中常因错误步骤导致故障，这些故障隐藏于冗长且复杂的轨迹（trajectory）中，传统软件调试方法（如断点调试、日志分析）难以适用，而完全依赖LLM作为评判者又存在不可靠性（例如LLM的判断可能产生幻觉或偏差）。
- **整体含义**：为了提升LLM代理的**有效性与可信度**，需要一种结构化的、可解释的故障诊断方法，能够自动定位轨迹中的故障步骤并识别故障类型，从而支持代理的评估、调试和改进。

## 2. 方法论：核心思想、关键技术细节与算法流程

- **核心思想**：提出**AGENTSCOPE**，一种神经符号（neuro-symbolic）方法，将代理轨迹抽象为**结构化行为表示**，并基于这些表示进行故障模式诊断。
- **关键步骤（文字描述）**：
  1. **轨迹抽象**：将LLM代理的原始行为序列（如API调用、LLM输出、环境反馈）转化为结构化的行为表示（例如状态-动作图、谓词序列等）。
  2. **神经网络不变性（Neural Invariants）**：定义一组行为属性规范（类似程序分析中的不变量），用于描述代理在正确执行时应满足的约束。这些不变性通过LLM引导的方式从轨迹中提取或预设。
  3. **故障诊断**：利用LLM引导的推理引擎，在结构化表示上对照神经不变性进行逐步骤检查，定位**故障步骤**（failure step）并识别**故障类型**（failure type）。
- **公式/算法**：文中未给出具体公式，核心流程为：轨迹 → 行为抽象 → 神经不变性匹配 → 故障定位与分类。

## 3. 实验设计

- **数据集**：
  - **Who&When**：一个公开的代理故障数据集（具体任务类型未在摘要中详述）。
  - **AgentErrata**：作者自行创建的更全面的数据集，覆盖更多故障模式。
- **Benchmark**：故障定位与归因的准确率。
- **对比方法**：文中提到“显著优于当前技术”（current art），但未列出具体对比基线（如基于LLM直接判断、规则方法等）。从摘要推测，至少对比了完全依赖LLM作为评判者的方法。

## 4. 资源与算力

- **文中未明确说明**：摘要及元数据中未提及使用的GPU型号、数量、训练时长或推理成本。因此无法总结具体算力消耗。

## 5. 实验数量与充分性

- **实验数量**：至少涉及两个数据集（Who&When 和 AgentErrata）。但文中未报告具体实验组数（如是否包含消融实验、不同抽象粒度对比、不变性设计对比等）。
- **充分性评价**：从摘要看，实验覆盖了公开数据集和自建数据集，且表明方法有效。但缺乏消融研究、跨任务泛化分析、以及与其他神经符号方法的细致比较，**实验充分性一般**。未报告统计显著性（如P值）、多次重复结果等，**客观性受限**。

## 6. 主要结论与发现

- AGENTSCOPE能**准确诊断代理轨迹中的故障步骤与类型**，显著优于当前最先进方法。
- **结构化抽象+LLM引导推理**的结合能够实现有效、可靠、可解释的代理故障诊断，为代理的可信评估提供新工具。

## 7. 优点

- **创新性**：将程序分析中的“不变性”概念引入LLM代理故障诊断，并融合神经符号方法，兼具自动化和可解释性。
- **实用性**：可直接应用于复杂轨迹的自动化调试，减少人工审查负担。
- **通用性**：方法不特定于某一种代理架构或任务，可推广到多种LLM代理场景。

## 8. 不足与局限

- **实验覆盖有限**：仅在两个数据集上验证，且其中一个为自建数据集，缺乏在多个不同领域、大型代理任务上的测评，存在过拟合风险。
- **故障类型覆盖未知**：未说明AgentErrata中包含哪些故障类型，以及AGENTSCOPE能否处理所有常见故障（如幻觉、工具调用错误、规划错误等）。
- **神经不变性设计依赖专家知识或LLM**：不变性的定义质量直接影响诊断效果，但文中未讨论不变性如何自动生成或验证其完备性。
- **计算成本**：未报告推理开销，实际应用中可能因LLM调用次数而成本较高。
- **可重复性**：未提供代码或详细超参数，难以复现结果。

（完）
