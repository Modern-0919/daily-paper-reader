---
title: "HalluClean: A Unified Framework to Combat Hallucinations in LLMs"
title_zh: HalluClean：对抗大语言模型幻觉的统一框架
authors: "Yaxin Zhao, Yu Zhang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40926/44887"
tags: ["query:agent-errors"]
score: 6.0
evidence: 检测和纠正LLM文本中的幻觉
tldr: 大语言模型常产生幻觉内容，影响事实可靠性。本文提出HalluClean，轻量级任务无关框架，通过推理增强范式将过程分解为规划、执行和修订阶段，识别并修正无依据声明。无需外部知识或监督检测器，在多个任务上取得良好效果，为幻觉纠正提供了通用解决方案。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40926/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1800, \"height\": 1043, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40926/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1851, \"height\": 485, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40926/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1846, \"height\": 490, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40926/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1649, \"height\": 569, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40926/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1715, \"height\": 707, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40926/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1715, \"height\": 784, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40926/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1777, \"height\": 282, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40926/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1419, \"height\": 417, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40926/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 852, \"height\": 242, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40926/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 858, \"height\": 243, \"label\": \"Table\"}]"
motivation: LLM常产生幻觉内容，损害事实可靠性，需要轻量级任务无关的检测纠正框架。
method: 采用推理增强范式，将过程分解为规划、执行和修订阶段，使用最小任务提示实现零样本泛化。
result: 在多项任务上有效检测并纠正幻觉，无需外部知识或监督检测器。
conclusion: HalluClean为LLM幻觉提供了轻量级且任务无关的通用检测纠正方案。
---

## Abstract
Large language models (LLMs) have achieved impressive performance across a wide range of natural language processing tasks, yet they often produce hallucinated content that undermines factual reliability. To address this challenge, we introduce HalluClean, a lightweight and task-agnostic framework for detecting and correcting hallucinations in LLM-generated text. HalluClean adopts a reasoning-enhanced paradigm, explicitly decomposing the process into planning, execution, and revision stages to identify and refine unsupported claims. It employs minimal task-routing prompts to enable zero-shot generalization across diverse domains, without relying on external knowledge sources or supervised detectors. We conduct extensive evaluations on five representative tasks—question answering, dialogue, summarization, math word problems, and contradiction detection. Experimental results show that HalluClean significantly improves factual consistency and outperforms competitive baselines, demonstrating its potential to enhance the trustworthiness of LLM outputs in real-world applications.

---

## 论文详细总结（自动生成）

# HalluClean：对抗大语言模型幻觉的统一框架——详细中文总结

## 1. 核心问题与整体含义
- **研究动机**：大语言模型（LLM）在多种自然语言处理任务中表现优异，但经常生成事实不准确或逻辑矛盾的“幻觉”内容，损害了输出的可靠性和可信度。
- **现有方法局限**：检索增强生成依赖外部知识源的可用性和准确性；监督检测方法需要大量人工标注，泛化能力弱且成本高；多数工作仅针对单一任务。
- **论文贡献**：提出HalluClean——一个轻量级、任务无关的零样本框架，无需外部知识或监督信号，通过结构化推理同时实现幻觉检测与修正，提升LLM输出的事实一致性。

## 2. 方法论：核心思想与关键技术
- **核心思想**：采用“推理增强”范式，将幻觉处理显式分解为规划、执行和修正三个阶段，引导模型进行系统性思考，再基于推理结果做针对性修正。
- **技术流程**（四步结构化提示）：
  1. **任务导向规划（Task-oriented Planning）**：根据任务描述和输入，让模型制定检测计划，明确验证策略（如识别关键实体、时间、动作）。
  2. **计划引导推理（Plan-guided Reasoning）**：模型按计划逐步执行验证，生成详细的推理链。
  3. **最终判断（Final Judgment）**：基于推理链输出二元结论（是否存在幻觉）。
  4. **内容修正（Content Refinement）**：若检测到幻觉，则依据推理链中的具体分析，生成修订版本。
- **任务路由提示**：为不同任务（QA、对话、摘要、数学问题、矛盾检测）设计极简的任务描述，使框架能零样本泛化。
- **关键特征**：无需外部知识、无需监督数据、支持开源LLM（如Llama-3），适用于隐私敏感场景。

