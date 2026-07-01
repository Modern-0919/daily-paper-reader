---
title: "Towards Self-Evolving Agent Benchmarks : Validatable Agent Trajectory via Test-Time Exploration"
title_zh: 面向自演化智能体基准：通过测试时探索实现可验证的智能体轨迹
authors: "Dadi Guo, Tianyi Zhou, Dongrui Liu, Chen Qian, Qihan Ren, Shuai Shao, Zhiyuan Fan, Yi R. Fung, Kun Wang, Linfeng Zhang, Jing Shao"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=2H03gm4Rq6"
tags: ["query:agent-traj"]
score: 8.0
evidence: 利用智能体轨迹探索的自演化基准
tldr: TRACE框架解决现有智能体基准快速发展撞顶的问题。它从原始任务出发，鼓励智能体通过自由探索将任务演化为更高难度版本，同时记录执行轨迹。整个过程分为三阶段：演化、验证和迭代。生成的轨迹可用于验证新任务的难度和可解性。该框架为持续评估智能体能力提供了动态基准演化方法。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有智能体基准被迅速饱和，无法区分先进模型。
method: 提出TRACE框架，通过智能体探索演化任务并记录轨迹。
result: 生成更难任务，延长基准寿命。
conclusion: 自演化基准有助于持续跟踪智能体进步。
---

## Abstract
Recent advances in large language models (LLMs) and agent system designs have empowered agents with unprecedented levels of capability. However, existing agent benchmarks are showing a trend of rapid ceiling-hitting by newly developed agents, making it increasingly difficult to meet the demands of evaluating agent abilities. To address this problem, we propose the Trajectory-based Validated-by-Reproducing Agent-benchmark Complexity Evolution (TRACE) framework. This framework takes an original task from an existing benchmark and encourages agents to freely explore and evolve it into a new task with higher difficulty while recording the corresponding execution trajectories. The framework proceeds in three stages: (1) evolutionary proposal mining, which generates task evolution proposals through preliminary exploration and divergent thinking; (2) problem construction via free exploration, where proposals are instantiated into concrete problem instances through agent exploration, with execution trajectories recorded along the process; and (3) multi-level validation, which ensures that the evolved tasks are accompanied by reproducible and logically coherent trajectories. Experiments on the GAIA benchmark demonstrate that the TRACE framework consistently enhances task complexity while improving correctness reliability through trajectory-level validation. In addition, our framework can successfully adapt to and improve reasoning benchmarks such as AIME-2024. This work marks a paradigm shift from static, manually curated benchmarks to dynamic, self-evolving evaluation systems, providing a sustainable and challenging foundation for agent development. Code and data can be found at https://github.com/titanwings/trace-benchmark-evolving.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：近年来大语言模型（LLM）和智能体系统快速发展，但现有智能体基准（benchmark）很快被新模型“撞顶”（ceiling-hitting），无法有效区分先进模型的性能差异，难以满足持续评估需求。
- **整体含义**：需要从静态、人工设计的基准转向动态、自演化的评估系统，以提供可持续且具有挑战性的智能体能力评估基础。

## 2. 论文提出的方法论

- **核心思想**：提出**TRACE框架**（Trajectory-based Validated-by-Reproducing Agent-benchmark Complexity Evolution），利用智能体在测试时的自由探索，将原始任务演化为更高难度的新任务，同时记录执行轨迹用于验证。
- **关键技术细节**：
  - 框架包含三个阶段：
    1. **进化提议挖掘（evolutionary proposal mining）**：通过初步探索和发散思维生成任务演化提议。
    2. **自由探索式问题构建（problem construction via free exploration）**：将提议实例化为具体问题实例，并在探索过程中记录执行轨迹。
    3. **多级验证（multi-level validation）**：确保演化后的任务具有可复现、逻辑连贯的轨迹。
- **流程文字说明**：原始任务→智能体自由探索→生成更高难度任务→记录轨迹→多级验证→形成可验证的新基准。
- **无公式或算法伪代码**（论文未提供详细信息，仅描述框架流程）。

## 3. 实验设计

- **使用的数据集/场景**：
  - 主要评测基准：**GAIA**。
  - 额外适应和改进的推理基准：**AIME-2024**。
- **Benchmark**：原始基准任务作为起点，TRACE框架生成演化任务。
- **对比方法**：未明确列出具体对比方法（如直接使用原始基准或人工扩展），但实验通过显示任务复杂度提升和正确性可靠性改进来验证有效性。

## 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及使用了多少GPU、训练时长或具体算力配置。仅给出代码仓库链接，未提供算力信息。

## 5. 实验数量与充分性

- **实验数量**：主要在GAIA和AIME-2024两个基准上进行了实验，验证了TRACE框架能持续提升任务复杂度并改善轨迹级验证的可靠性。
- **充分性评价**：实验覆盖了通用智能体基准（GAIA）和推理基准（AIME-2024），表明框架的适应性和有效性。但缺乏与其他自演化方法的对比实验，消融实验数量不详。由于论文是接收短文（ICLR 2026 Accepted），实验规模可能有限，但针对核心假设提供了基本验证。

## 6. 论文的主要结论与发现

- TRACE框架能够从原始任务出发，通过智能体自由探索生成难度更高的任务，并确保任务的可解性和轨迹的可验证性。
- 在GAIA基准上，框架一致地提升了任务复杂度，同时提高了正确性可靠性（通过轨迹验证）。
- 框架成功适应并改进了推理基准AIME-2024。
- 标志着从静态人工基准到动态自演化评估系统的范式转变。

## 7. 优点

- **创新性**：首次提出基于智能体测试时探索的自演化基准框架，解决了基准迅速饱和的痛点。
- **可验证性**：通过记录执行轨迹和多级验证，确保演化任务的质量和可复现性，避免了传统自动生成任务可能引入的噪声。
- **动态性**：基准可以随智能体进步而持续演化，延长了基准的生命周期。
- **通用性**：适用于不同类型的基准（通用任务和推理任务）。

## 8. 不足与局限

- **实验覆盖有限**：仅在GAIA和AIME-2024两个基准上测试，缺乏更多样化的领域（如代码生成、多模态、工具使用等）验证。
- **对比基线不足**：未与现有自动生成任务方法（如反向翻译、对抗性过滤等）进行系统性对比，难以评估相对优势。
- **算力与成本未讨论**：自演化过程需要智能体多次探索，计算开销可能较大，但论文未分析。
- **偏差风险**：演化任务可能偏向于智能体已擅长的方向，导致评估不够全面；框架本身依赖当前智能体的探索能力，可能存在“自我强化”偏差。
- **应用限制**：框架要求任务能够通过执行轨迹验证可解性，不适用于主观或开放式任务。

（完）
