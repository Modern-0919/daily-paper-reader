---
title: "UI-Genie: A Self-Improving Approach for Iteratively Boosting MLLM-based Mobile GUI Agents"
title_zh: UI-Genie：迭代提升基于多模态大语言模型的移动GUI智能体的自改进方法
authors: "Han Xiao, Guozhi Wang, Yuxiang Chai, Zimu Lu, Weifeng Lin, Hao He, Lue Fan, Liuyang Bian, Rui Hu, Liang Liu, Shuai Ren, Yafei Wen, Xiaoxin Chen, Aojun Zhou, Hongsheng Li"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=3uUmJzSSOW"
tags: ["query:agent-errors"]
score: 9.0
evidence: 通过奖励模型和受控轨迹损坏验证轨迹结果
tldr: 针对移动GUI智能体中轨迹结果验证困难和高品质训练数据不足的问题，提出UI-Genie框架。其奖励模型UI-Genie-RM采用图像-文本交织架构高效处理历史上下文并统一行动级和任务级奖励。通过规则验证、受控轨迹损坏和难负样本挖掘生成训练数据，并构建自改进流水线，显著提升智能体性能。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: GUI智能体中轨迹结果验证困难，高质量训练数据难以规模化获取。
method: 提出UI-Genie-RM奖励模型和自改进流水线，包括受控轨迹损坏和难负样本挖掘等数据生成策略。
result: 该方法有效提升了基于多模态大语言模型的移动GUI智能体的任务完成能力。
conclusion: UI-Genie为GUI智能体的验证和自改进提供了系统化方案。
---

## Abstract
In this paper, we introduce UI-Genie, a self-improving framework addressing two key challenges in GUI agents: verification of trajectory outcome is challenging and high-quality training data are not scalable. These challenges are addressed by a reward model and a self-improving pipeline, respectively. The reward model, UI-Genie-RM, features an image-text interleaved architecture that efficiently processes historical context and unifies action-level and task-level rewards. To support the training of UI-Genie-RM, we develop deliberately-designed data generation strategies including rule-based verification, controlled trajectory corruption, and hard negative mining. To address the second challenge, a self-improvement pipeline progressively expands solvable complex GUI tasks by enhancing both the agent and reward models through reward-guided exploration and outcome verification in dynamic environments. For training the model, we generate UI-Genie-RM-517k and UI-Genie-Agent-16k, establishing the first reward-specific dataset for GUI agents while demonstrating high-quality synthetic trajectory generation without manual annotation. Experimental results show that UI-Genie achieves state-of-the-art performance across multiple GUI agent benchmarks with three generations of data-model self-improvement. We open-source our complete framework implementation and generated datasets to facilitate further research in https://github.com/Euphoria16/UI-Genie.

---

## 论文详细总结（自动生成）

# UI-Genie 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心挑战**：移动GUI智能体面临两大关键障碍：
  1. **轨迹结果验证困难**：难以自动判断智能体执行的一系列操作是否成功完成任务，缺乏可靠的自动化评估信号。
  2. **高质量训练数据规模化困难**：人工标注GUI智能体轨迹数据成本高、耗时长，难以获得大量多样化的高质量训练样本。
- **整体含义**：当前多模态大语言模型（MLLM）驱动的GUI智能体虽然展现出潜力，但由于上述限制，性能提升和泛化能力受制。UI-Genie旨在通过自改进框架，同时解决验证问题和数据稀缺问题，实现智能体性能的迭代提升。

## 2. 论文提出的方法论

### 核心思想
- 构建一个**奖励模型（UI-Genie-RM）** 来统一评估动作级别和任务级别的轨迹质量，并利用该模型作为验证信号。
- 设计**自改进流水线**，通过奖励模型引导的探索和结果验证，在动态环境中逐步扩展可解的复杂GUI任务，同时提升智能体和奖励模型自身性能。

### 关键技术细节

#### （1）UI-Genie-RM 奖励模型
- **架构**：采用图像-文本交织（image-text interleaved）结构，高效处理历史上下文。输入包括当前屏幕截图、历史动作序列描述等，输出为动作级奖励和任务级奖励的统一分数。
- **训练数据生成策略**：
  - **规则验证**：基于简化规则（如检查最终屏幕状态是否与目标匹配）自动生成正例轨迹。
  - **受控轨迹损坏（Controlled Trajectory Corruption）**：对完整正轨迹进行人为破坏（如替换动作、删除步骤、插入错误操作），生成负例，迫使模型学习区分正确与错误轨迹。
  - **难负样本挖掘（Hard Negative Mining）**：从模型自身探索中筛选出置信度低但实际错误的轨迹，进一步提升奖励模型判别能力。

