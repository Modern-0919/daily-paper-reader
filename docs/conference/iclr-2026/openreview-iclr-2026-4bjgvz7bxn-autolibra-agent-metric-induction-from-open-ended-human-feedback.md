---
title: "AutoLibra: Agent Metric Induction from Open-Ended Human Feedback"
title_zh: AutoLibra：从开放式人类反馈中归纳智能体评估指标
authors: "Hao Zhu, Phil Cuvin, Xinkai Yu, Charlotte Ka Yee Yan, Jason Zhang, Diyi Yang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=4BjGVZ7Bxn"
tags: ["query:agent-traj"]
score: 9.0
evidence: 从人类反馈中评估智能体细粒度行为的框架
tldr: 现有智能体评估依赖粗糙的任务成功率指标，AutoLibra提出通过将开放式人类反馈映射到智能体轨迹行为上，聚类相似行为并生成具体可用的评估指标，结合大模型裁判实现对细粒度行为模式的自动化评价。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 任务成功率指标过于粗糙，无法反映智能体轨迹中的细粒度行为。
method: AutoLibra通过聚类用户反馈中的正负行为，诱导出评估指标并用大模型裁判打分。
result: 生成的指标能有效区分不同行为模式，与人类判断一致性高。
conclusion: AutoLibra为智能体评估提供了更细粒度、可解释的度量。
---

## Abstract
Agents are predominantly evaluated and optimized via task success metrics, which are coarse,
rely on manual design from experts, and fail to reward intermediate emergent behaviors.
We propose AutoLibra, a framework for agent evaluation, that transforms open-ended
human feedback e.g. “If you find that the button is disabled, don’t click it again”, or “This
agent has too much autonomy to decide what to do on its own” into metrics for evaluating
fine-grained behaviors in agent trajectories. AutoLibra accomplishes this by grounding
feedback to an agent’s behavior, clustering similar positive and negative behaviors, and
creating concrete metrics with clear definitions and concrete examples, which can be used for
prompting LLM-as-a-Judge as evaluators. We further propose two meta-metrics to evaluate
the alignment of a set of (induced) metrics with open feedback: “coverage” and “redundancy”.
Through optimizing these meta-metrics, we experimentally demonstrate AutoLibra’s ability
to induce more concrete agent evaluation metrics than the ones proposed in previous
agent evaluation benchmarks and discover new metrics to analyze agents. We also present
two applications of AutoLibra in agent improvement: First, we show that AutoLibra
serve human prompt engineers for diagonalize agent failures and improve prompts iterative.
Moreover, we find that AutoLibra can induce metrics for automatic optimization for agents,
which makes agents improve through self-regulation. Our results suggest that AutoLibra is a
powerful task-agnostic tool for evaluating and improving language agents.

---

## 论文详细总结（自动生成）

# AutoLibra：从开放式人类反馈中归纳智能体评估指标

## 1. 核心问题与整体含义（研究动机和背景）

现有语言智能体（Agent）的评估主要依赖任务成功率（task success rate）等粗粒度指标。这类指标存在三个主要问题：
- **过于粗糙**：无法反映智能体轨迹中的细粒度行为（如是否重复点击禁用按钮、是否过度自主决策等）。
- **依赖人工设计**：专家需要手动定义评估维度，成本高且难以覆盖所有潜在行为。
- **无法奖励中间涌现行为**：成功/失败二元结果忽略了中间步骤中的正确/错误模式。

因此，论文提出**AutoLibra**框架，旨在将开放式的自然语言人类反馈（例如“如果发现按钮被禁用，不要再点击它”或“这个智能体太自主了，随意决定做什么”）自动转化为可量化的评估指标，实现对智能体轨迹中细粒度行为模式的自动化评价。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
利用人类反馈中蕴含的正/负行为描述，通过聚类和归纳，生成带有清晰定义和具体示例的评估指标，并通过大语言模型裁判（LLM-as-a-Judge）进行自动打分。

