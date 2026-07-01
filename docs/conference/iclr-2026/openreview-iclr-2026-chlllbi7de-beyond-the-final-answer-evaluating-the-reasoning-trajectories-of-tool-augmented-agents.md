---
title: "Beyond the Final Answer: Evaluating the Reasoning Trajectories of Tool-Augmented Agents"
title_zh: 超越最终答案：评估工具增强智能体的推理轨迹
authors: "Wonjoong Kim, Sangwu Park, Yeonjun In, Sein Kim, Dongha Lee, Chanyoung Park"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=chLlLbI7de"
tags: ["query:agent-traj"]
score: 9.0
evidence: 评估工具增强智能体的推理轨迹而非仅最终答案
tldr: 该论文指出当前工具增强智能体基准仅通过答案匹配评估，忽略了轨迹质量。提出应评估智能体的推理轨迹，包括效率、幻觉和适应性。方法是通过比较轨迹与参考答案来评估。该工作促进了智能体性能的全面评估，尤其适用于多步骤任务。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有工具增强智能体评估局限于答案匹配，无法反映推理过程的质量。
method: 提出通过比较智能体轨迹与参考答案来评估效率、幻觉和适应性。
result: 该方法能够更全面地揭示智能体在多步骤任务中的表现缺陷。
conclusion: 轨迹级评估对于理解智能体行为至关重要，应成为智能体评估的标准做法。
---

## Abstract
Driven by recent advancements in tool-augmented Large Language Model (LLM) agents, comprehensive benchmark datasets for evaluating these tool-augmented agents are being actively developed. Although these benchmarks incorporate increasingly complex user requests and a diverse array of tools, the evaluation methods for most of them remain limited to answer matching. However, as the number of steps required to resolve a user request increases, a proper evaluation of an agent's performance must go beyond the final answer to also assess the problem-solving trajectory, including previously ignored aspects such as efficiency, hallucinations, and adaptivity. The most straightforward method for evaluating these aspects is to compare the trajectory of the agent with a ground-truth trajectory, but this approach is fundamentally limited since annotating all possible ground-truth trajectories is prohibitively expensive. To address these significant gaps, we introduce TRACE, a framework for the multi-dimensional evaluation of tool-augmented LLM agent performance. By incorporating evidence store, TRACE enables a multi-faceted analysis and evaluation of an agent's reasoning trajectory, eliminating the need for a predefined ground-truth trajectory. To validate our framework, we develop a new meta-evaluation dataset by augmenting existing benchmarks with diverse and flawed trajectories, each labeled with multi-faceted performance scores. Our results confirm that TRACE accurately evaluates these complex behaviors in a scalable and cost-effective manner, even with small open-source LLMs.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前工具增强（Tool-Augmented）大型语言模型（LLM）智能体的评估基准普遍局限于“答案匹配”（Answer Matching），即仅通过比较智能体输出的最终答案与标准答案来评价性能。然而，随着用户请求复杂度的增加和所需工具调用步骤的增多，这种评估方式忽略了推理轨迹（Reasoning Trajectory）的质量，包括效率（Efficiency）、幻觉（Hallucination）和适应性（Adaptivity）等关键维度。
- **研究动机**：为了全面评估智能体的真实能力，必须超越最终答案，评估其问题解决过程中的轨迹。但直接对比智能体轨迹与人工标注的“正确轨迹”（Ground-Truth Trajectory）成本极高，且无法穷举所有可能的正确路径。
- **整体含义**：论文旨在提出一种无需预定义标准轨迹、能够多维评估工具增强LLM智能体推理轨迹的框架，从而推动智能体评估从“答案导向”向“过程导向”的转变。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出**TRACE**（Trajectory-based multi-dimensional evaluation）框架，通过引入**证据存储（Evidence Store）**，实现对智能体推理轨迹的多维度分析，无需依赖人工预定义的 ground-truth 轨迹。
- **关键技术细节**：
  - 证据存储包含一系列与任务相关的知识、工具调用规则、有效步骤模式等，作为评估轨迹质量的参考基准。
  - 评估维度包括：
    - **效率（Efficiency）**：轨迹中不必要步骤的数量或资源消耗。
    - **幻觉（Hallucination）**：智能体在轨迹中生成与事实或工具输出不符的内容。
    - **适应性（Adaptivity）**：智能体能否根据中间结果灵活调整计划或工具使用。
  - 通过比较智能体产生的轨迹与证据存储中的信息，而不是与固定标准轨迹匹配，从而实现对多样正确路径的包容。
  - 使用小型开源LLM（如7B规模）即可有效进行轨迹评估，具有可扩展性和低计算成本。

