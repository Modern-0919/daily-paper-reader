---
title: "EnCompass: Enhancing Agent Programming with Search Over Program Execution Paths"
title_zh: EnCompass：通过程序执行路径搜索增强智能体编程
authors: "Zhening Li, Armando Solar-Lezama, Yisong Yue, Stephan Zheng"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=IKVkpjSJzJ"
tags: ["query:agent-traj"]
score: 7.0
evidence: 在智能体程序执行路径上搜索的框架，支持轨迹测试
tldr: EnCompass提出概率天使非确定性（PAN）编程模型，将智能体工作流逻辑与推理策略解耦，允许对执行路径进行系统性搜索。该框架为轨迹测试和推理策略比较提供了基础设施，可支持错误检测和轨迹分析。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 当前智能体编程将工作流逻辑与推理策略耦合过紧。
method: 提出PAN编程模型，用Python装饰器编译工作流为搜索空间。
result: 通过案例展示框架如何轻松更换推理策略。
conclusion: 为智能体轨迹测试和策略探索提供了灵活框架。
---

## Abstract
We introduce a new approach to *agent programming*, the development of LLM-based agents. Current approaches to agent programming often entangle two aspects of agent design: the core workflow logic and the inference-time strategy (e.g., tree search). We introduce *probabilistic angelic nondeterminism* (PAN), a programming model that disentangles these two concerns, allowing the programmer to describe the agent workflow and independently experiment with different inference-time strategies by simply changing a few inputs. We provide an implementation of PAN in Python as the EnCompass framework, which uses a Python decorator to compile agent workflow programs into a search space. We present three case studies that demonstrate how the framework lets the programmer quickly improve the reliability of an agent and easily switch between different inference-time strategies, all with little additional coding.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：当前基于大语言模型（LLM）的智能体编程中，核心工作流逻辑与推理策略（如树搜索）常被紧密耦合，导致代码难以复用、调试和维护。开发者修改推理策略时往往需要重写大量逻辑，限制了智能体可靠性的快速迭代。
- **整体含义**：论文旨在解耦这两个方面，提出一种新的编程范式，让程序员能独立描述工作流，并通过更改少量输入参数实验不同推理策略，从而提升智能体开发的灵活性和可靠性。

### 2. 论文提出的方法论
- **核心思想**：引入**概率天使非确定性（Probabilistic Angelic Nondeterminism, PAN）**编程模型，将智能体工作流编译为搜索空间。
- **关键技术细节**：
  - PAN 模型将工作流中的非确定性选择（例如：选择LLM输出的哪个路径继续执行）抽象为可搜索的节点，赋予每个选择概率分布。
  - 实现为**EnCompass框架**，基于Python，通过一个Python装饰器将工作流程序自动编译为搜索空间，程序员只需编写普通的工作流函数，装饰后即可支持搜索。
  - 推理策略（如广度优先搜索、蒙特卡洛树搜索等）作为外部参数输入，可轻松切换，无需改动工作流代码。
- **算法流程（文字说明）**：程序员用Python定义智能体工作流（包含条件分支、循环等），用@encompass.decorator装饰；EnCompass自动分析控制流，将每个非确定性点转化为概率分支；运行时，框架根据指定的推理策略（如beam search）探索程序执行路径，返回最优轨迹或集合。

### 3. 实验设计
- 论文仅提供**三个案例研究（case studies）**，未提及具体数据集、标准benchmark或对比方法。
- 案例内容：展示如何通过更换推理策略快速提升智能体可靠性（例如，从简单采样改为强化搜索），以及如何在不同策略间轻松切换。未进行定量对比实验。

### 4. 资源与算力
- 论文摘要及元数据中**未提及**任何GPU型号、数量或训练时长等信息，也未说明实验环境。推测该研究侧重于框架设计，未涉及大规模训练或分布式计算。

### 5. 实验数量与充分性
- **实验数量**：仅3个案例研究，无大规模消融实验或对比实验。
- **充分性**：不足。案例研究可证明方法论可行性，但缺乏定量指标（如成功率、效率）、与现有方法的系统性比较，以及在不同复杂度任务上的泛化能力验证。客观性和公平性无法评估。

### 6. 论文的主要结论与发现
- EnCompass框架能有效解耦智能体工作流逻辑与推理策略，允许开发者通过简单更改输入（如策略名称和参数）来实验不同策略。
- 这一框架为轨迹测试和错误检测提供了基础设施，可辅助开发者快速定位程序中的逻辑问题或LLM输出异常。

### 7. 优点
- **创新性**：将编程语言中的非确定性（angellic nondeterminism）与概率搜索结合，引入智能体开发领域，思想新颖。
- **实用性**：通过Python装饰器实现零侵入式集成，对现有代码改动小，降低使用门槛。
- **灵活性**：支持即插即用多种推理策略，便于性能对比和迭代优化。

### 8. 不足与局限
- **实验设计薄弱**：未提供定量评估，缺乏公开基准和对比基线，无法判断性能提升幅度。
- **可扩展性风险**：仅展示了小型案例，未测试在复杂多步骤、动态环境中框架的搜索开销和扩展性。
- **偏差风险**：案例可能为作者自选，存在选择偏差；未讨论失败情况或局限性。
- **应用限制**：依赖LLM作为非确定性源，框架性能受限于LLM的输出质量和搜索策略的效率，未见对计算成本的分析。

（完）
