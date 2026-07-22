---
title: "Scope Delineation Before Localization: A Two-Stage Framework for Enhancing Failure Attribution in Multi-Agent Systems"
title_zh: 先划定范围再定位：增强多智能体系统故障归因的两阶段框架
authors: "Kai Sun, Wenqiang Li, Bo Dong, Yuxin Lin, Jingyao Zhang, Bin Shi"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40594/44555"
tags: ["query:agent-errors"]
score: 9.0
evidence: 多智能体系统中的故障归因
tldr: 针对多智能体系统中故障归因困难的问题，本文提出了一种两阶段框架，先划定故障范围再定位具体步骤，从而减轻LLM的推理负担，实现更精准的归因。实验证明该方法能有效处理长日志并提升归因准确率，增强了系统的可解释性和鲁棒性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40594/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 883, \"height\": 602, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40594/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1626, \"height\": 949, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40594/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 863, \"height\": 668, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40594/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 841, \"height\": 509, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40594/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 690, \"height\": 137, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40594/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 868, \"height\": 354, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40594/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1762, \"height\": 535, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40594/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 871, \"height\": 248, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40594/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 744, \"height\": 249, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40594/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 830, \"height\": 213, \"label\": \"Table\"}]"
motivation: 现有LLM归因方法在处理长日志和缺乏专家知识时面临挑战，需要更高效的故障归因方法。
method: 提出先划定故障范围再定位故障步骤的两阶段框架，将归因过程解耦。
result: 该方法提升了故障归因的准确率，能够处理复杂的长日志场景。
conclusion: 两阶段框架有效减轻LLM推理负担，实现更精准的故障归因，增强系统鲁棒性。
---

## Abstract
Large language models (LLMs) are seeing growing adoption in multi-agent systems. In these systems, efficient failure attribution is critical for ensuring robustness and interpretability. Current LLM-based attribution methods often face challenges with lengthy logs and lacking expert knowledge. Drawing inspiration from human debugging strategies, we propose an automated failure attribution framework, Scope Delineation Before Localization, which operates in two key stages: (1) identifying the failure scope and (2) pinpointing the failure step. By decoupling failure attribution into the two stages, our approach alleviates the reasoning workload of LLMs, enabling more precise failure attribution. To support scope delineation, we further introduce two strategies: Stepwise Scope Delineation and Expertise-Assisted Scope Delineation. Experiments on the Who&When dataset validate the efficacy of our two-stage framework, demonstrating substantial improvements over prior methods (up to 24.27% on step-level accuracy).

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **问题**：在大规模多智能体系统（MAS）中，故障归因（即定位导致系统失败的根因智能体和具体步骤）是确保系统鲁棒性和可解释性的关键。然而，现有基于LLM的归因方法（如ALL AT ONCE、STEP BY STEP、BINARY SEARCH）在处理长执行日志时性能急剧下降（最高step accuracy仅16.59%），且通用LLM缺乏MAS领域的专有知识，导致归因精度不足。
- **动机**：受人类程序员调试策略启发——经验丰富的开发者会先根据异常现象缩小可疑区域（如超时检查循环、未定义变量检查声明范围），再精确定位错误行。论文认为将故障归因解耦为“范围划定”和“精确定位”两个阶段，可以减轻LLM的推理负担，提升归因准确率。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出两阶段框架 **SDBL（Scope Delineation Before Localization）**，先识别包含故障的步骤范围（Scope Set S），再基于该范围精确定位故障步骤。
- **关键技术细节**：
  - **阶段一：范围划定（Scope Delineation）**。通过两种策略构建范围集合S，并设超参数N（S的最大大小）：
    - **Stepwise Scope Delineation（SSD）**：基于STEP BY STEP方法，但遇到疑似故障步骤时不终止，而是将其加入S并继续迭代，直到|S|=N或日志末尾。使用LLM的固有知识逐步判断。
    - **Expertise-Assisted Scope Delineation（EASD）**：利用预定义的专家范式识别故障易发步骤，包括：
      - **Overstep范式**：检测智能体越权动作（如执行者发出终止命令）。
      - **Loop范式**：检测重复相似命令（如指挥官反复发送相同指令）。这些范式通过提示工程融入LLM。S由两种范式的检测结果取并集得到，若超过N则截断。
  - **阶段二：故障定位（Localization）**。在原始日志L中附加提示，突出显示S中的高风险步骤，再让LLM综合判断并输出最终故障智能体和故障步骤。
- **算法流程（文字说明）**：
  1. 输入执行日志L = {s1, s2, ..., sn}。
  2. 根据选择策略（SSD或EASD）生成范围集S，|S| ≤ N。
  3. 构造增强提示：包含完整日志L与重点关注的步骤S。
  4. 调用LLM输出故障智能体角色和故障步骤索引(rt, t)。

