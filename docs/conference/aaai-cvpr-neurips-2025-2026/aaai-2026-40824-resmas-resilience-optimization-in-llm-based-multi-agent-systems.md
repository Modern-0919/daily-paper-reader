---
title: "ResMAS: Resilience Optimization in LLM-based Multi-agent Systems"
title_zh: ResMAS：基于LLM的多智能体系统弹性优化
authors: "Zhilun Zhou, Zihan Liu, Jiahe Liu, Qingyu Shao, Yihan Wang, Kun Shao, Depeng Jin, Fengli Xu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40824/44785"
tags: ["query:agent-errors"]
score: 7.0
evidence: 基于LLM的多智能体系统在面对智能体故障时的弹性优化，涉及级联失败
tldr: ResMAS研究了LLM多智能体系统在智能体故障等扰动下的弹性问题，发现通信拓扑和提示设计显著影响系统弹性。通过优化这些因素，可以主动设计固有不被破坏的系统，从而缓解由早期错误引起的级联失败。工作直接关联多步骤工作流中的故障传播与预防。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40824/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 825, \"height\": 658, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40824/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 834, \"height\": 501, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40824/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1377, \"height\": 668, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40824/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 874, \"height\": 482, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40824/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 739, \"height\": 376, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40824/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 875, \"height\": 299, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40824/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 875, \"height\": 306, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40824/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 864, \"height\": 913, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40824/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1103, \"height\": 295, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40824/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1836, \"height\": 270, \"label\": \"Table\"}]"
motivation: 多智能体系统易受扰动影响，现有方法多为被动防御而非主动设计弹性。
method: 通过分析通信拓扑和提示设计对系统弹性的影响，提出优化策略。
result: 验证了拓扑与提示设计对弹性提升的关键作用。
conclusion: 主动设计弹性系统比被动防御更有效。
---

## Abstract
Large Language Model-based Multi-Agent Systems (LLM-based MAS), where multiple LLM agents collaborate to solve complex tasks, have shown impressive performance in many areas. However, MAS are typically distributed across different devices or environments, making them vulnerable to perturbations such as agent failures. While existing works have studied the adversarial attacks and corresponding defense strategies, they mainly focus on reactively detecting and mitigating attacks after they occur rather than proactively designing inherently resilient systems.
    In this work, we study the resilience of LLM-based MAS under perturbations and find that both the communication topology and prompt design significantly influence system resilience. Motivated by these findings, we propose ResMAS: a two-stage framework for enhancing MAS resilience. First, we train a reward model to predict the MAS’s resilience, based on which we train a topology generator to automatically design resilient topology for specific tasks through reinforcement learning. Second, we introduce a topology-aware prompt optimization method that refines each agent’s prompt based on its connections and interactions with other agents. 
    Extensive experiments across a range of tasks show that our approach substantially improves MAS resilience under various constraints. Moreover, our framework demonstrates strong generalization ability to new tasks and models, highlighting its potential for building resilient MASs.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：基于大语言模型的多智能体系统（LLM-based MAS）在多个领域表现出色，但其分布式部署特性使其容易受到扰动（如智能体随机故障、回答错误等）的影响。现有研究主要关注对抗性攻击的被动检测与防御，忽略了非恶意的常见错误，也缺乏从系统设计层面主动提升弹性的方法。
- **整体含义**：本文旨在研究MAS在智能体随机故障下的弹性（resilience，定义为系统在扰动下维持功能的能力），并探索如何通过优化通信拓扑和智能体提示来主动设计固有不被破坏的系统，从而缓解早期错误引起的级联失败。

## 2. 方法论：核心思想、关键技术细节、算法流程

- **核心思想**：通过两阶段框架ResMAS，先优化MAS的通信拓扑，再基于拓扑优化各智能体的提示，以提升系统弹性。
- **关键技术细节**：
  - **第一阶段：拓扑优化**
    - 训练一个GNN（图卷积网络）奖励模型，预测给定任务下MAS的弹性（通过预测每个问题在不同错误率下的正确性计算）。
    - 使用GRPO（Group Relative Policy Optimization）算法微调一个小型LLM（Qwen2.5-7B-Instruct），使其能根据任务描述和约束（节点数、边数）自动生成弹性高的拓扑。
    - 奖励函数：格式错误或节点数不符时-1；边数超出时惩罚；正常时奖励为预测弹性+边数归一化项。
  - **第二阶段：拓扑感知的提示优化**
    - 固定第一阶段生成的拓扑，在训练集上运行MAS，收集每个智能体在每一轮讨论中的正确/错误转变情况。将“从错误纠正为正确”的实例作为正样本，“被邻居误导从正确变为错误”的实例作为负样本。
    - 利用更强的LLM（如GPT-4o）基于正负样本以及邻居的提示，迭代更新每个智能体的系统提示，使其学会何时参考邻居、何时坚持己见。
