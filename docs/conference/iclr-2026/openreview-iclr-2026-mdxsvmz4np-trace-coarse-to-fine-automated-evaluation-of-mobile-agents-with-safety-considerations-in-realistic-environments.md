---
title: "TRACE: Coarse-to-Fine Automated Evaluation of Mobile Agents with Safety Considerations in Realistic Environments"
title_zh: TRACE：面向移动Agent的粗到细自动评估与安全考量
authors: "Zijing Gao, Pengshuai Yang, Xue Yu, Benhui Zhuang, Bo Yuan, Chao Deng, Junlan Feng"
date: 2025-09-09
pdf: "https://openreview.net/pdf?id=mDXSvmz4np"
tags: ["query:agent-traj"]
score: 9.0
evidence: 移动agent轨迹的粗到细自动评估
tldr: 该论文针对移动agent在线评估的可靠性和通用性不足问题，提出TRACE方法。它基于VLM，采用粗到细两阶段评估：先按步骤评估，再整体评分。该方法还考虑了环境真实性和操作安全性。实验证明TRACE能准确评估多种移动agent在复杂任务上的表现。它为agent轨迹评估提供了自动化、可靠的方案。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有移动agent评估缺乏可靠性、通用性且忽视安全。
method: 基于VLM的粗到细两阶段轨迹评估，结合安全考量。
result: 在多种环境和agent上实现准确评估。
conclusion: 提供了自动化且安全的移动agent轨迹评估方法。
---

## Abstract
The online evaluation of mobile agents is becoming increasingly important for both accurately assessing agent capabilities and providing reward signals for online reinforcement learning. Evaluating mobile agents on complex multi-step tasks remains challenging, as existing work suffers from limitations in reliability and generality, while overlooking issues of environmental realism and operational safety. This paper introduces TRACE (TRajectory-based Automated Coarse-to-fine Evaluation), a fully automated vision language model (VLM)-based method designed to evaluate arbitrary mobile agents across diverse environments. TRACE evaluates agent trajectories in a two-stage manner, first through step-wise assessment and then through overall judgment, which significantly reduces evaluation difficulty and enhances reliability. Potentially risky or harmful operations are also detected simultaneously during the step-wise assessment. Furthermore, we construct TRACEBench, a scalable benchmark consisting of 187 tasks from 35 commonly used mobile applications, to better reflect the actual performance of agents in realistic online environments. Task design explicitly considers operational safety, and evaluation metrics cover three key dimensions: task completion, safety, and resource consumption. Experiments show that TRACE achieves an F1 score of 0.836 with the open-sourced Qwen2.5-VL-72B-Instruct, indicating high precision as well as better usability and cost-effectiveness. Extensive evaluation of 8 representative mobile agents on TRACEBench reveals that current mobile agents still have substantial room for improvement, particularly in terms of task completion and operational safety.

---

## 论文详细总结（自动生成）

# 论文总结：TRACE: Coarse-to-Fine Automated Evaluation of Mobile Agents with Safety Considerations in Realistic Environments

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有移动agent（mobile agent）的在线评估方法在可靠性（reliability）和通用性（generality）方面存在严重不足，且普遍忽视了环境真实性（environmental realism）和操作安全性（operational safety）问题。
- **研究动机**：随着移动agent在复杂多步骤任务中的广泛应用，准确评估其能力变得至关重要——既用于衡量agent性能，也为在线强化学习提供奖励信号。然而，当前评估工作大多依赖人工或特定规则，难以扩展到多样化的真实环境，且缺乏对安全风险的检测。
- **整体含义**：论文旨在提出一种**全自动化、基于视觉语言模型（VLM）的评估框架**，能够对任意移动agent在多种真实环境中的轨迹进行可靠、安全感知的评估，从而推动移动agent评估的标准化与实用化。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：采用“粗到细”（Coarse-to-Fine）两阶段评估策略，先按步骤（step-wise）进行局部评估，再对整体任务进行全局判断，以降低评估难度并提升可靠性。同时，在阶段评估中同步检测潜在危险或有害操作。
- **关键技术细节**：
  - **TRACE方法**（TRajectory-based Automated Coarse-to-fine Evaluation）：基于VLM（例如Qwen2.5-VL-72B-Instruct）实现对agent轨迹的自动分析。
  - **第一阶段：逐步评估**（Step-wise Assessment）：对agent在任务执行过程中的每个操作步骤进行独立评分，并识别是否包含不安全行为（如访问敏感权限、执行危险操作等）。
  - **第二阶段：整体判断**（Overall Judgment）：综合所有步骤的评估结果以及任务完成标准，给出最终的任务完成度、安全性得分和资源消耗评分。
  - **评估维度**：覆盖三个关键维度——任务完成（Task Completion）、安全性（Safety）、资源消耗（Resource Consumption）。
  - **安全性考量**：在任务设计阶段即明确安全约束，评估时自动检测违反安全规则的操作。

