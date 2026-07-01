---
title: "UltraHorizon: Benchmarking LLM-Agent Capabilities in Ultra Long-Horizon Scenarios"
title_zh: UltraHorizon：在超长时域场景下基准测试LLM-Agent能力
authors: "Haotian Luo, Huaisong Zhang, Xuelin Zhang, Haoyu Wang, Zeyu Qin, WenJie Lu, Guozheng Ma, Haiying He, Yingsha Xie, Qiyang Zhou, Zixuan Hu, Hongze Mi, Yibo Wang, Naiqiang Tan, Hong Chen, Yi R. Fung, Chun Yuan, Li Shen"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=FTZfVHWAIq"
tags: ["query:agent-traj"]
score: 6.0
evidence: 用于评估LLM agent长时域场景的基准
tldr: 该论文指出多数评估聚焦短期全观测任务，忽略了长时域部分可观测场景。为此提出UltraHorizon基准，使用探索作为统一任务，测试agent在持续推理、规划、记忆管理和工具使用上的能力。实验证明现有agent在此类场景中表现低下。该基准填补了长时域评估的空白。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有基准未能覆盖真实世界中长时域部分可观测任务。
method: 构建以探索为核心任务的基准，涵盖多维度能力。
result: 现有agent在长时域任务中表现有限。
conclusion: 提供了评估agent长时域能力的系统基准。
---

## Abstract
Autonomous agents have recently achieved remarkable progress across diverse domains, yet most evaluations focus on short-horizon, fully observable tasks. In contrast, many critical real-world tasks, such as large-scale software development, commercial investment, and scientific discovery, unfold in long-horizon and partially observable scenarios where success hinges on sustained reasoning, planning, memory management, and tool use. Existing benchmarks rarely capture these long-horizon challenges, leaving a gap in systematic evaluation. To bridge this gap, we introduce $\textbf{UltraHorizon}$, a novel benchmark that measures the foundational capabilities essential for complex real-world challenges. We use exploration as a unifying task across three distinct environments to validate these core competencies. Agents are designed in long-horizon discovery tasks where they must iteratively uncover hidden rules through sustained reasoning, planning, memory and tools management, and interaction with environments. Under the heaviest scale setting, trajectories average $\textbf{200k+}$ tokens and $\textbf{400+}$ tool calls, whereas in standard configurations they still exceed $\textbf{35k}$ tokens and involve more than $\textbf{60}$ tool calls on average. Our extensive experiments reveal that agents powered by state-of-the-art LLMs consistently underperform in these settings, whereas human participants achieve much higher scores, underscoring a persistent gap in agents' long-horizon exploration abilities. We also observe that simple scaling fails in our task. To better illustrate the failure of agents, we conduct an in-depth analysis of collected trajectories. We identify eight types of errors and attribute them to two primary causes: in-context locking and functional fundamental capability gaps.

---

## 论文详细总结（自动生成）

以下是基于论文元数据和摘要的详细中文总结：

### 1. 核心问题与整体含义
- **研究动机**：现有LLM-Agent评估大多聚焦于短时域、全观测任务，而现实世界中许多关键任务（如大规模软件开发、商业投资、科学发现）属于长时域、部分可观测场景，需要持续的推理、规划、记忆管理和工具使用能力。现有基准未能系统覆盖这些挑战。
- **整体含义**：构建一个能够衡量Agent在超长时域场景下核心能力的基准，填补评估空白，为后续研究提供参考。

### 2. 方法论：核心思想与关键设计
- **核心思想**：以**探索（exploration）**作为统一任务，要求Agent在多个环境中迭代地发现隐藏规则，过程中必须综合运用持续推理、规划、记忆管理和工具交互。
- **技术细节**：
  - 基准包含**三个不同的环境**（具体环境未在摘要中详述）。
  - 任务设计为长时域发现任务：Agent需通过多轮交互，逐步揭示环境的底层规则，每一步都需要基于已有记忆和推理进行决策。
  - 最重配置下，每条轨迹平均超过**200k tokens**和**400次工具调用**；标准配置下仍超过**35k tokens**和**60次工具调用**。
- 无公式或算法流程文字说明，主要体现为任务范式和评估协议。

### 3. 实验设计
- **数据集/场景**：UltraHorizon基准自身包含三个不同环境（具体名称未给出），每个环境代表一类长时域探索问题。
- **Benchmark**：UltraHorizon本身作为评估基准。
- **对比方法**：
  - **多种SOTA LLM**（如GPT-4系列等，摘要未列出具体模型名称）。
  - **人类参与者**作为上界参考。
- 实验测量指标：任务完成得分、轨迹长度、工具调用次数等，并收集轨迹进行错误分类分析。

### 4. 资源与算力
- 论文**未明确说明**使用的GPU型号、数量、训练时长等计算资源信息。仅提及实验涉及大量轨迹生成和评估，但未提供硬件规格。

### 5. 实验数量与充分性
- **实验数量**：在多个配置（轻量、标准、最重）下进行了广泛测试，并收集了各配置下的轨迹数据。
- **充分性**：
  - 对比了多种SOTA模型与人类基线，覆盖了不同能力水平的Agent。
  - 进行了深度错误分析，识别出**8种错误类型**，并归因于两种主要原因（上下文锁定和功能基础能力差距）。
  - 实验设计较为系统，但未披露消融实验（如去掉记忆模块的影响等），可能因基准是评估性质而非方法提出。
- **公平性**：对比人类参与者是合理的基线，但未详细说明人类实验的规模和条件，可能存在参与偏差。

### 6. 主要结论与发现
- **Agent表现低下**：当前SOTA LLM驱动的Agent在所有长时域设置下均表现出明显不足，分数显著低于人类。
- **简单扩展失败**：单纯增加模型规模或推理时间并不能有效提升长时域探索能力。
- **两大失败原因**：
  - **上下文锁定（in-context locking）**：Agent过度依赖历史上下文，无法跳出局部最优或重复错误模式。
  - **功能基础能力差距（functional fundamental capability gaps）**：Agent缺乏必要的长期规划、记忆管理和工具协同等基础能力。

### 7. 优点
- **填补空白**：首次系统聚焦于长时域部分可观测场景的评估，弥补了现有基准的不足。
- **统一任务范式**：使用“探索”作为核心任务，能同时考察推理、规划、记忆和工具使用等多维度能力。
- **详细错误分析**：通过轨迹挖掘出具体失效模式，为后续改进提供了方向。
- **人类基线**：引入人类表现作为参考，使评估结果更具参考价值。

### 8. 不足与局限
- **环境细节缺失**：未在摘要中具体描述三个环境的性质、复杂度、领域偏向（可能是模拟游戏、软件调试、科学实验等），难以评估泛化性。
- **计算资源未报告**：缺乏对实验成本（GPU时长等）的说明，影响可复现性。
- **模型覆盖有限**：仅提及“state-of-the-art LLMs”，未给出具体模型列表（如GPT-4, Claude, Llama等），可能遗漏某些表现较好的模型。
- **人类实验细节不足**：未说明人类参与者的数量、背景、任务熟悉程度，可能存在偏差。
- **应用限制**：基准任务可能无法完全代表所有真实长时域场景（如需要多Agent协作或复杂情感交互的任务）。
- **被拒稿可能暗示局限性**：作为ICLR 2026被拒论文，可能同行评审认为其创新性或实验设计存在不足。

（完）
