---
title: "TRAJECT-Bench:A Trajectory-Aware Benchmark for Evaluating Agentic Tool Use"
title_zh: TRAJECT-Bench：面向智能体工具使用的轨迹感知基准
authors: "Pengfei He, Zhenwei Dai, Bing He, Hui Liu, Xianfeng Tang, Hanqing Lu, Juanhui Li, Jiayuan Ding, Subhabrata Mukherjee, Suhang Wang, Yue Xing, Jiliang Tang, Benoit Dumoulin"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=TZWnWvsQ0X"
tags: ["query:agent-traj"]
score: 9.0
evidence: 精细评估工具使用轨迹的轨迹感知基准
tldr: TRAJECT-Bench提出轨迹感知的工具使用评估基准，弥补了仅关注最终答案的不足。它配套高保真可执行工具和生产级API，合成具有不同宽度（并行调用）和深度（链式依赖）的轨迹。除最终准确率外，还提供细粒度指标评估工具选择的正确性、参数设置和调用顺序。实验表明，该方法能有效区分不同模型在工具使用轨迹上的表现。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有评估仅关注最终答案，忽略工具使用轨迹的细节。
method: 构建轨迹感知基准，包含多样工具和细粒度评估指标。
result: 有效区分智能体在工具调用顺序和参数上的差异。
conclusion: 轨迹感知评估对理解智能体工具使用行为至关重要。
---

## Abstract
Large language model (LLM)-based agents increasingly rely on tool use to complete real-world tasks. While existing works evaluate the LLMs' tool use capability, they largely focus on the final answers yet overlook the detailed tool usage trajectory, i.e., whether tools are selected, parameterized, and ordered correctly. We introduce TRAJECT-Bench, a trajectory-aware benchmark to comprehensively evaluate LLMs' tool use capability through diverse tasks with fine-grained evaluation metrics. TRAJECT-Bench pairs high-fidelity, executable tools across practical domains with tasks grounded in production-style APIs, and synthesizes trajectories that vary in breadth (parallel calls) and depth (interdependent chains). Besides final accuracy, TRAJECT-Bench also reports trajectory-level diagnostics, including tool selection and argument correctness, and dependency/order satisfaction. Analyses reveal failure modes such as similar tool confusion and parameter-blind selection, and scaling behavior with tool diversity and trajectory length where the bottleneck of transiting from short to mid-length trajectories is revealed, offering actionable guidance for LLMs' tool use.

---

## 论文详细总结（自动生成）

# TRAJECT-Bench：面向智能体工具使用的轨迹感知基准 — 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现有基于大语言模型（LLM）的智能体在工具使用能力评估中，大多仅关注最终答案是否正确，而忽略了工具调用的**轨迹细节**——即：工具是否被正确选择、参数是否设置正确、调用顺序是否合理。这种粗粒度评估无法揭示智能体在工具使用过程中的真正行为模式与失败原因。
- **整体含义**：为了深入理解 LLM 智能体的工具使用能力，需要一种“轨迹感知”（trajectory-aware）的评估范式，既评估最终结果，也评估每一步工具调用的中间过程。TRAJECT-Bench 正是为此目标设计的基准。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：构建一个包含高保真、可执行工具的基准，并生成具有不同宽度（并行调用数量）和深度（链式依赖长度）的工具使用轨迹，从而对智能体的工具选择、参数设置和调用顺序进行细粒度评估。
- **关键技术细节**：
  - **工具构建**：配对实际领域（如生产级 API）的高保真可执行工具，确保评估环境接近真实应用场景。
  - **轨迹合成**：自动生成多样化轨迹，包括：
    - **广度（Breadth）**：某个步骤中并行调用的工具数量不同。
    - **深度（Depth）**：工具之间依赖关系的链式长度不同。
  - **评估指标**：除最终准确率外，额外提供细粒度诊断指标：
    - **工具选择正确性**：所选工具是否与预期一致。
    - **参数正确性**：工具调用时参数是否正确设置。
    - **依赖/顺序满足度**：工具调用顺序是否符合轨迹中的依赖关系。
  - **分析失败模式**：通过混淆矩阵等方法识别“相似工具混淆”、“参数盲目选择”等典型失败模式。