- **无需显式公式或算法流程**：整个方法由VLM驱动，通过精心设计的提示（prompt）和两阶段推理流程实现。

## 3. 实验设计：数据集/场景、Benchmark、对比方法

- **数据集/场景**：作者构建了**TRACEBench**，一个可扩展的基准测试，包含来自35种常用移动应用的187个任务，覆盖真实在线环境。
- **任务设计**：显式考虑操作安全性，例如禁止访问敏感数据、限制非授权权限等。
- **对比方法**：对8种代表性移动agent进行了全面评估（未在摘要中列出具体agent名称，但表明覆盖了主流方案）。
- **评估标准**：使用F1分数衡量评估准确性；同时从任务完成、安全、资源消耗三个维度比较不同agent的表现。

## 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及使用了多少GPU型号、数量或训练时长。仅指出使用了开源VLM模型 **Qwen2.5-VL-72B-Instruct**，该模型本身较大，但评估阶段可能仅需推理，无需大规模训练资源。具体算力信息未提供。

## 5. 实验数量与充分性

- **实验数量**：
  - 构建了包含187个任务的TRACEBench。
  - 评估了8种移动agent。
  - 对TRACE方法本身进行了性能验证（如F1=0.836）。
  - 可能包括消融实验（例如比较单阶段vs两阶段评估），但摘要未详细说明。
- **充分性**：实验设计覆盖了多种应用场景和agent，且考虑了安全性维度，较为全面。但缺少与已有评估方法（如人工评估、传统规则评估）的直接对比细节，也未报告跨环境泛化性实验。总体而言，实验规模适中，但可进一步增加对比基线。

## 6. 论文的主要结论与发现

- **TRACE方法有效性**：基于Qwen2.5-VL-72B-Instruct取得了0.836的F1分数，表明高精度，同时具有更好的可用性和成本效益。
- **当前agent的不足**：对8种代表性移动agent的评估显示，**当前移动agent仍有巨大改进空间**，尤其在**任务完成率**和**操作安全性**方面。
- **自动化评估的可行性**：证明了基于VLM的粗到细两阶段评估可以可靠地替代人工评估，同时内置安全检测功能。

## 7. 优点：方法或实验设计上的亮点

- **自动化程度高**：完全无需人工参与，可扩展至大量任务和agent。
- **安全性优先**：首次在移动agent评估中系统性地将操作安全性作为核心维度，并在任务设计时即考虑安全约束。
- **粗到细策略**：逐步评估+整体判断的设计降低了VLM处理长轨迹的难度，提升了评估可靠性。
- **实用性强**：TRACEBench基于真实移动应用，贴近实际部署环境；评估指标涵盖任务完成、安全、资源消耗三个实际关注点。
- **成本效益**：使用开源模型（Qwen2.5-VL-72B）即可达到较高F1，降低了依赖商业闭源模型的门槛。

## 8. 不足与局限

- **依赖VLM质量**：评估性能受限于底层VLM的理解能力，对复杂或罕见操作可能产生误判。
- **基准规模有限**：仅187个任务、35个应用，尚不能覆盖所有移动场景；任务复杂度和多样性可能不足。
- **缺乏与人工评估的定量比较**：未提供F1相对于人工评估的准确率或一致性分析，无法完全验证自动化评估的绝对可靠性。
- **资源消耗维度定义模糊**：未在摘要中明确“资源消耗”具体指代什么（如电量、内存、网络流量？），可能影响结果解释。
- **泛化性未验证**：未测试在未见过的应用或全新任务类型上的评估表现。
- **算力信息缺失**：无法评估方法在实际部署中的计算开销。

（完）
