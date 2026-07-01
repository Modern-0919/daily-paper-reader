---
title: "ExploraQA: Embodied Question Answering with Long-horizon Proactive Exploration"
title_zh: ExploraQA：具有长时域主动探索的具身问答
authors: "Hohin Kwan, Jinyu Chen, Chen Gao, Shifeng Zhang, Xu Zhou, Si Liu"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=MhYqm6v0yw"
tags: ["query:agent-traj"]
score: 6.0
evidence: 具身智能体评估，包含长时域探索轨迹
tldr: "该论文提出ExploraQA，一个大规模具身问答数据集，包含12,436个多样化开放问题，强调长时域主动探索轨迹和全面视角标注。现有EQA基准受限于探索范围、被动轨迹和视角不足。ExploraQA通过设计需要自主探索的环境来评估语言、视觉和空间推理，为测试自主智能体行为提供了更具挑战性的基准。"
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有具身问答基准探索范围有限、轨迹被动且视角标注不足。
method: 构建大规模数据集，包含主动探索轨迹和多角度标注，设计多样化问题。
result: 数据集规模大，任务多样，能够有效评估智能体的探索和推理能力。
conclusion: ExploraQA提升了具身问答评估的难度和全面性，有助于推动自主智能体研究。
---

## Abstract
Embodied Question Answering (EQA) is a critical task for developing embodied intelligence, requiring agents to autonomously explore environments and answer human questions through perception, navigation, and reasoning. However, existing EQA benchmarks suffer from three key limitations: constrained exploration scope, passive trajectory, and insufficient viewpoint annotation. To address these challenges, we introduce ExploraQA, a large-scale dataset featuring 12,436 diverse, open-ended questions across seven categories, designed to evaluate language, visual, and spatial reasoning. ExploraQA emphasizes long-horizon exploration, proactive trajectory, and comprehensive viewpoint annotations, enabling rigorous assessment of autonomous agents. We further propose an Iterative EQA Data Generation Framework to efficiently produce high-quality annotations via VLMs and human verification. To enhance exploration, we present the Answer Quality-Guided Navigator, which leverages a Topology-Aware Keyframe Search Module for efficient long-range navigation and an Answer Quality Reward Mechanism to optimize question-driven trajectories through dual LLM evaluators. Experimental results show that AQ-Nav achieves a 5.4% absolute improvement in E_score on the ExploraQA unseen test set over state-of-the-art navigators. We will release our dataset and code.

---

## 论文详细总结（自动生成）

# ExploraQA 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究领域**：具身问答（Embodied Question Answering, EQA），要求智能体自主探索环境、感知、导航并回答人类问题，是实现具身智能的关键任务。
- **现有瓶颈**：当前 EQA 基准存在三个主要局限：
  - **探索范围受限**：环境规模小、场景单一，智能体无需长时域探索即可回答。
  - **轨迹被动**：大多依赖预设或随机探索轨迹，缺乏主动规划能力。
  - **视角标注不足**：缺乏多角度、全面视角的标注，难以评估空间推理。
- **核心目标**：构建一个大规模、高难度、强调长时域主动探索的 EQA 数据集 ExploraQA，并设计相应的探索导航方法，推动自主智能体在复杂环境中的语言、视觉和空间推理能力评估。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：通过**大规模数据生成**（利用 VLM 与人工验证）和**主动探索导航机制**，解决现有基准被动轨迹与视角不足的问题。
- **关键技术细节**：
  1. **Iterative EQA Data Generation Framework（迭代式 EQA 数据生成框架）**：
     - 利用视觉语言模型（VLM）自动生成问题-答案对，并经过人工校验，高效产出高质量标注。
     - 涵盖7类开放问题，共12,436个样本，强调**多视角全面标注**。
  2. **Answer Quality-Guided Navigator（AQ-Nav，答案质量引导导航器）**：
     - **拓扑感知关键帧搜索模块（Topology-Aware Keyframe Search Module）**：构建环境拓扑图，选择关键帧作为导航锚点，实现高效长程导航，避免无效探索。
     - **答案质量奖励机制（Answer Quality Reward Mechanism）**：使用双大语言模型（LLM）评估器，根据当前观测下答案质量（如正确性、置信度）给予奖励，指导智能体选择能最大化预期答案质量的轨迹。
     - 整体流程：智能体在环境中通过拓扑感知模块选择下一个目标位置，到达后更新观测，双LLM评估当前回答质量，计算奖励并反馈，循环直至问题被高质量回答或探索终止。