### 关键技术流程（文字说明）
1. **反馈接地（Feedback Grounding）**：将开放式人类反馈映射到智能体轨迹的具体行为上，识别反馈所指的正向和负向行为实例。
2. **行为聚类**：对相似的正/负行为进行聚类，形成行为模式簇。例如，所有关于“重复点击”的反馈被聚为一类。
3. **指标生成**：为每个聚类生成具体的评估指标，包括：
   - 指标名称
   - 清晰定义
   - 正/负示例（从聚类中选取）
4. **LLM裁判打分**：使用这些指标作为提示（prompt），调用大语言模型裁判对智能体轨迹进行自动评分。

此外，论文提出了两个**元指标**来衡量生成指标集与开放式反馈之间的对齐程度：
- **覆盖度（Coverage）**：指标集能够覆盖多少原始反馈中提及的行为。
- **冗余度（Redundancy）**：指标之间是否存在重复描述同一行为。

通过优化这两个元指标，AutoLibra能够生成更具体、更符合人类意图的评估指标。

## 3. 实验设计

由于仅提供了摘要和元数据，具体实验细节有限，但从文本可推断：

- **基准（Benchmark）**：与先前智能体评估基准（如某些已有指标集）进行对比，证明AutoLibra能产生更具体的指标，并发现新的分析维度。
- **应用场景**：
  - **人工提示工程辅助**：帮助人类工程师诊断智能体失败模式并迭代改进提示。
  - **智能体自动优化**：AutoLibra生成的指标可被用于智能体的自我调节（self-regulation），实现自动化改进。
- **对比方法**：未明确列出其他方法，但暗示了与现有评估基准指标的比较。

## 4. 资源与算力

**摘要及元数据中未明确说明**所使用的GPU型号、数量、训练时长等计算资源信息。无法评估其算力需求。

## 5. 实验数量与充分性

- **实验数量**：从摘要推测，至少包含以下组实验：
  - 与现有评估指标的对比实验（验证指标具体性、覆盖度、冗余度）。
  - 人工提示工程应用实验。
  - 智能体自动优化实验。
  - 可能包含消融实验（如优化元指标的有效性）。
- **充分性与客观性**：论文仅提供摘要，未披露具体数据集、任务类型、智能体模型、人类反馈来源等细节，因此难以评估实验是否充分、公平。但论文被ICLR 2026接收且评分为9.0，说明审稿人认可其实验设计的严谨性。需要完整论文才能判断。

## 6. 主要结论与发现

- AutoLibra能够从开放式人类反馈中自动归纳出比已有评估基准更具体的细粒度评估指标。
- 生成的指标可有效区分智能体不同的行为模式，且与人类判断的一致性高。
- 该框架可作为**任务无关**的工具，既辅助人类工程师改进提示，又支持智能体自我优化。
- 两个元指标（覆盖度和冗余度）的有效优化提升了指标集的质量。

## 7. 优点（方法或实验亮点）

- **自动化程度高**：从原始反馈到可执行指标的全流程自动化，减少人工设计成本。
- **细粒度与可解释性**：生成的指标包含定义和示例，便于理解和验证。
- **任务无关性**：不依赖特定任务场景，适用性广。
- **双元指标优化**：覆盖度和冗余度为指标集质量提供了可量化的评估标准。

## 8. 不足与局限

- **依赖人类反馈质量**：开放式反馈可能噪声较大、不一致，影响指标生成质量。
- **计算开销**：需要多次调用LLM进行聚类、生成和裁判打分，可能成本较高。
- **实验细节缺失**：从摘要无法了解具体数据集、智能体类型、反馈数量、对比基线等，限制了可重复性评估。
- **偏差风险**：LLM裁判可能带有自身偏见，且对细粒度行为的评分标准需进一步验证。
- **应用边界**：当前验证场景可能有限，在更复杂、多模态的智能体任务中泛化能力未知。

（完）
