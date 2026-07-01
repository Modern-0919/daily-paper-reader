---
title: "LiveMCP-101: Stress Testing and Diagnosing MCP-enabled Agents on Challenging Queries"
title_zh: LiveMCP-101：对支持MCP的智能体进行挑战性查询的压力测试与诊断
authors: "Ming Yin, Dinghan Shen, Silei Xu, Jianbing Han, Sixun Dong, Mian Zhang, Yebowen Hu, Shujian Liu, Simin Ma, Song Wang, Sathish Reddy Indurthi, Xun Wang, Yiran Chen, Kaiqiang Song"
date: 2025-09-01
pdf: "https://openreview.net/pdf?id=qIsGS463qk"
tags: ["query:agent-traj"]
score: 9.0
evidence: 用于多步工具使用轨迹压力测试的基准
tldr: 针对MCP智能体在多步工具调用场景下的测试需求，本文提出了LiveMCP-101基准，包含101个精心筛选的真实世界查询，并引入并行评估框架处理响应时间变异性，有效诊断智能体在复杂任务中的表现。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有基准缺乏对MCP智能体在多步动态场景下的测试能力。
method: 构建101个真实世界查询，并设计并行评估框架来比较智能体与参考计划的执行结果。
result: 实验表明该基准能有效暴露智能体在复杂工具调用中的问题。
conclusion: LiveMCP-101为MCP智能体提供了一个可扩展的测试平台。
---

## Abstract
Tool calling has emerged as a critical capability for AI agents to interact with the real world and solve complex tasks. While the Model Context Protocol (MCP) provides a powerful standardized framework for tool integration, there is a significant gap in benchmarking how well agents can solve multi-step tasks using diverse MCP tools in realistic, dynamic scenarios. In this work, we present LiveMCP-101, a benchmark of 101 carefully curated real-world queries, refined through iterative LLM rewriting and manual revision, that require coordinated use of multiple MCP tools. To address temporal variability in real-world tool responses, we introduce a parallel evaluation framework where a reference agent executes a validated plan simultaneously with the evaluated agent to produce real-time reference outputs, rather than relying on static ground-truth answers. Experiments show that even frontier LLMs achieve a task success rate below 60\%, highlighting major challenges in multi-step tool use. Comprehensive error analysis identifies seven failure modes spanning tool planning, parameterization, and output handling, pointing to concrete directions for improving current models. LiveMCP-101 sets a rigorous standard for evaluating real-world agent capabilities, advancing toward autonomous agent systems that reliably execute complex tasks through MCP tool orchestration.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：工具调用是AI智能体与现实世界交互、解决复杂任务的关键能力。模型上下文协议（Model Context Protocol, MCP）为工具集成提供了标准化框架，但现有基准缺乏对智能体在**真实、动态场景**中**多步、多工具协作**能力的测试。
- **核心问题**：如何系统评估MCP智能体在复杂多步工具调用任务中的表现，并诊断其失败模式。
- **整体意义**：本文提出LiveMCP-101基准，通过精心设计的真实世界查询和并行评估框架，揭示了当前最先进大模型在复杂工具调用中成功率不足60%的现状，并归纳了七种失败模式，为提升智能体可靠性提供了明确方向。

### 2. 方法论：核心思想、关键技术细节
- **核心思想**：构建一个包含101个真实世界查询的基准，这些查询需要协调使用多个MCP工具，并通过**并行评估框架**处理真实世界工具响应的时效性变异性，避免依赖静态完全正确答案。
- **关键技术细节**：
  - **查询生成**：通过迭代LLM重写和人工修订，从真实世界场景中筛选出101个查询，覆盖多步、多工具依赖。
  - **并行评估框架**：运行评估时，同时启动一个执行已验证参考计划的**参考智能体**，与被评估智能体同步执行，实时获取参考输出，从而避免静态答案因工具返回值（如实时天气、价格）变化而失效。
  - **失败模式分析**：通过对比智能体轨迹与参考计划，自动识别并分类工具规划、参数化、输出处理等环节的失败类型，最终归纳为7种模式。
