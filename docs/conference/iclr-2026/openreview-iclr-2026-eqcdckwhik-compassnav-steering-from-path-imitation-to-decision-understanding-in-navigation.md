---
title: "CompassNav: Steering From Path Imitation to Decision Understanding In Navigation"
title_zh: CompassNav：从路径模仿转向决策理解的导航
authors: "LinFeng Li, Jian Zhao, Yuan Xie, Xin Tan, Xuelong Li"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=eqcDckWHik"
tags: ["query:agent-traj"]
score: 4.0
evidence: 导航agent从路径模仿到决策理解的训练范式转变
tldr: 该论文指出传统导航训练让agent模仿单条正确轨迹，限制了探索和泛化。提出从路径模仿转向决策理解的新范式，并构建Compass-Data-22k数据集，包含强化微调子集以提供决策全景。该方法旨在让agent真正理解导航而非盲目跟随。实验表明新范式提升了agent的决策能力和泛化性。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 路径模仿限制了agent的探索和泛化能力。
method: 构建22k轨迹数据集，采用强化微调提供决策景观。
result: agent决策理解和泛化能力得到提升。
conclusion: 提出了导航训练的新范式。
---

## Abstract
The dominant paradigm for training Large Vision-Language Models (LVLMs) in navigation relies on imitating expert trajectories. This approach reduces the complex navigation task to a sequence-to-sequence replication of a single correct path, fundamentally limiting the agent's ability to explore and generalize. In this work, we argue for and introduce a new paradigm: a shift from Path Imitation to Decision Understanding. The goal of this paradigm is to build agents that do not just follow, but truly understand how to navigate. We materialize this through two core contributions: first, we introduce Compass-Data-22k, a novel 22k-trajectory dataset.Its Reinforcement Fine-Tuning (RFT) subset provides a panoramic view of the decision landscape by annotating all feasible actions with A* geodesic distances. Second, we design a novel gap-aware hybrid reward function that dynamically adapts its feedback to decision certainty, shifting between decisive signals for optimal actions and nuanced scores to encourage exploration. Integrated into an SFT-then-RFT recipe, our CompassNav agent is trained not to memorize static routes, but to develop an internal ``compass'' that constantly intuits the direction to the goal by evaluating the relative quality of all possible moves. This approach enables our 7B agent to set a new state-of-the-art on Goal navigation benchmarks, outperforming even larger proprietary models, and achieve robust real-world goal navigation on a physical robot.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）
- **传统范式缺陷**：当前主流的导航训练方法（基于大视觉语言模型 LVLM）依赖于**模仿专家轨迹**，将复杂的导航任务简化为对单条正确路径的序列到序列复制。这种“路径模仿”范式从根本上限制了智能体的**探索能力**和**泛化能力**。
- **论文目标**：提出一种新范式——从路径模仿转向**决策理解**（Decision Understanding），旨在构建不只是“跟随”而是**真正理解如何导航**的智能体。
- **整体含义**：通过让智能体学习所有可行动作的相对优劣（而非仅模仿一条路径），提升其对导航任务的内在直觉和适应性，从而在未见环境中也能做出合理决策。

## 2. 方法论：核心思想、关键技术细节
### 核心思想
- **范式转变**：不再让智能体记忆静态路线，而是训练其形成一个内部的“罗盘”（compass），能够持续地通过评估所有可能移动的相对质量来直觉地判断目标方向。
- **训练流程**：采用 **SFT (Supervised Fine-Tuning) 后再 RFT (Reinforcement Fine-Tuning)** 的两阶段策略。

### 关键技术细节
- **数据集 Compass-Data-22k**：
  - 包含22,000条轨迹数据。
  - 核心创新是其 **RFT 子集**：为每条轨迹上的所有可行动作（action）标注了基于 **A\* 测地距离**（geodesic distance）的量化质量，从而提供**决策全景图**（panoramic view of the decision landscape）。
- **奖励函数（Gap-Aware Hybrid Reward Function）**：
  - **动态自适应**：根据模型的**决策确定性**（decision certainty）调整反馈形式。
  - **两种模式**：
    - 当决策明确（如最优动作明显优于其他）时，给出**决定性信号**（decisive signals）强化最优动作。
    - 当决策不确定（多个动作质量相近）时，给出**细微分数**（nuanced scores）以鼓励探索。
  - **目的**：平衡利用与探索，防止训练过早收敛或陷入局部最优。
