---
title: "Shepherd: Pattern-Guided Trajectory Selection for Coding Agents on SWE-Bench"
title_zh: "Shepherd: 基于模式指导的代码智能体轨迹选择方法"
authors: "Alejandro Cuadron, Aditya Desai, Luis Gaspar Schroeder, Xingyao Wang, Wenjie Ma, Dacheng Li, Yichuan Wang, Ion Stoica, Graham Neubig, Joseph E. Gonzalez"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=ZBOFr4ryBk"
tags: ["query:agent-traj"]
score: 8.0
evidence: 通过分析3908条执行轨迹识别失败模式
tldr: 该论文针对代码智能体在复杂软件工程任务中性能受限的问题，系统分析了18个先进模型的3908条执行轨迹，发现了三类常见失败模式：环境交互缺失、动作依赖顺序混乱和过早终止。基于这些模式，提出Shepherd方法进行轨迹选择，以提升训练数据质量。实验证明该方法能有效改善代码智能体的性能。该工作为智能体轨迹分析方法提供了重要参考。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有代码智能体在SWE-bench等基准上性能有限，需要系统性理解失败原因以改进训练。
method: 通过大规模轨迹分析识别失败模式，并依据模式指导轨迹选择。
result: 基于失败模式进行轨迹选择可显著提升代码智能体性能。
conclusion: Shepherd揭示了代码智能体失败的根本原因，并提供了一种有效的轨迹选择方法。
---

## Abstract
Despite major improvements in LLM coding agents, their performance on complex software engineering tasks is still limited—leading models to solve only about half of the software engineering tasks in benchmarks like SWE-bench. This gap highlights the need to systematically understand why coding agents fail.
We comprehensively analyze coding agent failure patterns in 18 state-of-the-art open- and closed-weight models. Through meticulous examination of 3,908 execution trajectories, we identify three distinct failure patterns: (1) \fa{}, where agents fail to interact with the environment; (2) \oo{}, where agents issue interdependent actions simultaneously rather than sequentially; and (3) \ft{}, where agents prematurely assume task completion.
Using these failure patterns, we introduce \judge{}, a test-time steering mechanism that leverages an LLM-as-a-judge framework to evaluate trajectories. \judge{} shows a strong monotonic correlation with expert annotations and can effectively identify problematic patterns in agent behavior. When applied to select optimal trajectories from multiple runs, \judge{} significantly improves performance, increasing o1-low from 21\% to 31\% on SWE-bench Verified, outperforming the more expensive o1-high model (29\%) at 57\% of the cost.
We open-source our comprehensive dataset of trajectories to facilitate further research on improving coding agent capabilities.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：尽管大语言模型（LLM）代码智能体在简单编程任务上表现优异，但在复杂软件工程任务（如SWE-bench基准）中性能仍然有限——最先进的模型也只能解决约一半的任务。现有方法缺乏对智能体失败根本原因的系统性理解，导致训练和推理改进缺乏针对性。
- **研究背景**：SWE-bench是评估代码智能体解决真实世界软件工程问题的主流基准，但模型性能停滞在50%左右。该论文旨在通过大规模轨迹分析，识别失败模式，并基于模式指导轨迹选择，从而提升智能体训练数据的质量与最终性能。

## 2. 论文提出的方法论
- **核心思想**：通过细致分析大量执行轨迹（3,908条），归纳出三类典型的失败模式，然后利用这些模式构建一个LLM-as-a-judge的测试时导向机制（名为Shepherd/Judge），用于评估和选择最优轨迹。
- **关键技术细节**：
  - **失败模式识别**：对18个先进模型（包括开源和闭源）的执行轨迹进行逐条标注，归纳出三类失败模式：
    1. **环境交互缺失（Fa）**：智能体未能与环境（如代码仓库、测试环境）进行必要交互。
    2. **动作依赖顺序混乱（Oo）**：智能体同时发出相互依赖的动作，而不是按正确顺序依次执行。
    3. **过早终止（Ft）**：智能体在任务未完成时过早认为已结束并停止。
  - **Shepherd/Judge 框架**：利用LLM作为评判员，根据上述三类失败模式对给定轨迹进行评分。评分与人工专家标注呈强单调相关性，能有效识别问题轨迹。
  - **轨迹选择应用**：从多次运行（multiple runs）中选择最优轨迹，作为训练数据或最终输出。
