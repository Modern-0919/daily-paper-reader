---
title: Enhancing Decision-Making for LLM Agents via Step-Level Q-Value Models
title_zh: 通过步骤级Q值模型增强大语言模型智能体的决策能力
authors: "Yuanzhao Zhai, Tingkai Yang, Kele Xu, Dawei Feng, Cheng Yang, Bo Ding, Huaimin Wang"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34924/37079"
tags: ["query:agent-errors"]
score: 5.0
evidence: 使用MCTS轨迹的步骤级Q值指导动作选择，与失败归因相关
tldr: 针对多步决策中中间动作难以评估的问题，提出通过蒙特卡洛树搜索（MCTS）收集带步骤级Q值的决策轨迹，并利用步骤级直接偏好优化（DPO）拟合偏好，从而引导智能体动作选择，提升多步任务性能。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34924/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 821, \"height\": 576, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34924/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1745, \"height\": 530, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34924/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1692, \"height\": 535, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34924/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 404, \"height\": 302, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34924/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 394, \"height\": 296, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34924/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 855, \"height\": 261, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34924/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1558, \"height\": 337, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34924/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 872, \"height\": 491, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34924/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 868, \"height\": 184, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34924/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 739, \"height\": 267, \"label\": \"Table\"}]"
motivation: 大语言模型智能体在多步决策任务中，中间动作难以合理奖励或惩罚。
method: 通过MCTS生成带步骤级Q值的偏好数据，用另一个LLM进行步骤级DPO拟合。
result: 提升了智能体在多步决策任务中的表现。
conclusion: 步骤级Q值模型有效增强了LLM智能体的决策能力。
---

## Abstract
Agents significantly enhance the capabilities of standalone Large Language Models (LLMs) by perceiving environments, making decisions, and executing actions. However, LLM agents still face challenges in tasks that require multiple decision-making steps. Estimating the value of actions in specific tasks is difficult when intermediate actions are neither appropriately rewarded nor penalized. In this paper, we propose leveraging a task-relevant Q-value model to guide action selection. Specifically, we first collect decision-making trajectories annotated with step-level Q values via Monte Carlo Tree Search (MCTS) and construct preference data. We then use another LLM to fit these preferences through step-level Direct Policy Optimization (DPO), which serves as the Q-value model. During inference, at each decision-making step, LLM agents select the action with the highest Q value before interacting with the environment. We apply our method to various open-source and API-based LLM agents, demonstrating that Q-value models significantly improve their performance. Notably, the performance of the agent built with Phi-3-mini-4k-instruct improved by 103% on WebShop and 75% on HotPotQA when enhanced with Q-value models, even surpassing GPT-4o-mini. Additionally, Q-value models offer several advantages, such as generalization to different LLM agents and seamless integration with existing prompting strategies.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：大语言模型（LLM）智能体在多步决策任务中，中间动作难以获得合适的奖励或惩罚（环境提供的通常是稀疏的最终结果奖励），导致误差累积、任务失败。
- **研究动机**：现有的方法（如提示策略、树搜索、微调LLM骨干）各有局限：提示策略无法积累任务经验；树搜索在推理时需要环境可回退假设且消耗大；微调LLM骨干需要大量计算且可能损伤通用能力，且不适用于API模型。
- **整体含义**：论文提出通过**步骤级Q值模型**来指导动作选择，在不修改原始LLM智能体的情况下，为每个决策步骤提供动作价值的估计，从而提升多步决策性能。

## 2. 论文提出的方法论
### 核心思想
利用蒙特卡洛树搜索（MCTS）探索高质量轨迹，并为每个中间步骤标注Q值（期望累计奖励）；然后构造偏好数据，训练一个额外的、轻量级的LLM作为Q值模型；推理时，LLM智能体采样多个候选动作，由Q值模型选择最高Q值的动作执行。

### 关键技术细节
- **步骤级Q值估计（MCTS）**：MCTS迭代进行选择、扩展、评估、回传四个阶段。在回传时更新节点值 V(s_t)（公式5），利用确定性转移假设计算步骤级Q值：Q̂(s_t, a_t) = V(s_{t+1})（公式6）。
- **偏好数据构造**：从MCTS最终树中，找出最优路径（最高奖励），沿路径每个深度取该动作作为偏好动作 a_w^t；在同一深度从其他候选动作中选取最低Q值的动作作为非偏好动作 a_l^t，构成三元组 (u, τ_t, a_w^t, a_l^t)。
- **步骤级直接偏好优化（Step-level DPO）**：使用额外LLM（π_θ）通过公式8的损失函数拟合偏好，训练目标是使得偏好动作的隐式Q值大于非偏好动作的隐式Q值。推理时Q值计算为公式9。
- **推理过程**：在每个决策步骤，LLM智能体采样n个候选动作，由Q值模型评分，选择Q值最高的动作与环境交互（公式10）。