## 3. 实验设计：数据集 / 场景、基准、对比方法

- **数据集/场景**：论文自身构建了 TRAJECT-Bench 基准，包含多种生产风格 API 映射的实际任务。任务覆盖不同领域，工具类型多样。
- **基准**：TRAJECT-Bench 本身就是新提出的基准，用于评估多种 LLM 智能体的工具使用能力。
- **对比方法**：论文对比了不同 LLM（未明确列出具体模型名称，但应包含主流模型如 GPT-4、Llama 系列等）在相同轨迹上的表现，通过最终准确率和轨迹级指标进行对比。

## 4. 资源与算力

- **文中未明确说明**所使用的 GPU 型号、数量或训练时长。论文重点在于基准构建和评估，未提及训练/推理的算力开销。因此无法总结具体资源消耗。

## 5. 实验数量与充分性

- **实验规模**：论文主要在合成轨迹上进行了评估，涵盖了从短到中长度的轨迹（宽度和深度均有变化）。通过消融分析揭示了“短→中长度轨迹的瓶颈”。
- **充分性与公平性**：
  - 实验设计覆盖了轨迹的不同宽度和深度，能够系统性地测试模型在复杂度增加时的表现差异。
  - 提供了细粒度指标，不仅看最终准确率，还看工具选择、参数和顺序的正确性，因此评估更全面。
  - 但未明确报告在多个独立数据集上的重复实验次数，也没有详细说明随机种子或交叉验证设置，严格意义上的统计显著性检验未提及。总体而言，实验对于展示基准功能是充分的，但在严格科学验证上略显不足。

## 6. 论文的主要结论与发现

- **主要结论**：TRAJECT-Bench 能够有效区分不同 LLM 在工具使用轨迹上的表现，特别是在工具调用顺序和参数设置方面的差异。
- **具体发现**：
  - 模型在短轨迹上表现普遍较好，但从中等长度轨迹开始出现显著瓶颈——尤其是链式依赖和并行调用增加后，模型的工具选择准确率和参数正确性明显下降。
  - 典型失败模式包括：相似工具混淆（如选择功能相近但不同的工具）、参数盲目选择（忽略上下文，随意填入参数）。
  - 工具多样性对模型性能的影响呈现缩放规律（scaling behavior），工具越多、轨迹越长，性能下降越明显。

## 7. 优点：方法或实验设计上的亮点

- **轨迹感知评估的创新性**：首次系统性地引入工具使用轨迹的细粒度诊断，超越传统仅看最终答案的做法，更能揭示模型真实能力。
- **高保真可执行工具**：使用生产级 API 风格的工具，保证评估结果对实际应用有指导意义。
- **合成轨迹的多样性**：通过控制宽度和深度生成多种复杂度轨迹，能够有效测试模型在不同情形下的鲁棒性。
- **细粒度指标**：工具选择、参数正确性、依赖顺序满足度等指标可定位具体失败环节，提供可操作的改进方向。

## 8. 不足与局限

- **实验覆盖有限**：论文未披露对比的 LLM 种类和数量，也未提供跨领域、跨工具类别的实验结果，可能不足以证明基准的通用性。
- **合成轨迹与现实任务的距离**：虽然工具是高保真的，但轨迹是合成的，可能无法完全反映真实用户与工具交互的复杂性和噪音。
- **缺失统计严谨性**：未报告多次运行的标准差或置信区间，可能存在偶然性。
- **算力与可复现性信息不足**：未提及具体实验环境或代码开源情况，增加了复现难度。
- **应用限制**：当前基准主要关注单一智能体单轮或多轮工具调用，未涉及多智能体协作或在线学习场景。

（完）
