---
title: "Beyond Reactivity: Measuring Proactive Problem Solving in LLM Agents}"
title_zh: 超越反应性：衡量LLM Agent的主动问题解决能力
authors: "Gil Pasternak, Dheeraj Rajagopal, Julia White, Dhruv Atreja, Matthew Thomas, George Hurn-Maloney, Ash Lewis"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=LYUpZLDObv"
tags: ["query:agent-traj"]
score: 4.0
evidence: 评估LLM agent的主动行为
tldr: 该论文指出当前基准局限于局部上下文，无法测试agent跨源的长期推理。为此提出PROBE框架，将主动性拆解为搜索未指定问题、识别瓶颈和执行解决三个能力，用于评估LLM agent。实验表明最先进模型仍难以解决大部分主动性问题。该工作为评估agent行为提供了新视角。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有基准无法充分测试agent的主动性和长期推理。
method: 提出PROBE框架，将主动性分解为三个核心能力并设计评估任务。
result: 最先进模型在PROBE上表现不佳，说明主动性问题具有挑战性。
conclusion: 为评估agent的主动行为提供了系统化方法。
---

## Abstract
LLM-based agents are increasingly moving towards proactivity: rather than awaiting instruction, they exercise agency to anticipate user needs and solve them autonomously. However, evaluating proactivity is challenging; current benchmarks are constrained to localized context, limiting their ability to test reasoning across sources and longer time horizons. 

To address this gap, we present PROBE (Proactive Resolution of Bottlenecks). PROBE decomposes proactivity as a pipeline of three core capabilities: (1) searching for unspecified issues, (2) identifying specific bottlenecks, and (3) executing appropriate resolutions. We apply PROBE to evaluate leading LLMs and popular agentic frameworks, showing that even state-of-the-art models struggle to solve this benchmark. Computing our consistent measurements across frontier LLMs and agents, we find that the best end-to-end performance of 40% is achieved by both GPT-5 and Claude Opus-4.1. Additionally, we demonstrate the relative capabilities of each model and analyze mutual failure modes. Our results highlight the current limitations of autonomous action in agentic systems, and show promising future directions.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

当前基于大语言模型（LLM）的智能体正从被动响应向主动行为演进——即主动预测用户需求并自主解决问题，而非仅等待指令。然而，现有基准测试存在明显局限：它们通常局限于局部上下文，无法评估智能体跨信息源、跨时间维度的长期推理与主动性。为此，论文提出了 **PROBE（Proactive Resolution of Bottlenecks）** 框架，旨在系统化地衡量LLM智能体的主动问题解决能力，填补该评估空白。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
将“主动性”分解为三个核心能力组成的流水线（pipeline），每个能力对应一个评估维度：

- **能力1：搜索未指定问题** – 智能体能否在没有明确指示的情况下，主动发现潜在问题或需求。
- **能力2：识别特定瓶颈** – 在发现问题后，能否准确定位关键障碍（bottlenecks）。
- **能力3：执行适当解决方案** – 能否自主选择并执行有效的解决步骤。

这三个能力依次递进，形成从“发现→定位→解决”的完整主动行为链条。

### 关键技术细节
- 该框架不依赖特定领域，通过设计多源信息、长时跨度的任务来测试智能体的跨上下文推理。
- 评估时分别计算每个能力的指标以及端到端（end-to-end）成功率。
- 未提供具体的算法伪代码或公式，但明确给出了评估流程：输入多个相关/无关文档 → 智能体需自主识别隐含问题 → 定位关键瓶颈 → 执行解决。

## 3. 实验设计

### 数据集 / 场景
- 使用PROBE基准本身作为评估场景，该基准由论文设计，包含需要主动推理的多源、长上下文任务。
- 未列出具体的外部数据集名称，但任务涉及跨文档的信息整合、隐含需求发现等。

### 基准对比
- 评估了领先的LLM模型：GPT-5 和 Claude Opus-4.1 等前沿模型。
- 对比了流行的智能体框架（agentic frameworks），但未具体列出框架名称。

### 对比方法
- 主要对比不同模型/框架在PROBE上的端到端性能和各能力模块的独立表现。

## 4. 资源与算力

论文摘要及元数据中 **未明确提及** 使用的GPU型号、数量、训练时长等算力信息。因此无法总结相关细节。可能的原因是该工作主要侧重于评估而非训练，或论文正文中有说明但摘要未体现。

## 5. 实验数量与充分性

- **实验数量**：仅提及对“leading LLMs and popular agentic frameworks”进行了评估，并报告了GPT-5和Claude Opus-4.1的端到端性能（均为40%）。未给出其他模型的完整列表或多次重复实验的次数。
- **充分性分析**：论文还进行了失败模式分析（mutual failure modes），显示实验有一定深度。但由于缺乏公开的完整实验设置（如任务数量、随机种子、统计显著性），无法判定实验是否充分。单从摘要看，实验覆盖了多个前沿模型，但样本量可能较小，客观性和公平性需依赖完整论文中的详细方法验证。

## 6. 论文的主要结论与发现

1. **最先进模型表现不佳**：即使是GPT-5和Claude Opus-4.1，在PROBE上的最佳端到端准确率也仅为 **40%**，表明主动性问题对当前AI系统极具挑战性。
2. **框架有效性**：PROBE能有效区分不同模型在主动性各个子能力上的相对优劣。
3. **失败模式共性**：不同模型在主动发现未指定问题上普遍存在瓶颈，说明了当前Agent自主行动的根本局限。
4. **未来方向**：指出需要改进模型的长程推理、跨源信息整合以及自主决策能力。

## 7. 优点：方法或实验设计上的亮点

- **新颖的评估视角**：将主动性从单一步骤的“反应”拓展为多步骤的流水线（搜索→识别→解决），更贴近真实场景。
- **系统化解构**：分解为三个可独立测量的能力，便于诊断模型短板。
- **关注长期推理**：任务设计需要跨多源、长时间关联，突破了现有局部下文的限制。
- **包含失败模式分析**，有助于理解模型失效的根本原因。

## 8. 不足与局限

- **实验覆盖有限**：仅报告了两个模型的端到端结果，对比模型较少；缺乏对开源模型（如Llama系列）的评估。
- **任务多样性不足**：未说明PROBE包含多少种具体任务，可能存在领域偏差（如仅涉及特定类型的主动性问题）。
- **资源与细节缺失**：未提供计算资源、任务数量、统计指标等关键实验细节，降低了可复现性。
- **未讨论安全性与伦理**：主动行为可能带来隐私或误操作风险，论文未涉及。
- **适用场景限制**：该基准可能主要针对信息搜寻类任务，对物理世界或实时交互场景的拓展性尚未验证。

（完）
