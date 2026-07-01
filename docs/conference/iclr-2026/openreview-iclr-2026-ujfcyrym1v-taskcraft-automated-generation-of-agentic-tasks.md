---
title: "TaskCraft: Automated Generation of Agentic Tasks"
title_zh: TaskCraft：代理任务的自动生成
authors: "Dingfeng Shi, Jingyi Cao, Qianben Chen, Weichen Sun, Weizhen Li, Hongxuan Lu, Fangchen Dong, Tianrui Qin, King Zhu, Minghao Liu, Yuchen Eleanor Jiang, Jian Yang, Ge Zhang, Jiaheng Liu, Changwang Zhang, Jun Wang, Wangchunshu Zhou"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=UJFCyrYM1V"
tags: ["query:agent-traj"]
score: 7.0
evidence: 自动生成代理任务用于轨迹采样和评估
tldr: 代理评估基准依赖人工标注，扩展性有限。本文提出TaskCraft，首个自动化任务生成流程，通过深度和宽度扩展复杂化原子任务，结合拒绝采样和LLM验证。生成的任务支持轨迹采样和端到端训练，为代理轨迹测试提供可扩展的任务源。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有代理基准可扩展性受限，人工标注成本高。
method: TaskCraft通过深度和宽度扩展原子任务，并利用拒绝采样和LLM分析生成可验证的多工具代理任务。
result: 生成的任务支持端到端训练，在多个基准上提升代理性能。
conclusion: TaskCraft为代理评估提供了可扩展的任务生成方法，间接促进轨迹测试。
---

## Abstract
Agentic tasks, which require multistep problem solving with tool use and adaptive reasoning, are becoming increasingly central to the advancement of NLP and AI. Although benchmarks such as GAIA and BrowseComp have advanced agent evaluation, their scalability remains limited by the high cost of human annotation. We introduce TaskCraft, the first automated workflow for generating scalable, multitool, and verifiable agentic tasks of difficulty. TaskCraft progressively complexifies atomic tasks through depth-based and width-based extensions, with incremental validation via rejection sampling and LLM-based linguistic analysis, ensuring both scalability and efficiency. The generated tasks enable trajectory sampling within state-of-the-art workflows, supporting end-to-end SFT and RL training. Experimental results on multiple LLMs show that TaskCraft data substantially improves multi-hop reasoning and agentic capabilities. Further scaling with TaskCraft tasks and applying RL training yields additional gains, achieving state-of-the-art performance on four agentic benchmarks. The resulting dataset comprises 41k tool-intensive tasks across varied difficulty levels, including 12.6k tool-interaction trajectories and 5k multihop decompositions.

---

## 论文详细总结（自动生成）

# TaskCraft：代理任务的自动生成 — 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有智能体（Agent）评估基准（如GAIA、BrowseComp）依赖人工标注，成本高、可扩展性有限，难以生成大规模、多工具、可验证的代理任务。
- **整体含义**：自动化生成高质量、多步骤、工具导向的代理任务，既能用于轨迹采样评估，也能用于端到端训练（SFT和RL），从而推动NLP和AI中代理能力的提升。

## 2. 论文提出的方法论
- **核心思想**：TaskCraft是首个自动化任务生成流程，通过**深度扩展**（depth-based）和**宽度扩展**（width-based）逐步复杂化原子任务，并借助拒绝采样（rejection sampling）和基于LLM的语言分析进行增量验证，确保生成任务的可扩展性与质量。
- **关键技术细节**：
  - 从简单原子任务出发，通过增加任务步骤（深度）或引入更多工具/分支（宽度）来构建复杂多步骤任务。
  - 每一步扩展后使用拒绝采样过滤无效或不可执行的任务。
  - 利用LLM进行语言分析和验证，确保任务描述清晰、约束合理、答案可验证。
- **算法流程**（文字说明）：
  1. 定义原子任务（单步、单工具）。
  2. 对原子任务进行深度扩展（链式步骤）和宽度扩展（并行或分支工具调用）。
  3. 对扩展后的任务进行拒绝采样（运行模拟代理，筛选可完成的任务）。
  4. 对通过样本进行LLM语义验证（检查任务逻辑、工具使用合理性）。
  5. 输出最终任务集，包含任务描述、期望答案、工具交互轨迹等。

## 3. 实验设计
- **使用的数据集/场景**：TaskCraft自身生成了包含**41k个工具密集型任务**的数据集，涵盖不同难度级别，其中包括**12.6k条工具交互轨迹**和**5k个多步分解**。
- **Benchmark**：在四个智能体基准（agentic benchmarks）上评测，具体基准名称未在摘要中列出（推测包括GAIA、BrowseComp等）。
- **对比方法**：对比了不同LLM在未使用/使用TaskCraft数据训练后的性能，以及不同训练策略（SFT、RL）的效果。

## 4. 资源与算力
- **文中未明确说明**：摘要及元数据中未提及使用的GPU型号、数量、训练时长等具体算力信息。仅提到训练在多个LLM上进行，但未提供硬件细节。

## 5. 实验数量与充分性
- **实验组数**：实验覆盖了：
  - 多个基础LLM上的SFT和RL训练。
  - 四种不同代理基准的评测。
  - 不同难度的TaskCraft任务。
  - 可能包含消融实验（如对比深度扩展与宽度扩展效果、使用拒绝采样的影响等），但摘要未详细展开。
- **充分性评价**：从摘要看，实验数量较为充分，覆盖了从数据生成到下游训练的完整流程，并在多个基准上取得了最先进结果，具有较好的说服力。但缺乏详细的消融实验和跨基准对比的表格，因此客观性需依赖全文。

## 6. 论文的主要结论与发现
- TaskCraft生成的数据能够显著提升LLM的多跳推理和代理能力。
- 使用TaskCraft任务进行扩展训练，并结合强化学习（RL），可在四个代理基准上达到**最先进（state-of-the-art）性能**。
- 生成的数据集规模大（41k任务）且具有多样性，支持轨迹采样和端到端训练，间接促进了代理轨迹的测试与评估。

## 7. 优点
- **自动化与可扩展性**：第一个全自动代理任务生成流程，大幅降低人工标注成本，易于扩展至更大规模。
- **多工具、可验证**：生成的任务要求多工具协同使用，且答案可自动验证，适合评估和训练。
- **训练直接受益**：生成的任务可直接用于SFT和RL训练，并带来显著性能增益，实用价值高。
- **数据丰富**：提供大量轨迹和分解，可用于轨迹级测试。

## 8. 不足与局限
- **实验细节缺失**：摘要中未列出具体基准名称、对比方法、消融实验、以及任务质量的人工评估结果，结论的坚实度需全文验证。
- **算力成本未说明**：缺乏对任务生成和训练的计算资源描述，难以评估实际复现成本。
- **生成质量依赖LLM**：拒绝采样和语言分析环节依赖现有LLM，可能继承其偏差或限制。
- **应用限制**：任务生成仅限于当前设定的工具集和领域，跨领域泛化性未知；对复杂现实世界任务的覆盖可能不足。

（完）
