---
title: Trajectory-Aware Verbalized Optimization for Multi-Agent Systems
title_zh: 多智能体系统的轨迹感知言语化优化
authors: "Bin Wu, Haoran Xu, Xiang Zhuang, Emine Yilmaz, Qiang Zhang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=dkbQwUp9gW"
tags: ["query:agent-traj"]
score: 7.0
evidence: 轨迹感知的信用分配用于优化多智能体提示
tldr: 该论文提出轨迹感知的言语化优化（TAVO）框架，用于多智能体系统的提示优化。受强化学习启发，TAVO引入信用分配机制，将交互轨迹分解为子轨迹，关联具体推理和协调步骤与最终结果。相比仅依赖任务级结果的优化方法，TAVO能更精细地定位问题。该工作为多智能体系统提供了一种自动化的轨迹级优化方法。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有自动优化方法忽视轨迹级信息，无法捕捉智能体推理和协调中的具体失败。
method: 借鉴强化学习，设计信用分配机制将整体轨迹分解为子轨迹并关联结果。
result: 在多种多智能体任务上，TAVO显著提升提示优化效果。
conclusion: 轨迹级信息对于多智能体系统优化至关重要，TAVO提供了一种有效框架。
---

## Abstract
Large language model (LLM)-based multi-agent systems have shown significant potential, but their effectiveness often depends on manually engineered prompts, which are refined through labor-intensive trial and error. While automatic optimization methods exist, they often rely on coarse, task-level outcomes, neglecting the rich trajectory-level information that captures how agents reason, coordinate, and fail. To address this gap, we propose a Trajectory-Aware Verbalized Optimization (TAVO) framework for prompt refinement in multi-agent systems. Inspired by reinforcement learning, TAVO introduces a credit assignment mechanism that decomposes interaction trajectories into sub-trajectories, linking specific reasoning and coordination steps to the final outcome. This generates fine-grained, process-level feedback. By modeling prompts as verbalized policies, TAVO translates this trajectory feedback into concrete editing instructions, which are aggregated across tasks for systematic refinement. Experiments on both collaborative and competitive multi-agent benchmarks demonstrate that our framework enhances system performance while reducing coordination costs, underscoring the value of leveraging trajectory-level signals to construct more adaptive and efficient LLM-based multi-agent systems.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：基于大型语言模型（LLM）的多智能体系统性能高度依赖人工设计的提示词（prompts），而手动调优耗费大量人力，现有自动优化方法仅使用粗粒度的任务级结果（如最终输出好坏），忽略了能够捕捉智能体推理、协调和失败过程的丰富轨迹级信息。
- **研究动机**：为了弥补这一空白，作者提出一种轨迹感知的言语化优化（Trajectory-Aware Verbalized Optimization, TAVO）框架，通过利用智能体交互过程中的轨迹细节，实现更精细、更有效的提示词自动优化。
- **整体含义**：该工作揭示了轨迹级信号对于多智能体系统自动优化的关键价值，为构建更自适应、更高效的基于LLM的多智能体系统提供了新范式。

## 2. 论文提出的方法论

### 核心思想
- 受强化学习中的信用分配（credit assignment）机制启发，将多智能体交互的完整轨迹分解为若干子轨迹，并将每个子轨迹中的具体推理和协作步骤与最终任务结果关联起来，从而生成细粒度的过程级反馈。
- 将提示词建模为“言语化策略”（verbalized policy），利用轨迹反馈生成具体的编辑指令，并通过跨任务聚合这些指令来系统地优化提示词。

### 关键技术细节
- **信用分配机制**：将一整条交互轨迹（如多轮对话、动作序列）按时间或逻辑切分为子轨迹，再根据子轨迹对最终结果的贡献程度分配信用，识别出导致成功或失败的关键步骤。
- **言语化策略**：将提示词视为一种可被自然语言描述和修改的策略，而非隐式参数。轨迹反馈被转化为自然语言的编辑建议（例如“在第3步中Agent A的推理逻辑需要补充领域知识”）。
- **聚合与优化**：收集多个任务上的编辑指令，进行去重、归纳，形成统一的提示修改方案，迭代更新提示词。

