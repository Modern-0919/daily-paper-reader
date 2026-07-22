---
title: "Mobile-Agent-RAG: Driving Smart Multi-Agent Coordination with Contextual Knowledge Empowerment for Long-Horizon Mobile Automation"
title_zh: Mobile-Agent-RAG：通过上下文知识赋能驱动长期移动自动化的智能多智能体协调
authors: "Yuxiang Zhou, Jichang Li, Yanhao Zhang, Haonan Lu, Guanbin Li"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40241/44202"
tags: ["query:agent-errors"]
score: 8.0
evidence: 规划中的策略性幻觉和UI执行中的操作错误
tldr: 当前移动智能体在长期跨应用任务中成功率低，源于过度依赖静态内部知识导致规划幻觉和执行错误。本文提出Mobile-Agent-RAG，通过区分高层规划和低层UI操作的知识需求，分别提供策略性经验和精确指令，有效缓解计划幻觉和操作错误，提升了复杂移动任务的完成率。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40241/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1823, \"height\": 583, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40241/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1648, \"height\": 614, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40241/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 845, \"height\": 359, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40241/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1824, \"height\": 422, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40241/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 891, \"height\": 473, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40241/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 883, \"height\": 187, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40241/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1713, \"height\": 519, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40241/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 883, \"height\": 448, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40241/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 882, \"height\": 626, \"label\": \"Table\"}]"
motivation: 移动智能体在长期跨应用任务中失败率高，原因是过度依赖静态知识导致规划幻觉和操作错误。
method: 区分高层规划和低层操作的知识需求，分别提供策略性经验和精确指令。
result: 显著缓解了规划幻觉和UI操作错误，提升了长期移动任务的成功率。
conclusion: 上下文知识赋能有效提升了移动智能体在长期跨应用任务中的可靠性。
---

## Abstract
Mobile agents show immense potential, yet current state-of-the-art (SoTA) agents exhibit inadequate success rates on real-world, long-horizon, cross-application tasks. We attribute this bottleneck to the agents' excessive reliance on static, internal knowledge within MLLMs, which leads to two critical failure points: 1) strategic hallucinations in high-level planning and 2) operational errors during low-level execution on user interfaces (UI). The core insight of this paper is that high-level planning and low-level UI operations require fundamentally distinct types of knowledge. Planning demands high-level, strategy-oriented experiences, whereas operations necessitate low-level, precise instructions closely tied to specific app UIs. Motivated by these insights, we propose Mobile-Agent-RAG, a novel hierarchical multi-agent framework that innovatively integrates dual-level retrieval augmentation. At the planning stage, we introduce Manager-RAG to reduce strategic hallucinations by retrieving human-validated comprehensive task plans that provide high-level guidance. At the execution stage, we develop Operator-RAG to improve execution accuracy by retrieving the most precise low-level guidance for accurate atomic actions, aligned with the current app and subtask. To accurately deliver these knowledge types, we construct two specialized retrieval-oriented knowledge bases. Furthermore, we introduce Mobile-Eval-RAG, a challenging benchmark for evaluating such agents on realistic multi-app, long-horizon tasks. Extensive experiments demonstrate that  Mobile-Agent-RAG significantly outperforms SoTA baselines, improving task completion rate by 11.0% and step efficiency by 10.2%, establishing a robust paradigm for context-aware, reliable multi-agent mobile automation.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：当前最先进的移动智能体在处理真实世界的长期、跨应用任务时成功率不高。作者将这一瓶颈归因于智能体过度依赖多模态大语言模型（MLLM）内部的静态知识，导致两个关键失败点：高层规划中的策略性幻觉（strategic hallucinations）和低层UI执行中的操作错误（operational errors）。
- **整体含义**：本文提出了一种新的分层多智能体框架Mobile-Agent-RAG，通过双层次检索增强生成（RAG）来分别解决规划和执行中的知识不足问题，从而提升长期移动自动化的可靠性和效率。

## 2. 论文提出的方法论：核心思想、关键技术细节、算法流程
- **核心思想**：高层规划和低层UI操作需要根本不同类型的知识：规划需要策略性经验，而操作需要与具体应用UI紧密相关的精确指令。因此，框架分别引入Manager-RAG（规划层）和Operator-RAG（执行层）来检索外部知识。
- **关键技术细节**：
  - **分层多智能体架构**：包含Manager Agent（负责高层战略规划和子任务分解）和Operator Agent（负责将子任务转化为可执行的原子动作）。辅助模块包括Perceptor（细粒度视觉感知）、Action Reflector（动作结果反思）和Notetaker（信息聚合）。
  - **Manager-RAG**：在任务初始阶段，根据输入任务指令检索最相关的“任务指令-人工步骤”文档（来自知识库K<sub>MR</sub>），作为few-shot示例指导MLLM生成整体计划。检索使用Contriever-MSMARCO模型计算语义相似度。
  - **Operator-RAG**：在执行每个子任务时，根据当前子任务和应用名称，从应用特定的知识库K<sub>OR</sub>中检索最匹配的“子任务-截图-动作”三元组（top-1），辅助MLLM生成精确的原子动作（如点击坐标、滑动等）。
  - **算法流程**：迭代执行循环：Perceptor感知当前截图 → Manager-RAG指导计划生成 → Operator-RAG指导动作生成并执行 → 感知后截图 → Action Reflector评估结果 → Notetaker更新笔记。若连续错误则参考最近错误日志进行恢复。
