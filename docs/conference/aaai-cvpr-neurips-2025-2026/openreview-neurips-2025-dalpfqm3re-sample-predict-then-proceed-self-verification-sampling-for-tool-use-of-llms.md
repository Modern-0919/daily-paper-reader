---
title: "Sample, Predict, then Proceed: Self-Verification Sampling for Tool Use of LLMs"
title_zh: 采样、预测、然后执行：工具使用的LLM自验证采样方法
authors: "Shangmin Guo, Omar Darwiche Domingues, Raphaël Avalos, Aaron Courville, Florian Strub"
date: 2025-05-03
pdf: "https://openreview.net/pdf?id=DALpFQM3rE"
tags: ["query:agent-errors"]
score: 9.0
evidence: 函数调用中的错误与幻觉缓解，涉及工具选择与参数错误
tldr: 该论文针对LLM在有状态环境中的工具使用问题，提出动态建模方法DyMo，使模型能预测动作后的未来状态。在此基础上，集成自验证采样策略，显著减少了工具调用中的幻觉和参数错误。在Berkeley函数调用排行榜V2上成功率和pass^k指标均有提升，直接解决了工具选择与参数错误问题。
source: NeurIPS-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 现有测试时计算策略在环境中的重复试验不实用，需要新方法减少工具调用错误。
method: 提出DyMo方法，在函数调用后增加状态预测能力，并结合自验证采样。
result: 在Berkeley函数调用排行榜V2上提高了成功率，减少了幻觉。
conclusion: 自验证采样有效提升了LLM工具使用的可靠性。
---

## Abstract
Tool use in stateful environments presents unique challenges for large language models (LLMs), where existing test-time compute strategies relying on repeated trials in the environment are impractical.
We propose dynamics modelling (DyMo), a method that augments LLMs with a state prediction capability alongside function calling during post-training.
This enables LLMs to predict the future states of their actions through an internal environment model.
On the Berkeley Function Calling Leaderboard V2, DyMo improves success rates and significantly reduces hallucinations.
We further integrate the internal environment model into self-verification sampling (SVS), and show that this substantially improves pass^k over number of trials k, and allows the model to refuse unreliable outputs.
Together, DyMo and SVS greatly enhance the effectiveness and reliability of LLMs for tool use.
We believe this work charts a path towards scalable planning RL methods for LLM inference without repeatedly querying the oracle environment.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：大型语言模型（LLM）在**有状态环境**（stateful environments）中执行工具调用时，面临频繁的**工具选择错误**和**参数幻觉**问题。
- **现有不足**：传统的测试时计算策略（test-time compute）依赖在环境中进行重复试验，这在有状态环境中**不切实际**（例如每次试错可能改变状态，导致不可逆后果）。
- **研究动机**：需要一种无需反复查询真实环境就能提高 LLM 工具调用可靠性的方法，减少幻觉和错误。

## 2. 方法论：核心思想、关键技术细节与流程

### 2.1 核心思想
- 提出 **DyMo（Dynamics Modelling）**，在 LLM 后训练阶段引入**状态预测能力**，使其能够通过内部环境模型预测动作后的未来状态。
- 进一步将内部环境模型集成到**自验证采样（SVS）** 中，通过多次采样和自验证提升输出可靠性，并允许模型**拒绝不可靠的输出**。

### 2.2 关键技术细节
- **后训练阶段增强**：在标准函数调用训练基础上，增加对动作后环境状态的预测任务。
- **内部环境模型**：LLM 学习一个隐式的环境转移模型，无需外部模拟器。
- **自验证采样（SVS）**：
  - 对于给定输入，模型生成多个候选工具调用序列。
  - 利用内部环境模型模拟每个候选序列的执行结果。
  - 选择与自验证结果一致的输出，必要时可以拒绝所有不可靠结果。