- **训练算法流程（文字说明）**：
  1. 使用 Compass-Data-22k 进行 **SFT 预训练**，让模型学会基础的动作映射。
  2. 利用 RFT 子集和 gap-aware 奖励函数进行 **强化微调**，使模型学会评估所有动作的相对价值，而非仅记住一条路径。
  3. 最终模型在推理时，每一步都会考虑所有可能动作的“方向感”，并选择最优者。

## 3. 实验设计
- **使用的数据集/场景**：
  - 训练：自建的 **Compass-Data-22k** 数据集。
  - 测试：**Goal Navigation benchmarks**（具体名称在摘要中未列出；元数据也未详述）。
  - 实物验证：在**物理机器人**上进行了真实世界的目标导航测试。
- **Benchmark**：Goal Navigation 领域的标准基准（可能包括 Habitat、Matterport3D 等，但文中未明确）。
- **对比方法**：
  - 包括**更大的专有模型**（如 GPT-4V 等未具名模型）。
  - 未列出具体 baseline 方法（如纯模仿学习、经典 RL 等），仅提及超越更大模型。
- **评估指标**：摘要未明确列出（推测为成功率、SPL、导航误差等）。

## 4. 资源与算力
- 摘要及元数据中**未提及**任何具体算力信息（GPU 型号、数量、训练时长等）。仅能推断是7B参数的LVLM，但未说明训练硬件配置。

## 5. 实验数量与充分性
- **实验数量**：摘要仅报告了在 Goal Navigation benchmark 上的最终结果和机器人实物实验。**未说明做了多少组对比实验、是否包含消融研究、超参数分析等**。
- **充分性判断**：
  - 从有限信息看，**实验不够充分**：没有详细展示不同数据集上的性能、不同训练策略（如无RFT、无gap-aware奖励）的对比、泛化能力测试等。
  - 但摘要声称结果 SOTA 且真实机器人可行，**暗示了至少有一定说服力**。由于缺乏完整论文，无法评判公平性与客观性。
- **风险**：实验覆盖可能局限，未见对不同难度环境、不同动作空间、不同噪声条件下的测试。

## 6. 论文的主要结论与发现
- 提出的 **7B 参数模型 CompassNav** 在 Goal Navigation benchmark 上**取得了新的最优（SOTA）结果**，超越了**更大的专有模型**。
- 在**真实世界物理机器人**上成功实现了稳健的导航，证明了方法的实用性。
- 主要结论：从路径模仿转向决策理解的新范式能够**显著提升智能体的决策能力和泛化能力**，而无需增加模型规模。

## 7. 优点（方法或实验亮点）
- **范式创新**：首次明确提出从路径模仿转向决策理解的训练新范式，具有理论指导意义。
- **数据集设计**：Compass-Data-22k 的 RFT 子集标注了所有动作的 A\* 测地距离，提供了全局决策景观，是技术亮点。
- **奖励函数设计**：gap-aware 混合奖励函数能够动态适应决策确定性，兼顾利用与探索，设计巧妙。
- **效率与规模**：仅用 7B 参数就超越更大模型，证明了决策理解范式的效率优势。
- **实物验证**：在真实机器人上测试，增强了方法的实用价值。

## 8. 不足与局限
- **实验覆盖局限**：摘要中未展示在不同导航场景（如室内 vs 室外、静态 vs 动态障碍物）或更复杂指令下的性能；也未与多种主流方法（如 ViPT、NavGPT 等）进行定量对比。
- **信息缺失**：缺乏消融实验，无法评估各组件（如 RFT 子集、gap-aware 奖励）的独立贡献。
- **泛化风险**：仅在一个基准（Goal Navigation）上测试，无法判断该方法对更泛化任务（如问答、场景理解）的影响。
- **数据集细节不清**：Compass-Data-22k 的来源、多样性、是否包含人工标注、是否有偏见等未说明。
- **算力与可复现性**：未公开训练资源，复现成本未知。
- **理论与局限性分析不足**：未讨论模型在长距离、无结构化环境或遇到未见过动作时的潜在失败模式。

（完）
