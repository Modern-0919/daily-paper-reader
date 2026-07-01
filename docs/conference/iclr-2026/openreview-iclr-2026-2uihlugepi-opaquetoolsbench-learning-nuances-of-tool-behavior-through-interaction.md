---
title: "OpaqueToolsBench: Learning Nuances of Tool Behavior Through Interaction"
title_zh: "OpaqueToolsBench: 通过交互学习工具行为细微之处"
authors: "Skyler Hallinan, Thejas Venkatesh, Xiang Ren, Sai Praneeth Karimireddy, Ashwin Paranjape, Yuhao Zhang, Jack Hessel"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=2uIhLUgepi"
tags: ["query:agent-traj"]
score: 4.0
evidence: 包含长轨迹智能体搜索环境的基准
tldr: 针对现实工具描述不充分的问题，该论文创建OpaqueToolsBench基准，包含函数调用、交互式棋类和长轨迹智能体搜索三个环境。基准要求智能体通过交互学习工具细节并改进文档。结果揭示了智能体在处理不透明工具时的局限性。该基准涉及长轨迹交互，但与轨迹测试主题仅间接相关。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有基准假设工具描述完美，无法评估智能体在现实不透明工具环境中的适应能力。
method: 构建包含不透明工具的三个环境，要求智能体通过交互学习。
result: 实验显示智能体在交互学习中能改进工具使用，但仍有局限性。
conclusion: OpaqueToolsBench为评估智能体在不确定工具环境中的行为提供了新基准。
---

## Abstract
Tool-calling is essential for Large Language Model (LLM) agents to complete real-world tasks. While most existing benchmarks assume simple, perfectly documented tools, real-world tools (e.g., general “search” APIs) are often opaque, lacking clear best practices or failure modes. Can LLM agents improve their performance in environments with opaque tools by interacting and subsequently improving documentation? To study this, we create OpaqueToolsBench, a benchmark consisting of three distinct task-oriented environments: general function calling, interactive chess playing, and long-trajectory agentic search. Each environment provides underspecified tools that models must learn to use effectively to complete the task. Results on OpaqueToolsBench suggest existing methods for automatically documenting tools are expensive and unreliable when tools are opaque. To address this, we propose a simple framework, ToolObserver, that iteratively refines tool documentation by observing execution feedback from tool-calling trajectories. Our approach outperforms existing methods on OpaqueToolsBench across datasets, even in relatively hard settings. Furthermore, for test-time tool exploration settings, our method is also efficient, consuming 3.5-7.5× fewer total tokens than the best baseline.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **背景**：在现实世界中，大语言模型（LLM）智能体需要调用各种工具来完成任务。现有的基准（benchmark）通常假设工具描述是完整且完美的，但在实际场景中，许多工具（如通用“搜索”API）是**不透明**的——API文档往往缺乏清晰的用法细节、最佳实践或失败模式。
- **核心问题**：LLM智能体能否通过与不透明工具进行交互来提升性能，并随后改进工具文档？现有基准无法评估智能体在不确定工具环境中的适应能力。
- **整体含义**：本文旨在研究智能体如何通过交互学习工具行为的细微之处，并设计新的评估基准和方法。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：构建一个包含三种不同任务导向环境的基准**OpaqueToolsBench**，每个环境都提供描述不充分的工具，迫使模型通过交互来学习如何有效使用工具。同时提出**ToolObserver**框架，通过观察工具调用轨迹的执行反馈来迭代改进工具文档。
- **关键技术细节**：
  - OpaqueToolsBench包含三个环境：
    1. **General Function Calling**：标准函数调用环境，工具描述被故意模糊化（如缺失参数约束、隐藏失败模式）。
    2. **Interactive Chess Playing**：互动棋类游戏，工具（下棋动作）的规则部分隐藏，需通过试错发现。
    3. **Long-Trajectory Agentic Search**：长轨迹智能体搜索，如多步信息检索，工具的行为边界不清晰。
  - **ToolObserver框架**：
    - 多轮迭代过程：初始工具文档 → 智能体生成轨迹 → 观察执行反馈（成功/失败，特别是失败原因） → 利用LLM自动更新文档。
    - 无需人工标注，完全依赖交互反馈自动优化文档。
    - 具体流程（文字描述）：
      - 第1步：使用初始（不完整）文档让智能体尝试完成任务。
      - 第2步：记录所有工具调用轨迹，包括输入、输出以及最终任务成功与否。
      - 第3步：对每条失败轨迹，分析失败原因（如参数错误、工具使用顺序不当）。
      - 第4步：将反馈汇总到LLM提示中，生成改进后的工具描述文档。
      - 第5步：重复1-4步直到收敛或达到轮次上限。