- **算法流程**（文字说明）：
  1. 随机生成大量拓扑，收集其在不同任务及错误率下的正确性数据，训练GCN奖励模型。
  2. 构建指令集（含任务描述、节点/边约束），先对LLM进行SFT（以随机图作为标签）使其学会输出合法格式。
  3. 使用GRPO结合奖励模型对LLM进行强化学习训练，使其能生成高弹性拓扑。
  4. 在特定任务上，使用生成器生成拓扑，然后运行MAS收集交互历史，对每个智能体进行拓扑感知的提示优化。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：
  - **MMLU-Pro**：多学科选择题，测试常识推理。
  - **MATH（level-5）**：最难的数学问题，测试数学推理。
  - **Chess（BIG-Bench子集）**：棋步有效性判断，测试游戏推理。
  - **HumanEval**：代码生成任务，仅用于泛化性测试。
- **基准测试**：定义弹性为归一化曲线下面积（积分近似），在错误率p=0,0.2,0.4,0.6,0.8,1下测试准确率计算。
- **对比方法**：
  - **拓扑优化**：G-Designer（基于图变分自编码器）。
  - **提示优化**：OPRO、TextGrad（单智能体提示优化，用于MAS时使用随机拓扑+相同提示）。
  - **联合优化**：GPTSwarm（同时优化拓扑和提示，但非拓扑感知）。
- **实验设置**：使用Qwen2.5-32B-Instruct作为智能体骨干模型，节点数10/15/20，边数10-60不等。

## 4. 资源与算力

- **文中未明确提及**所使用的GPU型号、数量、训练时长等具体算力信息。仅说明使用Qwen2.5-7B-Instruct作为拓扑生成器，Qwen2.5-32B-Instruct作为智能体，GPT-4o用于提示优化；训练采用LoRA。但未给出详细硬件配置。

## 5. 实验数量与充分性

- **实验数量**：
  - 主要对比实验：三个数据集（MATH、MMLU-Pro、Chess）×三种节点数（10/15/20）×三种边数，共约27组设置，每组均报告主要结果（Tables 1&2）。
  - 消融实验：在三个数据集上分别移除拓扑优化或提示优化（图7）。
  - 泛化性实验：跨任务（HumanEval）、跨模型（GPT-3.5-turbo、GPT-4o-mini）各一组（图6）。
  - 准确性优化实验：在MATH上改变优化目标为准确率，与基线对比Pareto前沿（图5）。
  - 案例研究与提示示例展示。
- **充分性与公平性**：实验覆盖了多任务、多约束、多模型，对比方法包括当前主流方法。所有实验均在同一设置下进行，结果具有统计意义。但未报告多次重复运行的方差，可能影响显著性判断。整体而言，实验设计较为充分公平。

## 6. 主要结论与发现

- MAS在随机故障下比单智能体具有显著更高的弹性，且弹性随智能体数和通信链路数增加而增加。
- 在相同节点/边数约束下，拓扑结构和智能体提示对弹性影响巨大，通过优化这两个因素可以大幅提升弹性。
- ResMAS在所有数据集和约束下均优于所有基线（G-Designer、OPRO、TextGrad、GPTSwarm），证明了其有效性和鲁棒性。
- ResMAS不仅可优化弹性，还可通过修改奖励函数优化准确率，并达到Pareto最优。
- 生成的拓扑和提示具有良好的泛化能力，可直接迁移到未见过的任务（代码生成）和不同骨干模型（GPT-3.5、GPT-4o-mini）上。
- 消融实验证实拓扑优化和提示优化两个阶段均不可或缺。

## 7. 优点

- **主动设计**：区别于现有被动检测/防御方法，从系统设计层面提升弹性，具有本质优势。
- **两阶段解耦**：将拓扑和提示分开优化，降低搜索空间，且通过奖励模型预测弹性避免了耗时在线评估。
- **拓扑感知的提示优化**：充分考虑了智能体在通信图中的位置及邻居交互，生成的提示能有效抵抗扰动。
- **强泛化能力**：拓扑生成器可零样本迁移到新任务和新模型，提示优化也仅需少量训练数据。
- **多功能性**：框架可同时优化弹性和准确率，适应不同应用需求。

## 8. 不足与局限

- **实验覆盖**：主要评估了随机智能体故障这一种扰动类型，未涉及其他扰动（如故意攻击、网络延迟、模型退化等），弹性定义较为单一。
- **偏差风险**：所有实验使用Qwen2.5系列作为主要模型，其他语言模型上的表现未知；泛化性实验仅测试了GPT系列，可能无法代表所有模型族。
- **计算成本**：虽然避免了在线评估，但训练GCN奖励模型和GRPO仍然需要大量数据和计算，文中未提供具体资源消耗，可能对资源有限的研究者不友好。
- **提示优化依赖强LLM**：提示更新环节使用了GPT-4o等更强模型，可能引入额外成本和依赖，且未讨论弱模型提示优化的效果。
- **联合优化缺失**：目前两阶段独立进行，未来可以探索拓扑与提示的联合端到端优化。

（完）
