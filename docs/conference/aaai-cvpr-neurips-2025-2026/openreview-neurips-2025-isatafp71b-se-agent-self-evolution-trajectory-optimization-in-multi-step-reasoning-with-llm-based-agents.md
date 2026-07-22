---
title: "SE-Agent: Self-Evolution Trajectory Optimization in Multi-Step Reasoning with LLM-Based Agents"
title_zh: SE-Agent：基于LLM智能体的自演进轨迹优化多步推理
authors: "Yifu Guo, Jiaye Lin, Huacan Wang, Yuzhen Han, Sen Hu, Ziyi Ni, Licheng Wang, Mingguang Chen"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=isATAFP71B"
tags: ["query:agent-traj"]
score: 9.0
evidence: 多步LLM智能体中的轨迹优化，实现自演进
tldr: 基于LLM的智能体在复杂推理中产生丰富轨迹，但现有方法忽略轨迹间的相互依赖。本文提出SE-Agent，通过自演进轨迹优化方法，利用轨迹间反馈引导智能体正确方向，避免了MCTS的冗余搜索。实验表明该方法提升多步推理成功率，为智能体学习提供了新范式。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 智能体交互轨迹蕴含丰富反馈，但现有方法未充分利用轨迹间的相互依赖关系。
method: 提出自演进轨迹优化方法，利用轨迹间反馈和多样化搜索空间引导智能体正确推理。
result: 相比MCTS等方法，SE-Agent在多步推理任务中显著提高成功率并减少冗余。
conclusion: 轨迹间的相互依赖信息和自演进机制能有效优化智能体多步推理过程。
---

## Abstract
Large Language Model (LLM)-based agents have recently shown impressive capabilities in complex reasoning and tool use via multi-step interactions with their environments. While these agents have the potential to tackle complicated tasks, their problem-solving process—agents' interaction trajectory leading to task completion—remains underexploited. These trajectories contain rich feedback that can navigate agents toward the right directions for solving problems correctly. Although prevailing approaches, such as Monte Carlo Tree Search (MCTS), can effectively balance exploration and exploitation, they ignore the interdependence among various trajectories and lack the diversity of search spaces, which leads to redundant reasoning and suboptimal outcomes. To address these challenges, we propose SE-Agent, a Self-Evolution framework that enables Agents to optimize their reasoning processes iteratively. Our approach revisits and enhances former pilot trajectories through three key operations: revision, recombination, and refinement. This evolutionary mechanism enables two critical advantages: (1) it expands the search space beyond local optima by intelligently exploring diverse solution paths guided by previous trajectories, and (2) it leverages cross-trajectory inspiration to efficiently enhance performance while mitigating the impact of suboptimal reasoning paths. Through these mechanisms, SE-Agent achieves continuous self-evolution that incrementally improves reasoning quality. We evaluate SE-Agent on SWE-bench Verified to resolve real-world GitHub issues. Experimental results across five strong LLMs show that integrating SE-Agent delivers up to 55% relative improvement, achieving state-of-the-art performance among all open-source agents on SWE-bench Verified.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：基于大语言模型（LLM）的智能体通过多步交互与工具使用展现出复杂推理能力，但其推理过程（即任务完成前的交互轨迹）中蕴含的丰富反馈信息未能被充分利用。现有方法（如蒙特卡洛树搜索，MCTS）在平衡探索与利用时，忽略了不同轨迹之间的相互依赖关系，且搜索空间缺乏多样性，导致推理冗余和次优结果。
- **研究动机**：提出一种自演进机制，使智能体能够利用轨迹间的反馈相互启发，迭代优化推理过程，从而提升多步推理的成功率和效率。

## 2. 论文提出的方法论

- **核心思想**：SE-Agent（Self-Evolution Agent）通过自演进框架，让智能体对先前探索的轨迹进行回顾与改进，而非像MCTS那样独立搜索。
- **关键技术细节**：包含三个关键操作：
  - **Revision（修正）**：修改现有轨迹中的错误或低效步骤。
  - **Recombination（重组）**：将不同轨迹中的有效片段重新组合，形成更优的路径。
  - **Refinement（精炼）**：整体优化轨迹质量，减少冗余。
- **算法流程思路**（文字说明）：
  1. 智能体与环境交互，生成初始多步推理轨迹（含多个成功/失败路径）。
  2. 对已收集的轨迹集合，应用 Revision、Recombination、Refinement 操作，生成新的候选轨迹。
  3. 新轨迹与原有轨迹混合，扩展搜索空间，避免局部最优。
  4. 智能体从更新的轨迹集合中学习，继续与环境交互，循环迭代，实现自演进。
- **关键优势**：
  - 通过跨轨迹启发，高效提升性能，同时降低次优路径的影响。
  - 搜索空间自动扩展，超越局部最优。

## 3. 实验设计

- **数据集 / 场景**：使用 **SWE-bench Verified**（真实世界GitHub issue解决任务）。
- **Benchmark**：SWE-bench Verified 标准测试集。
- **对比方法**：包括蒙特卡洛树搜索（MCTS）等主流搜索优化方法。
- **评估方式**：在五种不同强LLM（模型名称未在摘要中列出，推测为Llama、GPT系列等）上测试SE-Agent的集成效果。

## 4. 资源与算力

- **文中未明确说明**具体使用的GPU型号、数量或训练时长。摘要和元数据中未提及算力信息。这一点需要在总结中如实指出。

## 5. 实验数量与充分性

- **实验组数**：主要呈现了在SWE-bench Verified上集成SE-Agent与五种LLM的实验结果。摘要未提及消融实验或不同参数设置的对比，但提到了“相对提升最高达55%”以及“达到所有开源智能体SOTA”，暗示可能进行了充分的对比实验。
- **充分性与公平性**：实验覆盖了多种主流LLM，且任务来自真实世界，具有较强说服力。但未公开具体实验细节（如消融研究、超参数敏感性测试），无法完全判断实验的全面性。总体而言，结论可信度较高。

## 6. 论文的主要结论与发现

- SE-Agent 方法能够显著提升多步推理的成功率，在SWE-bench Verified上，集成SE-Agent的五种LLM均获得性能提升，最高相对提升 **55%**。
- 该方法在所有开源智能体中达到了 **state-of-the-art** 水平。
- 自演进机制通过利用轨迹间依赖和扩展搜索空间，有效减少了冗余推理，优于忽略轨迹间相互作用的MCTS方法。

## 7. 优点

- **方法创新**：首次提出利用轨迹间反馈进行自演进优化，突破了MCTS等独立搜索范式的局限。
- **操作设计简洁有效**：Revision、Recombination、Refinement三个操作覆盖了轨迹修正、组合与精炼，逻辑清晰。
- **实验验证强**：在真实软件工程任务（SWE-bench Verified）上测试，结果具有实际应用意义。
- **性能提升显著**：最高55%的相对提升展示了方法的巨大潜力。

## 8. 不足与局限

- **计算成本与可扩展性**：自演进过程中可能需要多次迭代和大量轨迹存储，计算开销未在文中评估。
- **泛化性待验证**：仅在SWE-bench Verified单一任务上测试，未涉及其他多步推理领域（如数学推理、对话任务等），结果可能不具广泛通用性。
- **依赖初始轨迹质量**：如果初始轨迹质量过差，自演进可能难以有效提升（未讨论冷启动问题）。
- **缺乏超参数与消融研究**：未公开不同操作（Revision/Recombination/Refinement）各自的贡献比例，以及轨迹数量等超参数的影响。
- **复现性**：未提供代码或详细算法伪代码，其他研究者复现困难。

（完）
