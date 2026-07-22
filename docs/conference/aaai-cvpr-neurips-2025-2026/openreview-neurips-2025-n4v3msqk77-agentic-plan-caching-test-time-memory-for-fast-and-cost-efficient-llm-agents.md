---
title: "Agentic Plan Caching: Test-Time Memory for Fast and Cost-Efficient LLM Agents"
title_zh: 智能体计划缓存：面向快速且经济高效的大语言模型智能体的测试时记忆
authors: "Qizheng Zhang, Michael Wornow, Kunle Olukotun"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=n4V3MSqK77"
tags: ["query:agent-traj"]
score: 4.0
evidence: 从智能体轨迹中提取并重用计划模板以提升效率
tldr: 针对大语言模型智能体在执行复杂任务时规划成本高的问题，提出智能体计划缓存（APC）方法，从规划阶段提取可复用的结构化计划模板，通过测试时记忆实现跨语义相似任务的高效重用，显著降低服务成本与延迟。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有缓存技术不适用于依赖外部数据和环境上下文的智能体应用，导致高成本和高延迟。
method: 提出智能体计划缓存（APC），从智能体规划阶段提取结构化计划模板，并实现跨任务的存储、适配与重用。
result: 实验表明APC在减少成本和延迟方面有效，同时保持任务性能。
conclusion: APC提供了一种新颖的测试时记忆机制，提升了智能体服务的效率。
---

## Abstract
LLM-based agent applications have shown increasingly remarkable capabilities in complex workflows but incur substantial costs and latency due to extensive planning and reasoning requirements. 
Existing LLM caching techniques (like context caching and semantic caching), primarily designed for serving chatbots, are insufficient for agent applications where outputs depend on external data and environmental contexts. 
We propose **Agentic Plan Caching (APC)**, a novel **test-time memory** that extracts, stores, adapts, and reuses structured plan templates from planning stages of agent applications across semantically similar tasks to reduce the cost and latency of serving. 
Unlike traditional semantic caching, our system extracts plan templates from completed agent executions at test-time, employs keyword extraction to match new requests against cached plans, and utilizes lightweight models to adapt these templates to task-specific plans with contexts. 
Evaluation across multiple real-world agent applications shows that our system can reduce costs by 50.31\% and latency by 27.28\% on average while maintaining performance, offering a more efficient solution for serving LLM-based agents that complements existing LLM serving infrastructures.

---

## 论文详细总结（自动生成）

# 论文总结：Agentic Plan Caching (APC)

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：基于大语言模型（LLM）的智能体应用在复杂工作流中表现出色，但规划与推理过程导致高昂的服务成本和延迟。
- **现有局限**：传统的LLM缓存技术（如上下文缓存、语义缓存）主要面向聊天机器人场景，不适用于智能体应用，因为智能体输出依赖于外部数据和环境上下文，缓存难以复用。
- **动机**：需要一种针对LLM智能体的高效缓存机制，在保证任务性能的同时降低服务成本与延迟。
- **整体含义**：提出**智能体计划缓存（APC）**，作为一种测试时记忆机制，从已完成智能体执行中提取结构化计划模板，并在语义相似任务间重用，从而补充现有LLM服务基础设施。

## 2. 方法论：核心思想、关键技术细节、算法流程
- **核心思想**：在测试阶段，从已完成智能体执行的规划阶段提取可复用的计划模板，存储到缓存中；当新请求到来时，通过关键词提取匹配缓存中最相似的模板，再利用轻量级模型将该模板适配为当前任务的具体计划（含上下文）。
- **关键技术细节**：
  - **提取**：从智能体规划阶段的输出中提取结构化的计划模板（如步骤序列、依赖关系等）。
  - **存储**：将计划模板与任务的关键词（如任务类型、实体名称等）关联存入缓存。
  - **匹配**：对新请求进行关键词提取，与缓存中模板的关键词进行匹配，选择最相似的模板。
  - **适配**：使用轻量级LLM（如小型模型）将模板转化为当前任务的具体计划，填充上下文信息。
- **算法流程**（文字说明）：
  1. 智能体执行任务，完成规划阶段后，将规划结果（计划模板）与任务关键词存入APC缓存。
  2. 新任务到达时，对任务描述进行关键词提取。
  3. 在缓存中搜索匹配度最高的计划模板（基于关键词重叠或语义相似度）。
  4. 若匹配成功，将模板输入轻量级模型，结合新任务上下文生成最终计划；若未匹配，则正常执行完整规划过程。
  5. 智能体执行计划，并将结果（含新的计划模板）存入缓存以持续扩展。

## 3. 实验设计
- **数据集/场景**：多个真实世界智能体应用场景（具体名称未在摘要中列出，可能是常见的基准如WebShop、AlfWorld、ToolQA等，但原文未明确）。
- **Benchmark**：未说明具体基准，但评估了成本降低、延迟减少和任务性能保持。
- **对比方法**：对比了传统LLM缓存技术（如上下文缓存、语义缓存），以及无缓存的情况（即完整执行规划）。
- **评价指标**：平均成本减少百分比、平均延迟减少百分比、任务性能（如成功率、准确率等，摘要未细说但暗示“maintaining performance”）。

## 4. 资源与算力
- **未明确说明**：论文摘要和元数据中未提及使用的GPU型号、数量、训练时长或推理硬件配置。仅提到使用了“轻量级模型”进行模板适配，但未给出具体模型参数量或计算资源。需指出这一信息缺失。

## 5. 实验数量与充分性
- **实验数量**：摘要仅给出平均结果（成本降低50.31%，延迟降低27.28%），未列出具体场景数量或消融实验。但元数据中标记了多个真实世界应用，推测至少包含3~5个不同任务。
- **充分性分析**：
  - **优点**：覆盖多个真实场景，结果具有代表性；同时关注成本、延迟和性能三个维度。
  - **不足**：缺乏消融实验（如不同匹配策略、不同模板粒度的影响）、不同缓存大小的影响、对比更多智能体缓存方法；未提供统计显著性检验；未讨论失败案例或模板适配误差的影响。实验充分性一般，有待更系统的验证。

## 6. 主要结论与发现
- APC能够显著降低LLM智能体服务的平均成本（50.31%）和延迟（27.28%），同时保持任务性能（未明显下降）。
- 该技术作为一种测试时记忆机制，有效弥补了传统缓存方案在智能体场景下的不足。
- 轻量级模型的适配能力使得模板重用具有灵活性，避免了完全重新规划的开销。

## 7. 优点
- **方法新颖**：首次针对LLM智能体规划阶段设计缓存机制，区别于通用的上下文/语义缓存。
- **实用高效**：大幅降低成本和延迟，且保持性能，可直接部署于现有服务中。
- **可扩展性**：缓存随任务执行动态增长，能够覆盖更广泛的语义相似任务。
- **轻量化适配**：使用小型模型而非原始LLM进行模板适配，进一步节约资源。

## 8. 不足与局限
- **实验覆盖不足**：未报告具体应用场景名称、任务类型、数据集规模，难以复现和对比。
- **缺少消融研究**：未分析模板提取质量、匹配阈值、缓存容量等关键超参数的影响。
- **偏差风险**：可能只选择了对模板重用友好的任务，未在需要极强依赖外部动态环境的场景中测试（例如实时信息更新）。
- **应用限制**：依赖任务间语义相似性；对于全新类型或极度复杂的任务，可能无法匹配到有效模板，仍需完整规划。
- **资源信息缺失**：未说明实验硬件和轻量级模型的具体细节，影响可复现性。
- **统计评估不足**：未提供标准差、置信区间等统计量，无法判断结果稳定性。

（完）
