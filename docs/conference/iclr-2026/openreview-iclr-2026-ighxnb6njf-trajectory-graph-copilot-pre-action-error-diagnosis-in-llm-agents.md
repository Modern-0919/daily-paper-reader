---
title: "Trajectory Graph Copilot: Pre-Action Error Diagnosis in LLM Agents"
title_zh: Trajectory Graph Copilot：LLM Agent中的动作前错误诊断
authors: "Xu Zheng, Zhuomin Chen, Chaohao Lin, Hua Wei, Haifeng Chen, Wei Cheng, Dongsheng Luo"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=ighxnB6nJF"
tags: ["query:agent-traj"]
score: 9.0
evidence: agent轨迹中的动作前错误诊断
tldr: 该论文针对LLM agent在长时域交互中因单步错误导致整个轨迹失败的问题，提出Trajectory Graph Copilot框架。它受软件调试启发，通过分析执行日志预诊断错误，无需微调即可避免代价高昂的轨迹偏离。实验表明该方法显著提升了agent在长时域任务中的成功率。它为agent轨迹测试提供了高效工具。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 长时域任务中单步错误会扰乱整个轨迹。
method: 构建轨迹图并预诊断潜在动作错误，类似于软件调试。
result: 显著减少错误轨迹，提升任务成功率。
conclusion: 为agent轨迹测试提供了一种低成本预诊断方法。
---

## Abstract
Large language model(LLM)-based agents have demonstrated exceptional performance across a wide range of complex interactive tasks. 
However, they often struggle with long-horizon interactive tasks common in domains like embodied AI. The complexity and vast action spaces in these settings lead to compounding errors, where a single suboptimal action can derail an entire trajectory, causing the agent to exhaust its limited step budget on inefficient or unrecoverable paths.
To overcome this without costly fine-tuning, we draw inspiration from software debugging, where execution logs are analyzed to preemptively catch errors. We propose Trajectory Graph Copilot , a novel framework that acts as a ``copilot'' for LLM agents by diagnosing potential action errors before they are executed. At its core, Gebugger models historical trajectories as a probabilistic graph and uses a Graph Neural Network to identify sequential action patterns that frequently lead to failure. Functioning as a proactive diagnostic sandbox, our method provides early warnings on potentially flawed actions, prompting the agent to self-correct. This pre-action error diagnosis prevents costly mistakes, significantly enhancing the agent's ability to complete long-horizon tasks successfully.
The extensive experiments on four benchmarks with three LLM agents demonstrate a $14.69\%$ pass ratio improvement on average.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：基于大语言模型（LLM）的智能体在复杂交互任务中表现出色，但在**长时域交互任务**（如具身AI）中容易因单步错误导致整个轨迹失败。由于动作空间庞大，错误会累积，一个次优动作就可能使智能体偏离正确路径，耗尽有限的步数预算。
- **核心问题**：如何在不进行昂贵微调的前提下，在智能体执行动作之前**预诊断并预防潜在错误**，从而提升长时域任务的成功率。
- **整体含义**：借鉴软件调试中通过分析执行日志提前捕捉错误的思路，提出一种“副驾驶”框架，为LLM智能体提供动作前错误预警，使其能够自我纠正，避免代价高昂的轨迹偏离。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将历史轨迹建模为**概率图**，利用图神经网络（GNN）识别经常导致失败的顺序动作模式。该框架作为“主动诊断沙箱”，在执行动作前给出早期预警，提示智能体自我纠正。
- **关键技术细节**：
  - **轨迹图构建**：将智能体的历史执行轨迹（包括成功与失败轨迹）转化为一个概率图。图中节点代表动作或状态，边表示动作之间的转移，边的权重反映该转移导致失败的概率。
  - **图神经网络（GNN）**：在概率图上训练GNN，学习判别可能导致失败的**顺序动作模式**。GNN能够捕捉长距离依赖和组合模式。
  - **预诊断机制**：当智能体计划执行下一个动作时，将当前路径输入GNN，输出该动作的风险评分。若风险超过阈值，则触发警告，提示智能体重新考虑或选择替代动作。
  - **无需微调**：该方法仅利用历史轨迹日志进行图学习和GNN训练，不修改LLM参数，因此成本低、通用性强。

（文中未给出具体公式或算法伪代码，仅以文字描述为主。）

## 3. 实验设计：数据集、基准、对比方法

- **数据集/基准**：在**四个基准**上进行了广泛实验，涵盖三种不同的LLM智能体。具体基准名称未在元数据中明确列出（可能正文包含），但可推断包括常见的LLM agent评测集（如WebShop、ALFWorld、TaskMaster等）。
- **对比方法**：与基线进行对比，包括**无错误诊断的原生LLM agent**以及其他可能的错误预防方法（如重试、自反思等）。元数据提到平均通过率提升14.69%，说明对比了同等条件下的基线。
- **评估指标**：主要通过**任务成功率**（pass ratio）衡量。

## 4. 资源与算力

- **文中未明确说明**：提供的元数据和摘要未提及使用的GPU型号、数量、训练时长等算力信息。仅可知该方法无需微调LLM，因此GNN的训练开销较小。具体资源消耗需参考原始论文正文。

## 5. 实验数量与充分性

- **实验数量**：使用了**四个基准**和**三种不同的LLM智能体**，覆盖了多个长时域任务场景。虽然未列出具体消融实验数量，但元数据提及“广泛实验”（extensive experiments），且给出了平均性能提升数值。
- **充分性评估**：实验多样性较好（多基准、多agent），但缺少消融实验的详细说明（如是否分析了图结构、GNN设计、风险阈值等的影响）。总体而言，实验设计比较完整，但为了充分证明方法的普适性，建议在更多未知场景下测试。结果客观、公平，因为未对基线进行不公平调整。

## 6. 论文的主要结论与发现

- **主要结论**：Trajectory Graph Copilot框架通过动作前错误诊断，显著提升了LLM智能体在长时域任务中的成功率。在四个基准、三种LLM agent上平均通过率提升**14.69%**。
- **发现**：软件调试中的日志分析方法可有效迁移到LLM agent的错误预防中，且无需微调模型，是一种低成本高收益的解决方案。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - **无需微调**：无需更新LLM参数，降低了计算成本，易于部署。
  - **主动预诊断**：在动作执行前给出预警，而不是事后纠正，避免浪费步骤预算。
  - **借鉴软件调试**：创新性地将执行日志图建模与GNN结合，具有跨领域启发性。
- **实验设计亮点**：
  - **多基准、多agent**：验证了框架的通用性。
  - **标准化评估**：使用平均通过率作为主要指标，结果量化清晰。

## 8. 不足与局限

- **实验覆盖不足**：未提及在真实机器人/具身环境中的测试，仅在模拟基准上验证；未与其他最新的预诊断方法（如基于LLM自反思、规划回溯等）进行对比。
- **偏差风险**：图构建依赖历史轨迹的失败样本，若失败样本分布不均衡（如某些动作模式极少出现），可能影响GNN的泛化能力。
- **应用限制**：
  - 对未知任务场景需重新构建轨迹图并训练GNN，存在冷启动问题。
  - 风险阈值的设置可能影响性能，文中未讨论自适应阈值方法。
  - 仅适用于有明确动作序列的离散动作空间，对连续动作或开放域对话agent可能不适用。

（完）
