---
title: "AgentBreeder: Mitigating the AI Safety Risks of Multi-Agent Scaffolds via Self-Improvement"
title_zh: AgentBreeder：通过自我改进减轻多智能体脚手架的AI安全风险
authors: "J Rosser, Jakob Nicolaus Foerster"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=mlU9KqdZUS"
tags: ["query:agent-output"]
score: 6.0
evidence: 多智能体脚手架的安全评估
tldr: "本文关注多智能体脚手架的安全风险，提出AgentBreeder框架进行多目标自我改进的进化搜索。在安全基准上取得79.4%的平均提升，同时保持或提升能力分数，但也发现与能力优化并存的对抗性弱脚手架风险。该工作揭示了多智能体脚手架的风险，并提供了自动缓解方法。"
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 多智能体脚手架的安全性影响尚未被充分探索，需要自动化的安全改进方法。
method: 提出多目标自我改进的进化搜索框架，对脚手架进行安全优化。
result: "安全基准性能平均提升79.4%，能力指标维持或提升，但存在对抗性弱脚手架风险。"
conclusion: 多智能体脚手架具有安全风险，但可通过进化搜索自动缓解，同时需警惕潜在弱点。
---

## Abstract
Scaffolding Large Language Models (LLMs) into multi-agent systems often improves performance on complex tasks, but the safety impact of such scaffolds has not been thoroughly explored. We introduce AgentBreeder, a framework for multi-objective self-improving evolutionary search over scaffolds. We evaluate discovered scaffolds on widely recognized reasoning, mathematics, and safety benchmarks and compare them with popular baselines. In "blue" mode, we see a 79.4% average uplift in safety benchmark performance while maintaining or improving capability scores. In "red" mode, we find adversarially weak scaffolds emerging concurrently with capability optimization. Our work demonstrates the risks of multi-agent scaffolding and provides a framework for mitigating them. Code is available at \url{https://github.com/jrosseruk/AgentBreeder}.

---

## 论文详细总结（自动生成）

# AgentBreeder：通过自我改进减轻多智能体脚手架的AI安全风险

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：将大型语言模型（LLM）编排为多智能体系统（即“脚手架”，scaffolds）虽然能提升复杂任务的性能，但这种脚手架结构对AI安全的影响尚未被充分探索。
- **研究动机**：随着多智能体系统的广泛应用，其潜在的安全风险（如对抗性弱点的产生）急需自动化的缓解方法。现有研究多关注单智能体安全，缺乏对多智能体脚手架安全性的系统性评估和改进。
- **整体含义**：本文旨在揭示多智能体脚手架的安全风险，并提出一种自动化的自我改进框架（AgentBreeder），通过进化搜索来平衡能力与安全，从而降低风险。

## 2. 论文提出的方法论：核心思想、关键技术细节、算法流程

- **核心思想**：将脚手架视为可进化的个体，通过多目标优化进行自我改进，同时提升安全性和能力。
- **关键技术细节**：
  - **演化搜索（Evolutionary Search）**：对脚手架的结构或超参数进行变异和交叉，生成新的群体。
  - **多目标优化**：同时考虑“能力得分”（如推理、数学基准性能）和“安全得分”（如安全性基准性能），避免单一目标优化带来的安全退化。
  - **双模式运行**：
    - **“蓝色模式”（Blue mode）**：以提升安全为主要目标，同时维持或提升能力。
    - **“红色模式”（Red mode）**：以优化能力为主要目标，但可能发现对抗性弱脚手架（即安全性能反而下降）。
- **算法流程（文字说明）**：
  1. 初始化一组脚手架（可能基于常见基线）。
  2. 在每一代中，对每个脚手架进行变异（如修改提示、任务分配策略等），生成子代。
  3. 在每个子代上运行多智能体任务，记录其在安全基准和能力基准上的性能。
  4. 使用帕累托前沿或多目标排序选择出同时具有高能力与高安全（蓝色模式）或高能力（红色模式）的脚手架。
  5. 重复多代，直到收敛或达到预定义迭代次数。
- **公式**：摘要中未提供具体公式，但可推断使用类似NSGA-II的多目标优化算法。

## 3. 实验设计：数据集、场景、基准、对比方法

- **使用的基准（Benchmark）**：
  - **能力基准**：广泛认可的推理（reasoning）和数学（mathematics）基准。
  - **安全基准**：多种安全测试集（原文提及“widely recognized safety benchmarks”），但未具体列出名称。
- **对比方法**：与流行的基线脚手架（baselines）进行比较，但未指明具体基线名称（如ReAct、AutoGPT等可能作为对比）。
- **实验场景**：多智能体协作完成复杂任务，可能涉及对话、工具使用等。
- **评估指标**：安全基准性能提升百分比（蓝色模式平均提升79.4%），能力分数维持或提升。

## 4. 资源与算力

- **未明确说明**：论文元数据和摘要中未提及GPU型号、数量、训练时长等具体算力信息。仅可推断实验基于LLM API调用或本地模型，但未提供细节。需要读者查阅原文或代码仓库（https://github.com/jrosseruk/AgentBreeder）获取更详细计算资源。

## 5. 实验数量与充分性

- **实验数量**：摘要未提供具体实验组数或消融实验细节。仅给出在蓝色模式下安全基准平均提升79.4%，并同时报告了红色模式的对抗性弱点发现。
- **充分性与客观性**：
  - 优势：同时报告了蓝色和红色模式的结果，显示安全与能力的权衡，具有一定客观性。
  - 不足：缺乏消融实验（如进化代数影响、不同变异策略影响）、不同安全基准的细分结果、与更多基线的对比等，实验充分性可能有限。由于是NeurIPS 2025接收论文，预计正文包含更多实验，但摘要中信息不足以全面评估。

## 6. 论文的主要结论与发现

- 多智能体脚手架确实存在安全风险，在能力优化过程中可能产生对抗性弱脚手架（红色模式）。
- AgentBreeder框架能够在蓝色模式下平均提升安全基准性能79.4%，同时保持或提升能力分数，证明自动化的自我改进方法可以缓解安全风险。
- 然而，安全与能力之间存在张力：刻意优化能力可能无意中产生安全弱点，需要警惕。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：将进化搜索应用于多智能体脚手架的安全优化，首次系统性地研究多目标自我改进在安全场景下的应用。
- **双模式分析**：同时考虑蓝色（安全优先）和红色（能力优先）模式，揭示安全与能力之间的对抗关系，具有洞察力。
- **自动化**：无需人工干预即可自动发现并缓解安全风险，具备实用潜力。
- **代码开源**：提供GitHub仓库，便于复现和扩展。

## 8. 不足与局限

- **实验覆盖有限**：仅提及推理和数学基准，未涉及其他领域（如代码生成、对话安全等）；安全基准的具体构成未说明，可能偏向特定类型攻击。
- **未处理算力开销**：进化搜索需要多代评估，计算成本较高，论文未讨论实际部署的可行性。
- **对比基线不明确**：没有列举具体对比的脚手架方法，难以判断提升幅度是否显著。
- **偏差风险**：进化搜索可能过度适应特定安全基准，导致在未见过的场景中表现不稳定（分布外泛化问题）。
- **应用限制**：框架假设脚手架结构可变异，对于固定或黑盒系统可能不适用；且可能无法完全消除对抗性弱点，仅提供缓解。

（完）