## 3. 实验设计
- **数据集与场景**：
  - **HaluEval**：QA、知识对话、文本摘要中的幻觉样本。
  - **UMWP**：不可解数学文字题引发的幻觉。
  - **ChatProtect**：模型自我矛盾幻觉。
  - **HaluBench**：医学（CovidQA、PubMedQA）和金融（FinanceBench）领域幻觉。
- **对比方法**：
  - **直接询问（Direct Ask）**：多种LLM（GPT-3.5-turbo、GPT-4o-mini、Llama-3-70B、DeepSeek-V3、DeepSeek-R1）。
  - **现有基线**（统一用GPT-3.5-turbo骨干）：Step-by-Step、Plan-and-Solve、SelfCheckGPT、ChatProtect。
- **评估指标**：
  - **检测**：F1分数、准确率（与人工标注对比）。
  - **修正**：幻觉减少率（修正后幻觉比例下降）、修正成功率（BERTScore≥0.85的修正占比，MWP用精确匹配）。

## 4. 资源与算力
- 论文**未明确说明**使用的GPU型号、数量或训练时长。实验主要调用各LLM的API（GPT-3.5-turbo、GPT-4o-mini、DeepSeek-V3等）或本地部署Llama-3-70B，属于推理阶段，未提及微调成本。因此无法量化具体算力需求。

## 5. 实验数量与充分性
- **实验覆盖**：在5种任务类型（QA、总结、对话、数学、矛盾）上进行了检测与修正评估；在医学/金融领域额外验证；在检索增强和跨语言（中文）场景测试；对多个骨干LLM进行适配性分析。
- **消融实验**：逐步加入任务路由提示和结构化推理模块，验证各自贡献。
- **充分性与公平性**：
  - 全面对比了多种骨干模型和现有基线，骨干统一（GPT-3.5-turbo）下比较，控制变量。
  - 使用标准公开数据集和公认指标。
  - 但未对识别出的失败案例进行定性分析，也未计算修正的运行时开销。

## 6. 主要结论与发现
- **HalluClean显著优于所有基线**：在检测F1和准确率上，于QA（67.8% F1）、对话（74.3%）、摘要（65.9%）、数学（80.3%）、矛盾（87.0%）均取得最佳或接近最佳。
- **修正效果突出**：幻觉减少率在多数任务超过其他方法（如QA 72.5%，对话89.0%），修正成功率也较高（QA 25.5%等，部分任务修正质量受限于骨干）。
- **模块贡献互补**：任务路由和结构化推理单独或组合均能提升性能，后者贡献更大（尤其中复杂任务如矛盾检测）。
- **领域鲁棒性**：在医学、金融领域也取得最佳检测性能。
- **与检索增强兼容**：在QA上，HalluClean+检索增强获得80.4% F1，表明可互补外部知识。
- **跨语言泛化**：在中文幻觉基准上也优于直接询问。

## 7. 优点
- **轻量且任务无关**：纯提示驱动，无需训练或外部知识，即插即用。
- **结构化推理增强可解释性**：检测过程提供推理轨迹，便于人工审核和信任。
- **模块化设计**：检测与修正可独立使用，修正基于分析而非盲目重写。
- **广泛适用性**：支持多种开源LLM，隐私友好，覆盖主流幻觉场景。
- **实验设计全面**：涵盖多种任务、骨干、领域和扩展场景，验证了框架的鲁棒性和泛化能力。

## 8. 不足与局限
- **未报告计算开销**：未提及推理时延或token消耗，可能影响部署效率评估。
- **修正质量依赖骨干能力**：修正成功率在部分任务（如QA仅25.5%）偏低，表明弱模型修正能力有限。
- **跨语言验证有限**：仅测试中文两个小型基准，缺乏多语言系统性评估。
- **修正评估阈值主观**：BERTScore阈值0.85可能不够鲁棒，未提供敏感性分析。
- **缺乏错误分析**：未讨论检测或修正失败的典型情况，不利于理解方法边界。
- **仅针对生成文本**：未考虑多模态或知识图谱等更复杂的输入输出形式。

（完）