### 3. 实验设计：数据集、基准、对比方法

- **数据集**：使用 **Who&When** 数据集（Zhang et al., 2025），包含两个子集：
  - **Hand-Crafted (HC)**：54个日志，平均50步，涉及6.59个角色。
  - **Algorithm-Generated (AG)**：126个日志，平均8.72步，3.63个角色。
  每个样本包含执行日志和标注的故障步骤、负责智能体及原因。
- **对比方法**：ALL AT ONCE（单次全日志分析）、STEP BY STEP（逐步骤逐步判断）、BINARY SEARCH（递归二分查找）。
- **评估指标**：
  - **Agent Accuracy**：正确识别故障智能体的比例。
  - **Step Accuracy**：正确识别故障步骤的比例。
  - **Hit@K**：用于评估范围划定质量——真实故障(ri, ti)是否在S内。
- **LLM Backbone**：主要使用GPT-4o；额外使用DeepSeek-R1、DeepSeek-V3验证泛化性。
- **超参数**：N在HC上最优为5，在AG上设为3。

### 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等信息。实验仅涉及LLM推理（无模型训练），仅报告了token消耗量（表5）。例如，SDBL (EASD)在HC上的平均token消耗为24657 token，低于STEP BY STEP（87720 token），略高于ALL AT ONCE（17106 token）。因此无法评估具体硬件成本，但可推断推理对算力需求较低。

### 5. 实验数量与充分性

- **实验组数**：共进行了多组对比实验：
  - 主实验结果（表2）比较6种方法在HC和AG上的agent/step accuracy。
  - 不同LLM对比（表3）使用3种LLM和6种方法，共18组。
  - Hit@K比较（表4）在HC上对4种方法/变体评估5个K值。
  - Token消耗比较（表5）报告5种方法的平均token。
  - 不同日志长度性能分析（图3）将HC样本按步数分为6个等级，展示6种方法的趋势。
  - 消融实验（表6）针对EASD的Overstep和Loop范式进行移除研究。
  - 案例研究（图4）展示两个具体示例。
- **充分性判断**：实验覆盖了不同日志长度、不同LLM、多种指标、消融分析，设计全面，对比客观（基线方法来自同一数据集和相同LLM backbone）。但缺乏对更大规模数据或跨领域的验证。

### 6. 论文的主要结论与发现

- **核心发现**：两阶段框架SDBL显著优于所有基线方法。在HC上，SDBL (EASD)的step accuracy达到27.78%，比最佳基线高出24.27%（ALL AT ONCE仅3.51%）；在AG上达到33.33%，提升16.74%。
- **范围划定的重要性**：即使使用随机范围（RSD），性能仍超过基线，表明任何缩小搜索范围的策略都有帮助。
- **专家范式的有效性**：EASD在大多数场景下优于SSD，尤其在长日志中；消融实验显示移除Loop或Overstep范式均导致性能下降，其中Loop更为关键。
- **长日志鲁棒性**：基线方法在日志超过24步时step accuracy接近0，而SDBL在长日志上保持良好性能。
- **通用性**：在GPT-4o、DeepSeek-R1、DeepSeek-V3上均能取得一致改进，表明框架不依赖于特定LLM。

### 7. 优点

- **方法论创新**：将故障归因分解为“范围划定”和“精确定位”，模拟人类调试策略，有效降低LLM推理负担。
- **专家知识集成**：提出Overstep和Loop两种简单但高频的故障范式，通过提示工程增强通用LLM，无需微调。
- **实验设计全面**：不仅比较精度，还分析Hit@K、token消耗、日志长度影响、跨模型泛化，并进行消融和案例研究，论证充分。
- **实用性**：SDBL (EASD)在token消耗上仅略高于单次分析的方法（ALL AT ONCE），但精度大幅提升，成本效率高。

### 8. 不足与局限

- **局限性**：
  - **专家范式覆盖有限**：仅考虑了Overstep和Loop两种模式，实际MAS故障可能包含其他类型（如死锁、超时、数据不一致），需进一步扩展。
  - **超参数N依赖人工调优**：不同子集需手动选择最优N（HC用5，AG用3），未讨论自适应设置方法。
  - **仅依赖推理阶段**：未尝试对LLM进行微调或使用强化学习提升归因能力，可能仍有提升空间。
  - **数据集规模较小**：HC仅54个样本，统计显著性有限；AG为算法生成，与现实场景可能存在差距。
  - **未涉及安全与公平性**：未讨论故障归因中的偏差风险（如特定角色或步骤被过度关注）或系统安全影响。
  - **计算资源未明确**：虽然token消耗低，但未提供实际GPU推理时间，难以评估实时性。

（完）