### 2.3 算法流程（文字说明）
1. **训练阶段**：在标准函数调用数据上加入状态预测标签，通过多任务学习同时训练工具调用和状态预测。
2. **推理阶段**：
   - 对输入问题，模型生成 k 个候选工具调用方案。
   - 对每个候选方案，模型内部模拟其执行后的状态。
   - 比较模拟状态与预期目标，保留一致性最高的方案；若无一致方案，则拒绝回答或返回默认安全输出。

## 3. 实验设计

### 3.1 数据集/场景与 Benchmark
- **Benchmark**：伯克利函数调用排行榜 V2（Berkeley Function Calling Leaderboard V2）。
- **场景**：有状态环境下的工具调用，涉及多步操作和状态变化。

### 3.2 对比方法
- 未在元数据中详细列出对比基线，但从摘要推断可能包括：
  - 标准 LLM 直接调用（无自验证）
  - 其他测试时计算策略（如 repeated trials）
  - 可能还有其他幻觉缓解方法（未明确）

### 3.3 评估指标
- **成功率**（Success Rate）
- **pass^k**（在 k 次尝试中至少一次成功的概率）
- **幻觉率**（Hallucination rate）降低

## 4. 资源与算力

- **未明确说明**：论文元数据和摘要未提及使用的 GPU 型号、数量、训练时长等算力信息。
- 仅能推测：后训练阶段基于可能已有的预训练 LLM（如 LLaMA 系列）进行微调，算力需求与常见指令微调相当。

## 5. 实验数量与充分性

- **实验数量**：仅给出了在单一 benchmark（BFCL V2）上的结果，未提到跨多个数据集或领域的实验。
- **消融实验**：元数据中未提及消融实验细节，但 SVS 的加入提供了与无自验证的对比可视为消融。
- **充分性评估**：
  - **优点**：在标准 benchmark 上给出了量化提升。
  - **不足**：
    - 缺少更广泛的任务类型（如对话、多轮工具调用）。
    - 未与更多最新基线（如 Reflexion、ReAct 等）对比。
    - 实验规模较小，仅一个 benchmark 的结论可能不够泛化。
  - **公平性**：使用公开排行榜，比较相对公平，但缺乏对基线复现的详细说明。

## 6. 主要结论与发现

1. **DyMo 显著提高成功率**：在 BFCL V2 上，带状态预测的模型成功率优于无状态预测的基线。
2. **幻觉大幅减少**：通过内部状态预测，模型能识别并拒绝可能导致幻觉的工具调用。
3. **自验证采样提升 pass^k 效率**：随着尝试次数 k 增加，pass^k 曲线明显优于简单重复采样。
4. **模型学会拒绝不可靠输出**：SVS 支持必要时不输出，增强安全性。
5. **为可扩展规划 RL 提供了新路径**：无需反复查询真实环境即可在推理时进行规划和验证。

## 7. 优点

- **方法创新性**：首次将状态预测能力直接集成到 LLM 后训练中，实现内部环境模型，减少对外部模拟器的依赖。
- **实用性**：解决有状态环境中重复试验不可行的问题，更具实用性。
- **自验证采样设计简洁**：易于集成到现有 LLM 工具使用流程中。
- **性能提升显著**：在标准 benchmark 上验证了有效性和可靠性提升。
- **推理时可拒绝输出**：增加安全性与可控性。

## 8. 不足与局限

- **实验覆盖不足**：
  - 仅在一个 benchmark（BFCL V2）上测试，未覆盖其他函数调用数据集或真实应用。
  - 缺少跨模型（不同 LLM 规模）的泛化实验。
- **消融分析不深入**：
  - 未单独分析状态预测模块和自验证采样的贡献。
  - 未说明状态预测准确率与最终性能的关系。
- **算力资源未公开**：影响可复现性评估。
- **未讨论失败案例**：未分析仍会产生幻觉的场景或模型拒绝原因的分布。
- **应用限制**：
  - 依赖后训练阶段获取状态预测能力，对于无法微调的模型（如 GPT-4 API）不适用。
  - 内部环境模型可能不适合复杂、不确定的环境变化。

（完）