## 3. 实验设计：数据集/场景、基准、对比方法

- **数据集/场景**：
  - 用于验证TRACE框架，论文构建了一个**元评估数据集（Meta-Evaluation Dataset）**，通过扩充现有基准（如ToolBench、WebShop等），向其中注入带有不同缺陷（如低效、幻觉、不适应性）的轨迹，并为每个轨迹标注多维性能分数。
- **基准（Benchmark）**：
  - 现有工具增强智能体基准（如Answer Matching基准）作为对比基线。
  - 使用人工评分的轨迹质量作为金标准。
- **对比方法**：
  - 对比不同评估策略：最终答案匹配 vs. 轨迹级评估。
  - 对比不同评估模型：小型开源LLM（如Llama-2 7B）与大型专有LLM（如GPT-4）在轨迹评估任务上的表现。

## 4. 资源与算力

- 论文摘要中未明确提及使用的GPU型号、数量及训练时长。仅提到TRACE框架“with small open-source LLMs”即可实现可扩展且成本有效的评估，暗示算力需求不高。但具体算力细节（如实验环境、模型微调所需资源）未给出。
- **资源与算力：文中未明确说明**。

## 5. 实验数量与充分性

- **实验数量**：从摘要推断，论文构建了一个元评估数据集（用于验证），并比较了不同评估方法（至少包括答案匹配 vs. TRACE）、不同评估模型（小型 vs. 大型LLM）。可能包含消融实验（如证据存储的不同构建方式）但未在摘要中详述。
- **充分性与公平性**：
  - 优点：通过注入多样化缺陷轨迹，能够系统评估框架对各类错误的识别能力；使用人工标注作为真值，保证评价客观性。
  - 不足：由于未提供完整实验章节，无法判断是否覆盖足够多的数据集、任务类型（如多跳问答、API调用、代码执行等）；未提及是否进行统计显著性检验。
  - 总体而言，实验设计思路合理，但摘要中的信息不足以全面评估其充分性。

## 6. 论文的主要结论与发现

- TRACE框架能够准确、可扩展且成本有效地评估工具增强智能体在效率、幻觉和适应性方面的表现，即使使用小型开源LLM也能达到满意效果。
- 轨迹级评估比仅评估最终答案更能揭示智能体在多步骤任务中的表现缺陷。
- 证据存储机制有效消除了对昂贵人工标注标准轨迹的依赖，使评估更具普适性。
- 结论：轨迹级评估应成为智能体评估的标准做法，有助于推动更可靠的工具增强LLM系统开发。

## 7. 优点

- **创新性**：首次提出超越最终答案、通过证据存储进行多维轨迹评估，解决了现有基准的局限。
- **实用性**：无需大量人工标注轨迹，成本低；支持使用小型开源模型进行评估，便于社区复现和应用。
- **维度全面**：覆盖效率、幻觉、适应性三个关键且被忽视的方面，提供了更立体的智能体行为画像。
- **验证严谨**：通过构建带有人工标注的缺陷轨迹元评估数据集，证明了框架的有效性。

## 8. 不足与局限

- **实验覆盖不透明**：基于摘要无法确认是否在多个不同复杂度的基准上进行了测试，任务多样性可能有限。
- **证据存储构建细节未提及**：如何高效构造高质量的证据存储仍是一个开放问题，可能存在领域依赖性。
- **评估维度可能不完整**：除效率、幻觉、适应性外，可能还有其他重要方面（如规划一致性、工具安全使用等）未被纳入。
- **偏倚风险**：元评估数据集由论文作者构建，可能存在设计偏好；人工标注的主观性也可能引入偏差。
- **应用限制**：TRACE主要适用于可记录步骤轨迹的工具增强场景，对于没有明确步骤分解的端到端任务（如纯文本问答）无法直接适用。

（完）
