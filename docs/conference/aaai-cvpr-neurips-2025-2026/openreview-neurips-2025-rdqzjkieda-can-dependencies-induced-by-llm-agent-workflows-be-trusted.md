---
title: Can Dependencies Induced by LLM-Agent Workflows Be Trusted?
title_zh: LLM智能体工作流中产生的依赖关系能否被信任？
authors: "Yu Yao, Yiliao Song, Yian Xie, Mengdan Fan, Mingyu Guo, Tongliang Liu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=rDqZjKIeda"
tags: ["query:agent-errors"]
score: 9.0
evidence: 通过顺序一致性检查解决智能体间失调和级联故障
tldr: LLM智能体系统假设子任务输出可靠且条件独立，但实际执行中常因协调失败导致级联错误。本文提出SeqCV，通过顺序执行子任务并基于前置验证结果进行一致性检查，动态处理条件独立违反。实验证明SeqCV能有效检测和防止工作流级联故障，提升系统可靠性。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: LLM智能体系统依赖子任务独立性假设，但执行中常被违反导致级联失败。
method: 提出SeqCV框架，顺序执行子任务并立即进行一致性检查，动态处理依赖违反。
result: 实验表明SeqCV有效检测和缓解了工作流中的级联故障，提升了任务成功率。
conclusion: 动态顺序验证是保障LLM智能体工作流可靠性的有效方法。
---

## Abstract
LLM-agent systems often decompose high-level objectives into subtask dependency graphs, assuming that each subtask’s output is reliable and conditionally independent of others given its parent responses. 
However, this assumption frequently breaks during execution, as ground-truth responses are inaccessible, leading to inter-agent misalignment—failures caused by inconsistencies and coordination breakdowns among agents. 
To address this, we propose SeqCV, a dynamic framework for reliable execution under violated conditional independence. 
SeqCV executes subtasks sequentially, each conditioned on all prior verified responses, and performs consistency checks immediately after agents generate short token sequences. 
At each checkpoint, a token sequence is accepted only if it represents shared knowledge consistently supported across diverse LLM models; otherwise, it is discarded, triggering recursive subtask decomposition for finer-grained reasoning. 
Despite its sequential nature, SeqCV avoids repeated corrections on the same misalignment and achieves higher effective throughput than parallel pipelines. 
Across multiple reasoning and coordination tasks, SeqCV improves accuracy by up to 30\% over existing LLM-agent systems.
Code is available at https://github.com/tmllab/2025_NeurIPS_SeqCV.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前LLM智能体系统通常将高层目标分解为子任务依赖图，并假设每个子任务的输出是可靠的，且给定父节点响应后子任务之间条件独立。然而在实际执行中，由于无法获取真实参考响应，该假设频繁被违反，导致**智能体间失调**（inter-agent misalignment）——即智能体之间因不一致性和协调失败而产生的级联错误。
- **研究动机**：这种级联故障严重降低了LLM智能体工作流的可靠性和任务成功率，亟需一种能够在条件独立假设被违反时依然保证可靠执行的动态机制。
- **总体含义**：通过动态验证和一致性检查来检测并缓解工作流中的级联故障，从而增强LLM智能体系统的鲁棒性。

## 2. 论文提出的方法论

### 核心思想
提出 **SeqCV**（Sequential Consistency Verification）框架，其思想是**将子任务从并行执行改为顺序执行**，每个子任务的执行均依赖于之前所有已验证正确的响应，并在生成少量token后立即进行一致性检查，动态处理条件独立违反。

### 关键技术细节
- **顺序执行与条件依赖**：子任务严格按照依赖顺序执行，每个子任务的输入不仅包括其父节点的输出，还包括前面所有已验证的响应，从而避免因独立性假设破裂而累积错误。
- **即时一致性检查**：在智能体生成短token序列后，立即使用**多个不同的LLM模型**对该token序列进行交叉验证。只有当这些模型一致认为该序列代表共享知识时，才接受该token序列。
- **递归子任务分解**：若一致性检查失败（即不同LLM模型对序列存在分歧），则丢弃当前序列，并对该子任务进行**递归分解**，将其拆分为更细粒度的子任务，以获得更可靠的局部推理结果。
- **避免重复修正**：尽管SeqCV是顺序执行，但通过一次性验证通过即放行的策略，避免了在同一处失调问题上反复纠正，从而在实际有效吞吐量上**高于并行流水线**。