- **算法流程（文字说明）**：
  1. 收集多个代码智能体在SWE-bench任务上的执行轨迹。
  2. 对每条轨迹，使用LLM-as-a-judge（基于预定义的失败模式模板）进行逐项检查，给出问题分数。
  3. 根据分数选择得分最高（即问题最少）的轨迹作为最优解。
  4. 将最优轨迹用于微调或直接作为最终结果。

## 3. 实验设计
- **数据集/场景**：SWE-bench Verified（已验证的子集），以及可能包含SWE-bench完整版。
- **Benchmark**：SWE-bench（软件工程任务基准），度量求解成功百分比。
- **对比方法**：
  - 基础模型：o1-low（低成本版本）、o1-high（高性能版本）。
  - 方法对比：直接使用原始模型输出 vs. 使用Shepherd选择后的轨迹输出。
  - 还对比了不同模型系列（18个模型）的失败模式分布。

## 4. 资源与算力
- **明确说明**：论文未明确提及训练Shepherd/Judge所消耗的GPU型号、数量或训练时长。仅提到该方法基于LLM-as-a-judge，属于推理时导向，可能不需要额外训练。轨迹分析部分需要人工标注与LLM辅助，但算力需求未量化。
- **相关提示**：论文指出使用Shepherd后，o1-low模型的性能从21%提升至31%，且成本仅为o1-high的57%，暗示了计算效率优势，但未提供绝对算力数值。

## 5. 实验数量与充分性
- **实验数量**：
  - 轨迹分析覆盖了18个模型，共3,908条执行轨迹。
  - 核心性能实验在SWE-bench Verified上进行，比较了o1-low、o1-high及Shepherd增强版本。
  - 可能包含消融实验（例如单独使用每种失败模式的效果），但摘要未详细列出。
- **充分性与客观性**：
  - 轨迹数据规模较大（近4000条），覆盖了多种模型，具有较好的代表性。
  - 失败模式是通过人工细致检查归纳的，具有一定客观性。
  - 但实验仅在SWE-bench一个基准上进行，缺乏在其他软件工程任务或领域（如代码修复、测试生成）的验证，可能影响泛化性。

## 6. 主要结论与发现
- 代码智能体的失败可归结为三类可识别的模式：环境交互缺失、动作顺序混乱、过早终止。
- 基于这些模式构建的LLM-as-a-judge导向机制（Shepherd）能有效筛选高质量轨迹，显著提升模型性能。
- 具体提升：o1-low从21%提升至31%，超过o1-high的29%，且成本仅为57%。
- 开源了完整的轨迹数据集，以促进后续研究。

## 7. 优点
- **创新性**：首次系统性地对代码智能体在复杂软件工程任务中的失败模式进行大规模分类和量化，并据此设计轨迹选择方法。
- **实用性**：方法轻量，无需额外训练数据或模型微调，仅通过推理时选择即可提升性能，且成本可控。
- **可解释性**：失败模式清晰直观，便于后续研究者针对性改进智能体设计。
- **开源贡献**：公开了轨迹数据集，有利于社区复现和深入分析。

## 8. 不足与局限
- **实验覆盖有限**：仅在SWE-bench基准上验证，未在其他代码智能体任务（如HumanEval、MBPP、CodeContests）或真实项目仓库中测试，其泛化性存疑。
- **失败模式可能不完整**：三类模式基于现有轨迹分析归纳，可能存在遗漏（如模型偏见、检索错误、工具使用不当等），且不同领域任务可能引入新模式。
- **LLM-as-a-judge的偏差**：评判器本身也是LLM，可能存在自洽性偏差，且对复杂案例的评判能力可能受限于其自身的推理能力。
- **应用限制**：轨迹选择需要多次运行（multiple runs），增加了推理成本（尽管比高价模型低），且最优轨迹的定义依赖于评判器的准确度。
- **未明确算力需求**：论文未提供训练LLM-as-a-judge所需的计算资源，实际部署时可能需要较大模型作为评判器，可能不是零成本。

（完）