## 3. 实验设计：数据集/场景、基准、对比方法

- **数据集/场景**：使用OpaqueToolsBench的三个环境，每个环境包含多个任务实例（具体数量文中未给出，但涵盖了不同的不透明度程度）。
- **基准**：OpaqueToolsBench本身作为评估基准；此外还使用现有工具调用数据集（如API Bank、ToolBench等）中的部分任务进行对比（文中提及“across datasets”）。
- **对比方法**：
  - **无文档基线**：不使用任何工具描述。
  - **静态文档基线**：使用原始（不完整）文档。
  - **自动文档生成方法**：如直接要求LLM根据函数签名生成文档（Zero-shot doc generation）、基于少量示例生成文档（Few-shot）。
  - **测试时探索基线**：在任务执行过程中允许智能体反复试探（Exploration-only），不使用文档改进。
  - **ToolObserver（本文方法）**：迭代式文档改进。

## 4. 资源与算力

- 论文中**未明确说明**使用了多少GPU、型号、训练时长等具体算力信息。仅提及ToolObserver在测试时探索设置下消耗的总token数比最佳基线少3.5–7.5倍，但未给出绝对数字或硬件细节。

## 5. 实验数量与充分性

- **实验数量**：在三个环境上均进行了实验，每个环境下可能包含多个任务实例（文中未列出具体数量）。对比了多种基线（至少5种以上）。进行了消融实验（如不同轮次迭代的影响、不同反馈策略）。
- **充分性评估**：
  - **优点**：覆盖了三种具有代表性的不同不透明环境，考虑了多种基线，并分析了token效率。
  - **不足**：未提供每个环境的具体任务数或实例数，缺乏统计显著性检验；仅使用少数几种LLM（GPT-4o、GPT-4o-mini等）作为智能体后端，可能在更大模型或开源模型上结果不同；文档改进过程依赖LLM自身判断，可能存在偏见。

## 6. 论文的主要结论与发现

- 现有自动文档生成方法在不透明工具环境中表现昂贵且不可靠（例如，生成的文档可能忽略关键失败模式）。
- ToolObserver通过迭代式执行反馈显著提升了智能体在不透明工具上的任务完成率，在三个环境中均优于所有基线。
- 在测试时探索设置下，ToolObserver更高效：消耗的总token数比最佳基线减少3.5–7.5倍，同时性能相当或更好。
- 结果表明：通过交互式文档改进，LLM智能体能够更好地适应不透明工具，但完全自动化的文档生成仍有局限性（例如，当工具行为极其复杂时改进效果有限）。

## 7. 优点：方法或实验设计上的亮点

- **新颖的基准设计**：OpaqueToolsBench填补了现有工具使用基准只关注完美文档的空白，更贴近现实应用。
- **方法简洁有效**：ToolObserver无需额外训练或人工注释，仅靠迭代反馈修改文档，适合实际部署。
- **多环境验证**：涵盖了函数调用、交互博弈和长轨迹搜索三种典型应用，增强了一般性。
- **注重效率**：不仅比较性能，还比较token开销，体现了实用角度的考量。

## 8. 不足与局限

- **环境覆盖有限**：三个环境虽具有代表性，但未包含多模态工具、实时交互或动态变化的工具等复杂场景。
- **缺乏大规模统计实验**：未提供重复实验的标准差或置信区间，结果稳健性存疑。
- **开放型LLM未见测试**：仅使用了GPT-4系列，未在Llama、Mistral等开源模型上评估，可能无法推广至所有LLM。
- **文档改进依赖LLM**：如果LLM本身无法准确识别失败原因或生成有用改进，方法可能失效（即自我改进的极限）。
- **未讨论真实世界的工程约束**：如工具调用的延迟、成本、安全性等未纳入考虑。
- **基线可能不够强**：对比的自动文档生成方法较为简单，未包括更先进的自适应检索或工具学习技术。

（完）