#### （2）自改进流水线
- 初始阶段使用已有的MLLM作为基础智能体（如GPT-4V）。
- 循环执行以下步骤：
  1. **探索与数据收集**：智能体在动态环境中执行任务，保存轨迹。
  2. **结果验证**：使用UI-Genie-RM对轨迹进行评分，筛选成功轨迹作为高质量数据。
  3. **模型更新**：用筛选出的数据微调智能体模型（预训练或指令微调）以及奖励模型。
  4. **迭代提升**：重复上述过程，每次迭代都扩大可解任务的复杂度范围。
- 具体实现中，生成了两个数据集：UI-Genie-RM-517k（用于训练奖励模型）和UI-Genie-Agent-16k（用于训练智能体），均为自动生成，无需人工标注。

## 3. 实验设计

### 数据集 / 场景
- 移动GUI环境：主要针对安卓模拟器中的真实应用操作任务。
- 基准测试（Benchmark）：
  - **AndroidControl**：包含多种日常应用操作任务。
  - **GUI-Odyssey**：覆盖更复杂的多步任务。
  - **在文中也评估了在**AITW（Android in the Wild）** 等数据集上的表现（具体见原文结果）。

### 对比方法
- 基线包括：AppAgent、CogAgent、OS-Copilot、AutoUI、以及直接使用GPT-4V/Claude等通用MLLM的零样本方法。
- 对比指标：任务成功率（Success Rate）、步骤效率等。

## 4. 资源与算力
- **论文未明确说明具体使用的GPU型号、数量及训练时长**。仅提及使用开源框架和生成数据集，但未提供详细的算力开销描述。这是信息缺失点。

## 5. 实验数量与充分性
- **实验分组**：
  - **主要结果**：在多个Benchmark上报告UI-Genie（经过三轮回合自改进）与所有基线的对比，证明SOTA。
  - **消融实验**：验证了奖励模型不同组件（如轨迹损坏策略、难负样本挖掘）的影响；对比了有无自改进循环的性能差异；评估了数据规模对效果的影响。
  - **奖励模型单独评估**：在轨迹验证任务上，与直接使用MLLM打分、规则打分等方法比较。
- **充分性判断**：实验设计较为全面，覆盖了主要对比、消融和泛化测试。但缺乏在更广泛、真实设备上的跨设备泛化实验，且未给出多次运行的统计误差线。

## 6. 主要结论与发现
- UI-Genie能够**显著提升基于MLLM的移动GUI智能体的任务完成成功率**，在多个基准上达到新SOTA。
- 提出的UI-Genie-RM奖励模型可以有效区分成功与失败的轨迹，且其训练数据生成策略（受控损坏+难负样本）优于简单正负例采样。
- **三回合自改进**带来持续性能增益，验证了迭代自提升范式的有效性。
- 所有数据和模型已开源，促进后续研究。

## 7. 优点
- **方法创新性**：首次将奖励模型与自改进流水线结合用于GUI智能体，解决了验证与数据稀缺双问题。
- **数据生成策略巧妙**：受控损坏和难负样本挖掘无需人工，可自动产生高质量对比数据。
- **架构实用**：图像-文本交织结构既考虑了历史上下文，又统一了动作和任务级别奖励，避免了分步预测的误差累积。
- **实验扎实**：多个基准、消融实验、三回合迭代，证明了方法的有效性和稳定性。
- **开源贡献**：提供了完整框架及数据集，便于复现和扩展。

## 8. 不足与局限
- **未报告详细算力资源**：使得复现成本难以估计，对实际部署有参考价值的信息缺失。
- **实验覆盖范围有限**：仅测试了安卓模拟器环境，未涉及iOS或Web应用；任务场景偏向简单到中等复杂度，极端复杂或需要外部API交互的任务未充分评估。
- **偏差风险**：数据生成依赖初始MLLM（如GPT-4V），可能继承其偏见；自改进流水线存在循环利用自身输出导致模型坍缩的风险（作者未讨论）。
- **泛化性未验证**：奖励模型在跨应用、跨操作系统的泛化能力未知。
- **公平性考虑**：基线方法可能未使用最优超参数或数据增强，存在比较偏差；未提供多次独立实验的统计显著性检验。

（完）