- **知识库构建**：
  - Manager-RAG知识库：从Mobile-Eval-RAG基准中提取人工验证的任务指令和操作步骤。
  - Operator-RAG知识库：通过半自动方式收集智能体执行时产生的子任务、截图和动作，然后由人工验证和清理；为每个应用维护独立的检索库。

## 3. 实验设计：使用的数据集、基准、对比方法
- **数据集/基准**：作者构建了**Mobile-Eval-RAG**，包含50个多样且具有挑战性的任务，涵盖信息搜索、趋势查找、餐厅推荐、在线购物、旅行规划五类，平均16.9步，涉及2-3个应用。任务分为简单操作（前两类）和复杂操作（后三类）。
- **对比方法**：包括AutoDroid、AppAgent (Auto/Demo)、Mobile-Agent、Mobile-Agent-v2、Mobile-Agent-E、Mobile-Agent-E+Evo（均为开源移动智能体框架）。
- **推理模型**：Gemini-1.5-Pro（默认）、GPT-4o、Claude-3.5-Sonnet。
- **评估指标**：成功率（SR）、完成率（CR）、操作准确率（OA）、反思准确率（RA）、步数（Steps）、效率（Efficiency）。

## 4. 资源与算力
- 论文正文中**未明确说明**使用的GPU型号、数量或训练时长。仅提及使用Android Debug Bridge (ADB) 进行设备交互，以及使用Contriever-MSMARCO模型进行检索（该模型是轻量级检索模型，无需大量训练）。关于MLLM推理，作者使用的是商用API（如Gemini、GPT-4o），未披露具体的计算资源消耗。因此，论文在资源与算力方面信息不足。

## 5. 实验数量与充分性
- **实验数量**：主要对比实验包括：
  - 表1：单应用/多应用任务，与7个基线对比。
  - 表2：按任务复杂度（简单/复杂）细分对比。
  - 表3：在三个不同MLLM上对比。
  - 消融实验（图5、图6）：移除Manager-RAG或Operator-RAG，分析贡献。
  - 额外分析：图4展示不同MLLM下的性能增益来源。
- **充分性与客观性**：
  - 对比方法覆盖了单智能体和多智能体、基于探索/演示的多种类型，较为全面。
  - 报告了多个指标（CR、OA、RA、Steps、Efficiency、SR），从不同角度评估性能。
  - 消融实验验证了双RAG模块的互补作用。
  - 局限性：数据集仅50个任务，样本量偏小，可能不足以充分代表真实世界复杂性；未在真实用户或不同设备上进行测试；未讨论知识库覆盖不足时的退化情况。

## 6. 论文的主要结论与发现
- Mobile-Agent-RAG在多个指标上**显著优于**所有SoTA基线：任务完成率提升11.0%，步骤效率提升10.2%。
- 在复杂任务（如餐厅推荐、购物、旅行规划）上提升尤为明显：复杂操作任务CR从54.9%提升至74.2%。
- 双RAG组件各有侧重：Manager-RAG主要提升计划质量（减少规划幻觉），Operator-RAG主要提升操作精度（减少操作错误），二者互补。
- RAG对不同能力的MLLM均有提升，但对较弱模型（如Gemini-1.5-Pro）提升幅度更大，表明外部知识可以弥补模型推理能力的不足。
- 知识库（人工验证）优于自进化（Self-Evolution）策略，因为后者可能受限于模型自身的幻觉。

## 7. 优点：方法或实验设计上的亮点
- **方法论创新**：首次明确提出高层规划与低层执行需要不同类型知识，并分别设计RAG模块，形成双层次检索增强。这种知识分层设计避免了单一RAG的模糊性。
- **知识库构建**：结合自动收集和人工验证，保证了数据质量；为每个应用建立独立检索库，提高了检索精度。
- **实验设计**：
  - 创建专用于评估RAG移动智能体的基准Mobile-Eval-RAG，任务设计更贴合真实多应用场景。
  - 在不同复杂度、不同MLLM下进行对比，结论具有一般性。
  - 消融实验揭示了每个模块的具体贡献，并分析了错误类型分布（图6），增强了可解释性。

## 8. 不足与局限
- **基准规模有限**：仅50个任务，无法覆盖所有常见操作和意外情况，多样性可能不足。
- **知识库依赖人工**：构建知识库需要大量人工标注和验证，成本高，且难以自动扩展。对于新应用或新功能，无法快速覆盖。
- **未考虑UI变化**：基准假设应用UI相对稳定，实际中UI可能更新导致检索到的动作失效，论文未讨论此类适应性。
- **计算资源信息缺失**：未提供任何关于模型推理成本或训练开销的数据，难以评估实际部署所需资源。
- **未进行人机对比**：没有与人类操作进行对比，无法衡量绝对性能差距。
- **冷启动问题**：新任务或新应用缺乏足够的知识库条目时，系统性能会迅速下降，论文未给出缓解方案。

（完）