### 公式（文字说明）
- UCT选择公式：结合节点值和访问计数平衡探索与利用。
- 节点值更新：V(s_t) ← [V(s_t)*(N(s_t)-1) + r(u,τ)] / N(s_t)。
- Step-level DPO损失：L = -E[log σ(β log π_θ(a_w|u,τ)/π_ref(a_w|u,τ) - β log π_θ(a_l|u,τ)/π_ref(a_l|u,τ))]。

## 3. 实验设计
### 使用数据集/场景
- **WebShop**：在线购物导航任务，奖励范围为0~1（更细粒度）。
- **HotPotQA**：多跳问答任务，奖励为二元（0或1）。
- 训练集/验证集/测试集划分：WebShop：1824训练/100验证/100测试；HotPotQA：1000训练/100验证/100测试。

### Benchmark和方法对比
- **对比方法**：
  - 基础LLM智能体：Phi-3-mini-4k-instruct、Llama-3.1-8B-instruct、GPT-4o-mini、GPT-4-turbo（不加任何增强）。
  - 微调方法：RFT（拒绝采样微调）、AgentEvol（加权轨迹微调）、ETO（DPO微调 using outcome rewards）。
  - 推理方法：Best-of-N（BoN，采样多个完整轨迹后选最高奖励）。
  - 提示策略：ReAct、ReAct + Reflection。
- 所有方法的训练数据都使用相同的MCTS收集，保证公平。

### Q值模型
- 基于Phi-1.5（1.3B参数），训练数据由Phi-3-mini-4k-instruct智能体进行MCTS收集。

## 4. 资源与算力
- 论文明确提到：
  - 除微调方法外，所有实验在**单张NVIDIA A40 48G GPU**上进行。
  - 微调方法（RFT等）需要**两张NVIDIA A100 80G GPU**。
  - Q值模型训练数据收集使用Phi-3-mini-4k-instruct，训练一个epoch。
- 未明确训练时长，但表明使用较小的Q值模型（1.3B）比微调LLM骨干（3.8B）更高效。

## 5. 实验数量与充分性
- **主要实验结果**（表2）：在WebShop和HotPotQA上，对不同LLM后端（4种）分别进行有无Q值模型的对比，共8组主要结果。
- **Q值模型评估**（图4）：偏好准确率（训练集、IND测试集、OOD测试集）和Q值分布图。
- **消融实验**：
  - 步骤级 vs 轨迹级偏好数据（表3）。
  - 不同训练数据量对性能的影响（图5a）。
  - MCTS迭代次数对偏好数据数量的影响（图5b）。
  - 与不同提示策略集成（表4，ReAct+Reflection+Q）。
- **公平性**：所有方法训练数据同源（MCTS收集），BoN使用相同数量的候选动作（n=5）。但实验仅覆盖两个任务，且每个任务测试集规模较小（100题），泛化性需更多任务验证。

## 6. 论文的主要结论与发现
- Q值模型能显著提升LLM智能体决策能力：在WebShop上Phi-3-mini提升103%（0.30→0.61），在HotPotQA上提升75%（0.20→0.35），甚至超过GPT-4o-mini。
- Q值模型比微调LLM骨干更有效且更高效（使用更小模型即达到更好性能）。
- Q值模型可**泛化到不同LLM后端**（包括API模型），且无需额外训练。
- Q值模型与现有提示策略（如Reflection）兼容，叠加使用可进一步提升性能。
- 步骤级偏好数据优于轨迹级偏好数据，提供更细粒度的信用分配。

## 7. 优点
- **即插即用**：无需修改原始LLM智能体，可应用于开源和API模型。
- **轻量高效**：Q值模型只有1.3B参数，训练和推理开销远小于微调LLM骨干。
- **数据高效**：少量偏好数据（约250条指令）即可显著提升性能（图5a）。
- **可迁移性**：用低成本模型（Phi-3-mini）收集数据训练的Q值模型能增强强模型（GPT-4-turbo）。
- **步骤级监督**：通过MCTS分解稀疏奖励，提供中间步骤的价值信号，优于仅使用结果奖励的方法。

## 8. 不足与局限
- **任务覆盖有限**：仅验证了两个任务（WebShop和HotPotQA），在更多领域（如工具使用、代码生成等）的表现未知。
- **二元奖励挑战**：HotPotQA的二元奖励导致Q值模型偏好准确率较低（67%），且性能提升相对较小（9%~14%）。对于只有成功/失败的场景，MCTS早期停止会影响Q值估计质量。
- **MCTS依赖环境可回退**：训练阶段需要环境支持回退或模拟以探索不同分支（虽然推理时不需要），在真实不可逆环境中可能受限。
- **OOD泛化有限**：Q值模型在不同LLM后端（OOD状态/动作）上性能提升幅度小于同源智能体，且偏好准确率下降。
- **计算成本**：MCTS收集数据需要多次与环境交互（图5b显示约30次迭代才能获得足够数据），对复杂环境可能成本较高。
- **未分析失败案例**：论文未讨论Q值模型失败的具体场景或错误模式，缺少定性分析。

（完）
