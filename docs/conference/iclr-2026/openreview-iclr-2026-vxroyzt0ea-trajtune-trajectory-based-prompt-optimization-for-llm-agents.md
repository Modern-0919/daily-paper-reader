---
title: "TrajTune: Trajectory-Based Prompt Optimization for LLM Agents"
title_zh: TrajTune：基于轨迹的LLM代理提示优化
authors: "Bing Zhang, Shubhi Asthana, Hima Patel, Chad DeLuca"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=vXrOyzt0Ea"
tags: ["query:agent-traj"]
score: 6.0
evidence: 基于轨迹的提示优化方法，通过错误度量测试代理行为
tldr: LLM代理常因幻觉或重复动作失败，现有提示优化忽略轨迹信息。本文提出TrajTune，捕获结构化执行轨迹并计算细粒度错误指标，当超过自适应阈值时触发多LLM反馈循环迭代优化提示。实验表明该方法显著减少执行错误，提升代理可靠性，可视为一种轨迹驱动的测试方法。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: LLM代理失败常见于轨迹层面，但现有提示优化只关注输出文本质量。
method: TrajTune捕获代理执行轨迹，计算错误指标并与自适应阈值比较，触发多LLM反馈循环优化提示。
result: 在多个复杂任务上显著降低代理错误率，提高可靠性。
conclusion: TrajTune展现了轨迹分析在代理优化中的价值，间接贡献于代理测试。
---

## Abstract
Large Language Model (LLM) agents are increasingly deployed in complex tasks involving multi-step reasoning and dynamic API interactions. However, these agents often fail due to issues like hallucinated tool calls or repetitive actions, which are not effectively addressed by current prompt optimization methods that focus primarily on textual output quality.

We present TrajTune, a trajectory-aware prompt optimization framework designed to enhance the reliability and adaptability of LLM agents. TrajTune captures structured execution traces, computes fine-grained error metrics, and compares them against adaptive thresholds. When error metrics exceed these thresholds, a multi-LLM feedback loop is triggered to iteratively refine prompts, significantly reducing execution failures.

Across finance, software engineering, and IT-operations agents, TrajTune reduces hallucination rates by up to 40%, improves tool success rates by 30%, increases software engineering task accuracy by 25%, and boosts IT-ops success rates by 20%—while improving success-per-dollar and success-per-minute through fewer retries. These results demonstrate TrajTune’s effectiveness for robust, self-improving agentic systems.

---

## 论文详细总结（自动生成）

# 论文总结：TrajTune: Trajectory-Based Prompt Optimization for LLM Agents

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：大语言模型（LLM）代理在执行多步推理和动态API交互的复杂任务时，常常因幻觉性工具调用、重复动作等轨迹层面的错误而失败。现有提示优化方法主要关注文本输出质量，忽略了代理执行过程中丰富的轨迹信息。
- **整体含义**：本文提出**TrajTune**，一种轨迹感知的提示优化框架，通过显式捕获并分析代理的执行轨迹（而非仅最终输出），生成细粒度错误指标并与自适应阈值比较，触发多LLM反馈循环来迭代优化提示。该方法本质上是将代理的行为测试与优化相结合，显著提升了代理的可靠性和自适应性。

## 2. 方法论
- **核心思想**：利用结构化执行轨迹（包括动作、API调用、中间结果等）计算多种细粒度错误指标（如幻觉率、工具成功率、重复动作频率等），当指标超过由历史数据确定的自适应阈值时，启动多LLM反馈循环，让多个LLM协作生成有针对性的提示改进建议，从而迭代优化初始提示。
- **关键技术细节**：
  - **轨迹捕获**：记录代理的每一步动作、调用参数、返回结果及最终状态。
  - **错误指标计算**：定义一套量化指标（如工具调用幻觉次数、任务完成率、步骤冗余度等）。
  - **自适应阈值**：基于先前成功/失败轨迹动态设定阈值，避免固定阈值导致的过敏感或欠敏感。
  - **多LLM反馈循环**：当错误指标超阈值时，调用多个不同的LLM（如GPT-4、Claude等）对失败轨迹进行独立分析，综合它们的反馈生成提示修正建议。
- **算法流程**（文字说明）：
  1. 初始提示 → 代理执行任务 → 捕获完整轨迹。
  2. 计算轨迹的错误指标。
  3. 与自适应阈值比较：若未超阈值，结束；若超阈值，进入优化循环。
  4. 优化循环：将轨迹和错误指标输入多个LLM，各LLM分别给出提示改进建议 → 汇总、筛选、合并 → 生成新提示。
  5. 用新提示重新执行任务，重复上述过程直到收敛或达到最大迭代次数。

## 3. 实验设计
- **数据集/场景**：金融（FinTech）、软件工程（software engineering）、IT运维（IT-operations）三个领域的代理任务。
- **Benchmark**：未明确提及标准公开数据集名称，但文中表明使用真实业务场景或构建的复杂任务（如API调用、代码修改、运维故障处理等）。
- **对比方法**：未在提供的文本中明确列出基线方法，但隐含与“仅关注文本输出的常见提示优化方法”以及未优化的原始提示进行对比。

## 4. 资源与算力
- **显式说明**：文中**未提及**任何具体GPU型号、数量或训练时长。仅描述为“多LLM反馈循环”，可能依赖外部LLM API（如GPT-4等），未给出自训练算力消耗。

## 5. 实验数量与充分性
- **实验组数**：仅在摘要中定性给出三类场景（金融、软件工程、IT运维）的改善百分比（幻觉率降低40%、工具成功率提升30%、软件工程准确率提升25%、IT运维成功率提升20%），未详细列出多组实验的数据表或消融实验。
- **充分性评价**：实验覆盖了三个不同领域，具有一定多样性；但缺乏对数据集规模、任务数量、统计显著性检验、基线的详细比较、以及消融实验（如是否真的需要多LLM反馈、阈值自适应策略的必要性）的说明，**实验充分性不足**，主观性较强。

## 6. 主要结论与发现
- TrajTune能显著降低代理的错误率（幻觉率降低40%、工具成功率提升30%），提高任务准确率和成功率（软件工程25%、IT运维20%）。
- 通过减少重试次数，还改善了“成功每美元”和“成功每分钟”的经济效率。
- 轨迹分析与自适应阈值相结合是提升代理可靠性的有效手段。

## 7. 优点
- **视角新颖**：将轨迹（而非仅最终文本）作为优化反馈源，符合代理实际执行行为的特点。
- **自适应机制**：动态阈值避免固定阈值带来的泛化问题。
- **多LLM协作**：利用多个LLM的多样性生成更稳健的优化建议。
- **实际效果显著**：在多个行业场景中取得可观提升，且兼顾性能与成本。

## 8. 不足与局限
- **实验不透明**：缺乏详细的实验结果表格、统计误差、对比基线名称及配置，难以客观复现或评估公平性。
- **未提供算力数据**：无法评估资源开销（特别是多LLM调用可能带来高成本）。
- **应用限制**：依赖外部LLM的反馈能力，若反馈LLM本身存在偏差或幻觉，可能影响优化质量；未讨论对长轨迹任务的计算复杂度。
- **泛化性待验证**：仅在三个特定领域测试，未覆盖更通用的代理场景（如对话、机器人控制等）。
- **消融缺失**：未验证各组件（如多LLM vs. 单LLM、自适应阈值 vs. 固定阈值）的独立贡献。

（完）