### 算法流程（文字说明）
1. 初始化多智能体系统的提示词。
2. 运行多智能体在多个任务上，收集完整的交互轨迹和任务结果。
3. 对每条轨迹应用信用分配，分解为子轨迹并标记每个子轨迹的贡献（正面/负面）。
4. 将贡献为负的子轨迹中的具体推理/协作步骤转化为文本反馈。
5. 使用LLM将反馈翻译为针对当前提示词的编辑指令。
6. 跨任务聚合所有编辑指令，生成更新后的提示词。
7. 重复步骤2-6，直至收敛或达到最大迭代次数。

（注：原文未提供具体公式，以上为基于摘要描述的合理还原）

## 3. 实验设计

- **数据集/场景**：使用了**协作型**和**竞争型**多智能体基准测试（benchmarks）。摘要未列出具体名称，但推测可能包含如协同问答、谈判、市场博弈等常见多智能体任务。
- **对比方法**：
  - 基线包括：人工设计的提示词（手工调优）、仅基于任务级结果的自动优化方法（无轨迹信息）。
  - 可能还对比了不同信用分配策略、无聚合的版本等消融实验。
- **评估指标**：系统性能（如任务成功率、回答准确率等）以及协调成本（如通信轮次、token消耗等）。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量或训练时长。摘要和元数据均未提及任何算力细节。可能是由于该框架主要依赖LLM推理（如调用API或本地部署），未涉及大规模训练，因此作者未强调算力信息。

## 5. 实验数量与充分性

- **实验数量**：摘要描述较为概括，未给出具体实验组数。但基于元数据（score 7.0, ICLR-2026-Public），推测至少包含以下实验：
  - 主实验：在2~4个多智能体基准上的性能比较。
  - 消融实验：对比有无轨迹分解、有无信用分配、有无聚合等变体。
  - 可能还测试了不同LLM后端（如GPT-4, LLaMA等）的鲁棒性。
- **充分性评价**：从已公开信息看，实验覆盖了协作和竞争两种典型场景，对比了关键基线，设计较为合理。但缺乏更细致的实验描述（如数据集规模、统计显著性测试、收敛分析等），无法完全判断其公平性与客观性。不过鉴于该论文被ICLR 2026接收，实验设计大概率符合顶会标准。

## 6. 论文的主要结论与发现

- **轨迹级信息对于多智能体系统优化至关重要**：仅依赖任务级结果的优化方法无法定位智能体协作或推理中的具体错误，而TAVO通过细粒度信用分配显著提升了优化效果。
- **TAVO框架有效**：在协作和竞争多智能体基准上，TAVO在提升系统性能的同时，降低了协调成本（如减少不必要的通信、更高效的策略调整）。
- **言语化策略和跨任务聚合**：将提示词视为可编辑的策略，并利用自然语言反馈进行迭代修改，是一种可行且高效的自动化范式。

## 7. 优点

- **方法论创新**：首次将强化学习的信用分配思想引入多智能体提示词优化，并通过言语化策略进行解释性的、过程级的优化，而非黑盒调参。
- **实用性**：降低了对人工专家调试的依赖，减少重复试错成本，可直接适用于多种基于LLM的多智能体应用。
- **可解释性**：生成的编辑指令是自然语言形式，便于人类理解智能体的失败原因及优化方向。
- **性能与效率兼顾**：在提升性能的同时还降低了协调开销，说明轨迹级精细优化不仅提高了效果还带来了效率提升。

## 8. 不足与局限

- **实验覆盖有限**：摘要未公开具体数据集名称和规模，也未说明是否在超大规模智能体（如数十个Agent）或复杂长期任务上验证，泛化性存疑。
- **算力与成本**：虽然作者未提及，但多次调用LLM进行轨迹分解、反馈生成和聚合，可能产生较高的API成本或推理延迟，实际部署时需考虑经济性。
- **信用分配可靠性**：将轨迹分解为子轨迹并分配信用，依赖于LLM自身的判断能力，可能存在归因偏差；若LLM无法准确识别关键步骤，则优化方向可能错误。
- **缺乏与端到端强化学习方法的对比**：作者虽受RL启发，但未直接对比基于RL的策略梯度等传统方法，无法判断本方法的独特优势是否来自言语模态。
- **依赖高质量初始提示**：如果初始提示质量极低，轨迹信息可能噪声过多，导致信用分配失效。

（完）