- **算法/流程**（文字说明）：
  1. 对每个查询，由人类专家设计并验证一个**参考计划**（包含工具调用顺序、参数模板）。
  2. 评估时，参考智能体按参考计划执行并记录实时输出；被评估智能体自由调用工具生成轨迹。
  3. 比较两者的工具调用序列、参数、输出处理，逐步骤标记成功/失败，再聚合为任务级成功率。
  4. 对失败案例进行自动错误分类，映射到7种预定义模式。

### 3. 实验设计
- **数据集/场景**：LiveMCP-101基准，包含101个精心策划的真实世界查询。每个查询要求使用多个MCP工具（如天气、汇率、数据库、API调用等），涉及多步协作。
- **基准（Benchmark）**：LiveMCP-101本身作为新基准，无现成可比基准。
- **对比方法**：评估**前沿大模型**（frontier LLMs），包括GPT-4、Claude等（论文未列出具体型号，但指明为当前最先进模型）。不涉及其他专门方法或微调模型对比，重点是通过该基准暴露模型缺陷。

### 4. 资源与算力
- **文中未明确提及**使用的GPU型号、数量或训练时长。评估基于商用API调用，算力消耗取决于模型提供商，作者未提供具体信息。

### 5. 实验数量与充分性
- **实验数量**：主要实验是101个查询在各前沿模型上的完整测试，以及因此产生的失败模式统计。未提及消融实验或不同配置对比。
- **充分性评估**：
  - **客观性**：使用并行评估框架保证了比较的公平性，避免了静态标签过时问题；人工审查参考计划确保基准质量。
  - **公平性**：所有模型在相同查询和实时环境下测试，公平性较好。
  - **充分性**：仅报告了整体成功率（<60%），缺少细分模型对比、不同工具类型表现分析、跨查询难度粒度分析等。实验设计相对直接，未进行多轮、参数敏感性等更深入分析。因此充分性一般，但作为初始基准可接受。

### 6. 主要结论与发现
- **主要结论**：现有前沿大模型在LiveMCP-101上的任务成功率低于60%，说明多步MCP工具调用仍是重大挑战。
- **发现**：通过错误分析，识别出七种失败模式：
  1. 工具规划不完整（遗漏必要步骤）
  2. 工具顺序错误
  3. 参数缺失或错误
  4. 参数格式/类型不匹配
  5. 输出解析错误
  6. 未能正确处理中间结果
  7. 循环/停滞等控制策略问题
- 这些发现为改进模型提供了具体方向（如增强规划、参数验证、输出处理）。

### 7. 优点
- **方法亮点**：
  - **并行评估框架创新性**：解决了真实场景下工具返回结果时效性问题，避免静态基准失效，这是对传统评估范式的改进。
  - **真实世界查询**：查询经过LLM重写+人工修订，兼具复杂性与真实性，比人为构造的合成任务更有生态效度。
  - **系统性错误分类**：提炼7种失败模式，具有诊断价值，可直接指导后续研究。
- **实验设计亮点**：基准规模适中（101个），覆盖足够多样性；人工验证参考计划确保gold标准可靠。

### 8. 不足与局限
- **实验覆盖不足**：
  - 仅测试了极少数（未明确数量）前沿模型，缺乏对开源模型、小型模型或微调后模型的对比。
  - 未进行消融实验（如不同评估框架效果、错误分类一致率）。
  - 未分析不同查询难度梯度或工具依赖复杂度的关系。
- **偏差风险**：
  - 101个查询可能偏向特定领域（如信息查询、简单API调用），未必泛化到更多样化的MCP工具场景。
  - 参考计划由人类专家设计，可能引入专家偏差，且未评估不同参考计划对结果的影响。
- **应用限制**：
  - 并行评估框架需要额外运行参考智能体，增加了评估成本。
  - 基准当前规模小，可能不足以充分展示模型能力下限；未来需扩展至更多查询和工具类型。
- **其他**：未讨论基准更新维护（工具可能过期）、测试可重复性（需固定工具环境）等问题。

（完）
