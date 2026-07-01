---
title: "The Agent's Marathon: Probing the Limits of Endurance in Long-Horizon Tasks"
title_zh: 智能体马拉松：探索长时域任务中的耐力极限
authors: "Wenhao Zheng, Xinyu Ye, Peng Xia, Fang Wu, Linjie Li, Weitong Zhang, Lijuan Wang, Yejin Choi, Yun Li, Huaxiu Yao"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=dAn82lpLx4"
tags: ["query:agent-traj"]
score: 7.0
evidence: 用于评估长时域任务中LLM智能体的可控基准
tldr: 针对LLM智能体在长时域任务中性能退化的问题，该论文提出TaskWeaver，一个基于规则的可控平台，能够生成难度和时长精确可调的基准任务。TaskWeaver抽象了所有工具接口，支持可调节的复杂度。实验表明，现有智能体在延长交互序列时性能急剧下降，揭示了它们缺乏耐力。该基准为测试智能体在长轨迹下的行为提供了标准化方法。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有基准无法充分评估智能体在长时域任务中的耐力退化问题。
method: 构建基于规则的可控基准生成平台TaskWeaver，支持调节任务难度和时长。
result: 实验表明智能体在长时域任务中性能退化严重，揭示了耐力局限性。
conclusion: TaskWeaver为可重复的智能体耐力测试提供了有效工具。
---

## Abstract
Large Language Model (LLM) agents, augmented with diverse tools, have shown impressive progress in domains such as scientific discovery and enterprise automation. Yet they remain brittle in long-horizon tasks that require extended sequences of interactions, where performance often deteriorates rapidly. Existing benchmarks provide only partial coverage of this challenge: manual or crowdsourced tasks are too short, tool-use benchmarks emphasize breadth over depth, and web-based evaluations rely on emergent rather than controllable complexity. To fill this gap, we introduce TaskWeaver, a rule-based, controllable platform for generating benchmark tasks with precisely adjustable difficulty and horizon length. At its core, TaskWeaver abstracts all tool use as file-read operations. This design choice removes superficial API complexities, allowing us to directly probe an agent’s core ability to reason and integrate intermediate results over long, dependent sequences. We instantiate the framework across three domains: document understanding and navigation, multi-modal information integration, and executable code analysis. Each domain probes a complementary aspect of agentic reasoning, and together they form a unified benchmark, LORE (Long-horizon Reasoning Evaluation). Empirical results show that even for the strongest models we tested, performance degrades significantly as task length and per-step complexity increase. Specifically, their accuracy approaches zero on tasks exceeding 120 steps, and on more challenging variants, performance collapses in fewer than 15 steps. These findings highlight long-horizon robustness as a central open challenge for future agent development.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：现有大语言模型（LLM）智能体在科学发现、企业自动化等场景中已取得显著进展，但在需要长序列交互的长时域任务中性能迅速退化，表现出“脆弱性”。现有基准存在局限性：人工/众包任务过短，工具使用基准重广度轻深度，基于Web的评估依赖突发而非可控的复杂度。因此，需要一个能精确调节任务难度和交互时长的可控基准平台。
- **背景**：该论文旨在填补对LLM智能体长时域耐力系统评估的空白，揭示当前模型在延长交互序列时的核心能力退化问题。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：提出 **TaskWeaver**，一个基于规则的可控基准生成平台，能够生成难度和时长精确可调的任务。其关键设计是将所有工具调用抽象为**文件读取操作（file-read operations）**，去除表面API复杂性，直接探测智能体对长依赖序列中中间结果的推理与整合能力。
- **关键技术细节**：
  - 任务生成基于规则，可独立调节**每步复杂度**和**总步数**。
  - 抽象所有工具接口为统一文件读取，避免API细节干扰，聚焦核心推理耐力。
  - 实例化为三个领域：文档理解与导航、多模态信息整合、可执行代码分析，共同构成统一基准 **LORE（Long-horizon Reasoning Evaluation）**。

## 3. 实验设计
- **数据集/场景**：采用LORE基准，包含三个子领域：
  1. 文档理解与导航
  2. 多模态信息整合
  3. 可执行代码分析
- **Benchmark**：LORE基准，任务是可控生成的，步长和复杂度可调。
- **对比方法**：测试了最强的现有LLM智能体（未明确列出具体模型名称，但摘要称“测试的最强模型”）。实验比较了不同任务长度和每步复杂度下的性能退化情况。

## 4. 资源与算力
- 论文中**未明确说明**使用的GPU型号、数量、训练时长等算力资源。仅提及“测试了最强模型”，但未披露计算硬件信息。这一点属于信息缺失。

## 5. 实验数量与充分性
- **实验数量**：论文在三个领域各设置了不同步长和复杂度的任务，具体组数未详细列出，但观察到了性能随步长增加而下降的趋势，并给出了关键拐点（如超过120步准确率趋近0，复杂变体下少于15步性能崩溃）。
- **充分性**：实验设计覆盖了不同领域、不同复杂度调节，但未进行详细的消融实验或对比多种智能体架构，也未报告统计显著性。整体上实验数量有限，但针对核心问题（耐力）的测量是有效的。公平性方面，因为任务基于规则生成，避免了数据泄露，但缺少跨模型广泛对比。

## 6. 主要结论与发现
- 即使是当前最强的LLM智能体，在长时域任务中性能也**严重退化**：当任务步数超过120步时，准确率接近0；在更具挑战性的变体中，步数少于15步性能即崩溃。
- 长时域鲁棒性（“耐力”）是未来智能体开发的核心待解决问题。

## 7. 优点：方法或实验设计亮点
- **可控性**：TaskWeaver通过规则生成任务，可独立调节步长和每步复杂度，实现了对长时域任务难度的精确控制，避免了现有基准的不可控性或覆盖不全。
- **抽象设计**：将所有工具调用简化为文件读取操作，剔除了API表面复杂性的干扰，直接测量智能体对长依赖推理的核心耐力。
- **多领域覆盖**：文档理解、多模态、代码分析三个领域互补，提升了基准的泛化性。
- **可重复性**：基于规则的设计确保了实验的可重复性和标准化。

## 8. 不足与局限
- **实验覆盖有限**：仅测试了“最强模型”，未与不同大小、不同架构（如开源模型、小型模型）全面对比，结论的普适性有待验证。
- **缺乏消融实验**：未分析不同抽象层次（如保留部分API复杂性）对结果的影响，也未深入探讨性能退化背后的具体原因（如记忆衰减、注意力漂移等）。
- **偏差风险**：任务基于规则生成，可能与真实长时域交互场景有差距，生态效度有限。
- **算力信息缺失**：未报告实验的硬件资源，不利于复现和成本评估。
- **应用限制**：只评估了“文件读取”抽象下的推理，未覆盖需复杂工具调用（如数据库查询、网络请求）的真实长时域任务。

（完）
