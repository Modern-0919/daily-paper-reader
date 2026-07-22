---
title: "SiriuS: Self-improving Multi-agent Systems via Bootstrapped Reasoning"
title_zh: SiriuS：通过引导推理进行自我改进的多智能体系统
authors: "Wanjia Zhao, Mert Yuksekgonul, Shirley Wu, James Zou"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=IDSTtDw4Cs"
tags: ["query:agent-errors"]
score: 6.0
evidence: 构建高质量推理轨迹经验库用于自我改进
tldr: 针对多智能体系统依赖脆弱人工提示且优化困难的问题，提出SiriuS框架，通过保留导致成功结果的推理步骤构建经验库，以自改进和推理驱动方式优化多智能体系统，为自动错误修复和轨迹优化提供了新途径。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 多智能体系统依赖手动设计的提示和启发式，优化困难且缺乏训练数据。
method: 引入自改进框架SiriuS，构建高质量推理轨迹经验库，并通过库增强进行优化。
result: 经验库为多智能体系统提供了鲁棒的训练集，提升了性能。
conclusion: SiriuS通过自改进机制有效优化了多智能体系统，减少了对人工设计的依赖。
---

## Abstract
Multi-agent AI systems powered by large language models (LLMs) are increasingly applied to solve complex tasks. However, these systems often rely on fragile, manually designed prompts and heuristics, making optimization difficult. A key challenge in optimizing multi-agent systems is acquiring suitable training data for specialized agents. We introduce SiriuS, a self-improving, reasoning-driven optimization framework for multi-agent systems. Central to our approach is the construction of an experience library: a repository of high-quality reasoning trajectories. The library is built by retaining reasoning steps that lead to successful outcomes, providing a robust training set for optimizing multi-agent system. Additionally, we introduce a library augmentation procedure that refines unsuccessful trajectories, further enriching the library. SiriuS boosts performance by 2.86% to 21.88% on reasoning and biomedical QA and enhances agent negotiation in competitive settings. Our results show that SiriuS enhances multi-agent performance while generating reusable data for self-correction and self-play enhancement in the future.

---

## 论文详细总结（自动生成）

# 论文中文总结：SiriuS：通过引导推理进行自我改进的多智能体系统

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：当前基于大语言模型（LLM）的多智能体系统在处理复杂任务时，普遍依赖脆弱、人工设计的提示（prompts）和启发式规则，导致优化困难；同时，为专门化智能体获取高质量训练数据极为挑战。
- **整体含义**：旨在提出一种自我改进（self-improving）的推理驱动优化框架，减少对人工设计的依赖，使多智能体系统能够自主提升性能，并为未来的自我纠正和自我对弈提供可复用的数据。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：构建“经验库”（experience library）——一个高质量推理轨迹的仓库。通过保留导致成功结果的推理步骤，为多智能体系统提供稳健的训练集，并以自改进和推理驱动方式优化系统。
- **关键技术细节**：
  - **经验库构建**：在多智能体执行任务过程中，记录推理轨迹（reasoning trajectories），仅保留那些最终导向成功结果的步骤。
  - **库增强（library augmentation）**：对不成功的轨迹进行精炼（refine），将其转化为有用的训练数据，进一步丰富经验库。
  - **优化过程**：利用经验库中的高质量轨迹作为训练信号，迭代式地微调或引导智能体策略，实现自我改进。
- **算法流程（文字说明）**：
  1. 初始多智能体系统执行任务，收集所有推理轨迹及结果（成功/失败）。
  2. 筛选成功轨迹加入经验库；对失败轨迹执行精炼操作（如通过回溯或局部修正），尝试生成成功变体，扩充库。
  3. 利用当前经验库对多智能体系统进行优化（例如通过监督学习或强化学习）。
  4. 重复步骤1-3，直至性能收敛。

## 3. 实验设计
- **数据集/场景**：包括推理任务（reasoning）、生物医学问答（biomedical QA）以及智能体协商（agent negotiation）等竞争性场景。
- **Benchmark**：未明确给出具体benchmark名称，但涵盖了推理准确率、QA性能和协商成功率。
- **对比方法**：未详细列出，但对比基线应为基于手工提示的传统多智能体系统或其他固定提示方法。

## 4. 资源与算力
- **未明确说明**：论文摘要与元数据中未提及使用的GPU型号、数量或训练时长。需要指出这一点。

## 5. 实验数量与充分性
- **实验数量**：至少涉及三个不同的任务领域（推理、生物医学QA、协商），每个领域应有相应的消融实验（如有无经验库、是否使用库增强等）。摘要提到性能提升范围为2.86%至21.88%，说明实验覆盖了多个场景。
- **充分性与公平性**：由于未提供完整实验细节（如具体数据集大小、重复次数、统计显著性检验），难以完全判断。但领域多样性表明实验设计有一定覆盖。对比基线未明确，可能存在偏差风险。

## 6. 主要结论与发现
- SiriuS在推理和生物医学QA任务上实现了2.86%~21.88%的性能提升，在竞争性协商场景中也改善了智能体表现。
- 经验库为多智能体系统提供了鲁棒的训练集，减少了对手工设计的依赖。
- 库增强过程进一步提升了数据质量和系统性能。
- 该框架生成了可复用的数据，可用于未来的自我纠正和自我对弈增强。

## 7. 优点（方法和实验设计亮点）
- **方法亮点**：
  - 提出“经验库”概念，将成功推理轨迹结构化存储，实现训练数据的自动生成。
  - 库增强机制能够利用失败轨迹，减少数据浪费，提升训练效率。
  - 自我改进循环无需外部标注，具有良好扩展性。
- **实验设计亮点**：
  - 涵盖多个不同任务类型（推理、QA、谈判），验证泛化能力。
  - 性能提升幅度较大（最高21.88%），显示方法有效性。

## 8. 不足与局限
- **实验覆盖不足**：仅测试了三个领域，未包括更多实际应用场景（如代码生成、多模态任务等）。
- **偏差风险**：未列出对比的具体基线方法，难以判断相对于最新SOTA的提升；可能只对比了简单的手工基线。
- **应用限制**：依赖大语言模型作为基础，成本较高；经验库的构建和维护需要存储轨迹，可能带来资源开销。
- **消融实验**：摘要未提及是否对库增强、成功轨迹筛选等组件进行详细消融，可解释性有限。
- **算力未报告**：缺乏计算资源信息，难以评估可复现性。

（完）