## 3. 实验设计
- **数据集 / 场景**：
  - 使用论文提出的 **ExploraQA 数据集**（12,436个开放问题，7类，含训练/验证/测试集分割）。未见公开场景种类，但强调环境多样性和长时域。
  - 测试集包括**未见（unseen）场景**，用于评估泛化能力。
- **基准（Benchmark）**：ExploraQA 自身作为评估基准，指标为 **E_score**（可能为综合正确率与探索效率的分数）。
- **对比方法**：与当前最先进的导航器（state-of-the-art navigators）进行对比。具体方法名称未在摘要中列出，但明确 AQ-Nav 在 ExploraQA unseen 测试集上相比 SOTA 提升 5.4%（绝对），表明对比了多种已有导航方法。

## 4. 资源与算力
- **论文中未明确说明**使用的 GPU 型号、数量、训练时长等算力信息。
- 仅可推断 VLM 生成数据（需要 GPU 推理）和双 LLM 评估器（也需要一定推理资源），但具体规模未披露。

## 5. 实验数量与充分性
- **实验组数**：摘要仅提及在 ExploraQA unseen 测试集上与 SOTA 对比的结果。推测论文中应有：
  - **主实验**：在 ExploraQA 数据集上，AQ-Nav 与多种基线（如随机探索、贪婪探索、基于导航的 baseline）对比。
  - **消融实验**：可能消融拓扑感知模块、双 LLM 奖励机制等组件。
  - **可能的数据集规模分析**：问题类数、轨迹长度分布等。
- **充分性评估**：
  - **覆盖性**：仅在一个数据集上评估，缺乏在现有 EQA 基准（如 MP3D-EQA、Habitat-EQA）上的对比，可能限制了泛化结论的可靠性。
  - **客观公平**：使用自己提出的数据集作为唯一测试床，可能存在过拟合风险；但 unseen 测试集设置有助于评估泛化。未提及采用交叉验证或多种子实验。
  - **消融分析**：若对各个模块做充分消融，则实验较为充分；但摘要未提供细节。

## 6. 论文的主要结论与发现
- 现有 EQA 基准存在探索范围、轨迹和视角三方面不足，导致对自主智能体能力的评估不够全面。
- 构建的 **ExploraQA 数据集** 规模更大、问题更开放、需要长时域主动探索，能提供更具挑战性的评估。
- 提出的 **AQ-Nav 导航器** 利用拓扑感知关键帧搜索和答案质量奖励机制，在未见场景上相比 SOTA 提升 5.4% 的 E_score，验证了主动探索策略的有效性。

## 7. 优点
- **数据构建方法创新**：利用 VLM 迭代生成+人工校验，高效产出大规模、多视角、开放问题的 EQA 数据，降低标注成本。
- **导航机制设计巧妙**：拓扑感知模块解决长程导航效率问题，双 LLM 质量奖励提供探索信号，两者协同实现了主动、自适应的探索策略。
- **评估维度全面**：数据集包含7类问题，同时评估语言理解、视觉感知和空间推理能力，覆盖多模态智能核心。
- **问题动机明确**：精准指出现有基准的三大局限并系统解决，对领域有明确推动作用。

## 8. 不足与局限
- **算力与开源细节缺失**：未报告模型训练/推理所需的 GPU 资源，不利于复现和资源评估。
- **实验多样性不足**：仅在自己的数据集上评估，未在已有公开基准（如 Matterport3D、Habitat 等）上进行对比，难以证明方法的通用性和迁移能力。
- **评估指标单一**：仅使用 E_score（可能为自定义综合指标），缺乏与主流指标（如成功率、SPL、正确率等）的对齐，不利于跨论文比较。
- **数据偏差风险**：数据生成依赖 VLM 和人工校验，可能存在由 VLM 偏好或人工主观性带来的偏差，影响数据代表性。
- **应用限制**：方法依赖双 LLM 评估器，推理成本高，不适合实时或资源受限的具身系统；拓扑构建需要预知环境结构，限制了在未知动态环境中的应用。

（完）