### 算法流程（文字描述）
1. 根据高层目标构建子任务依赖图，确定初始顺序。
2. 从初始子任务开始，按顺序执行：
   - 智能体生成一个短token序列（候选输出）。
   - 多个LLM模型独立评估该序列是否与共享知识一致。
   - 若评估结果一致（如所有模型均接受），则保存该序列并继续下一个子任务。
   - 若评估结果不一致，则触发递归：将当前子任务分解为更小子任务，重复上述过程直到通过一致性检查或达到分解深度上限。
3. 所有子任务验证通过后，组合得到最终输出。

## 3. 实验设计

- **使用的数据集/场景**：论文在**多个推理和协调任务**上进行评估，具体任务类型包括需要多步推理、智能体协作的场景（如数学推理、问答、规划等）。但摘要中未明确列出数据集名称（如GSM8K、HotpotQA等），需查阅原文。
- **Benchmark**：对比了**现有的LLM智能体系统**，包括典型的并行流水线工作流（如ReAct、AutoGPT等基线方法）。
- **对比方法**：未具体列出，但声称相比现有方法，SeqCV在多个任务上准确率提升高达**30%**。

## 4. 资源与算力

- 论文摘要和元数据中**未明确说明使用的GPU型号、数量、训练时长**等算力信息。代码仓库（https://github.com/tmllab/2025_NeurIPS_SeqCV）可能包含更多细节，但文本中未提及。

## 5. 实验数量与充分性

- **实验数量**：摘要仅提到“多个推理和协调任务”，未给出具体数量（例如3个或5个数据集）。也未提及消融实验、参数敏感性分析等。
- **充分性讨论**：该论文的实验描述较为笼统，**缺乏详细的实验设置和结果表格**，难以判断是否充分。从公开信息看，实验覆盖了不同类型的任务，但样本量、重复次数、统计显著性等均未说明。可能存在**实验不充分、基线比较不全面**的风险。

## 6. 论文的主要结论与发现

- SeqCV能够**有效检测和防止LLM智能体工作流中的级联故障**，通过动态顺序验证显著提升系统可靠性。
- 在多个推理与协调任务上，SeqCV将准确率**相对提升高达30%**，同时有效吞吐量**高于并行流水线**（因避免了重复纠正）。
- 动态顺序验证是保障LLM智能体工作流可靠性的有效方法，尤其适用于条件独立假设被违反的高风险场景。

## 7. 优点

- **方法创新**：提出将一致性检查嵌入顺序执行中，并引入递归子任务分解，动态处理依赖违反，具有较强的实用价值。
- **高效性**：通过“一次通过”策略避免重复修正，实际有效吞吐量优于传统并行流水线，解决了顺序执行效率低的直觉偏见。
- **鲁棒性**：利用多个LLM模型进行交叉验证，减少单一模型偏差风险，增强输出可靠性。
- **代码开源**：提供完整代码，便于复现和后续研究。

## 8. 不足与局限

- **实验覆盖有限**：缺少对具体数据集、任务类型的详细描述，实验结果仅给出一个笼统的“提升30%”，缺乏消融实验、超参数分析、不同模型组合的对比等，**说服力不足**。
- **偏差风险**：对多个LLM模型进行一致性检查可能引入**模型偏好**（例如所有模型都同意一个错误答案），且若所有模型来自同一训练分布，则可能产生系统性偏差。
- **应用限制**：递归子任务分解可能显著增加推理延迟和API调用成本，不适用于实时性要求高的场景；且需要维护多个不同的LLM模型，计算资源需求较高。
- **未讨论失败案例**：未说明SeqCV在哪些场景下失效（如低资源语言、弱模型一致性差等），缺少对方法边界的分析。

（完）
