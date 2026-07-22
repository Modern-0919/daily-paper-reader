---
title: "How to Train Your LLM Web Agent: A Statistical Diagnosis"
title_zh: 如何训练你的LLM Web智能体：统计诊断
authors: "Dheeraj Vattikonda, Santhoshi Ravichandran, Emiliano Penaloza, Hadi Nekoei, Thibault Le Sellier de Chezelles, Megh Thakkar, Nicolas Gontier, Miguel Muñoz-Mármol, Sahar Omidi Shayegan, Stefania Raimondo, Xue Liu, Alexandre Drouin, Alexandre Piché, Alexandre Lacoste, Massimo Caccia"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=67xkPEM3bZ"
tags: ["query:agent-traj"]
score: 6.0
evidence: 训练Web智能体的统计诊断方法
tldr: 本文针对LLM web agent训练的可重复性和多步决策复杂性，提出两阶段流水线：模仿学习+on-policy微调。通过统计分析揭示种子和超参数敏感性，为agent轨迹测试与训练提供了方法论基础。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 开源Web agent性能落后于专有系统，训练面临可重复性和多步任务挑战。
method: 两阶段训练：从Llama 3.3 70B教师进行模仿学习，再通过GRPO在线微调。
result: 统计学分析揭示了关键训练因素，显著提升了开源agent性能。
conclusion: 为Web agent训练提供了稳健方法论，间接支持轨迹测试研究。
---

## Abstract
Large language model (LLM) agents for web interfaces have advanced rapidly, yet open-source systems still lag behind proprietary agents. Bridging this gap is key to enabling customizable, efficient, and privacy-preserving agents. Two challenges hinder progress: the reproducibility issues in RL and LLM agent training, where results often depend on sensitive factors like seeds and decoding parameters, and the focus of prior work on single-step tasks, overlooking the complexities of web-based, multi-step decision-making.

We address these gaps by providing a statistically driven study of training LLM agents for web tasks. Our two-stage pipeline combines imitation learning from a Llama 3.3 70B teacher with on-policy fine-tuning via Group Relative Policy Optimization (GRPO) on a Llama 3.1 8B student. Through 240 configuration sweeps and rigorous bootstrapping, we chart the first compute allocation curve for open-source LLM web agents. Our findings show that dedicating one-third of compute to teacher traces and the rest to RL improves MiniWoB++ success by 6 points and closes 60\% of the gap to GPT-4o on WorkArena, while cutting GPU costs by 45\%. We introduce a principled hyperparameter sensitivity analysis, offering actionable guidelines for robust and cost-effective agent training.

---

## 论文详细总结（自动生成）

# 如何训练你的LLM Web智能体：统计诊断 —— 中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：大型语言模型（LLM）在Web界面上的智能体（agent）技术发展迅速，但开源系统性能仍落后于专有方案。缩小这一差距对于实现可定制、高效且保护隐私的智能体至关重要。
- **核心问题**：两大挑战阻碍了进展——（1）强化学习和LLM智能体训练中的可重复性问题（结果对种子和解码参数等敏感因素高度依赖）；（2）先前工作重点放在单步任务上，忽略了Web环境下多步决策的复杂性。
- **整体含义**：本文通过提供一项基于统计驱动的研究，系统性地训练LLM智能体执行Web任务，旨在为开源Web智能体的稳健、低成本训练提供方法论基础。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：提出两阶段训练流水线，结合模仿学习（Imitation Learning）和在线策略微调（On-policy Fine-tuning），并通过统计分析揭示训练中的关键因素（如种子、超参数敏感性），从而提升性能并降低成本。
- **关键技术细节**：
  - **第一阶段：模仿学习（Imitation Learning）**：使用Llama 3.3 70B教师模型，通过其生成的轨迹（traces）对Llama 3.1 8B学生模型进行行为克隆。
  - **第二阶段：在线策略微调**：采用Group Relative Policy Optimization（GRPO）算法对学生模型进行在线强化学习微调。
- **算法流程**（文字说明）：
  1. 用教师模型（70B）生成多步Web任务轨迹数据。
  2. 用这些轨迹对学生模型（8B）进行监督学习（模仿学习）。
  3. 在模仿学习后的学生模型上，使用GRPO进行在线策略微调（环境反馈来自Web任务评估器）。
  4. 通过240组配置扫掠（configuration sweeps）以及严格的Bootstrap重抽样，绘制出首个开源LLM Web智能体的计算分配曲线（compute allocation curve）。
- **特点**：引入了原则性的超参数敏感性分析，提供可操作的训练指南。

## 3. 实验设计：使用了哪些数据集/场景，它的benchmark是什么，对比了哪些方法

- **数据集/场景**：
  - **MiniWoB++**：一个常用的Web操作基准，包含多种小型网页交互任务。
  - **WorkArena**：一个更贴近真实办公环境的Web任务基准。
- **Benchmark**：在两个基准上评估智能体的任务成功率。
- **对比方法**：
  - 与专有系统（如GPT-4o）进行了对比。
  - 内部对比：不同计算分配策略（不同比例的教师轨迹和RL计算）、不同种子、不同超参数配置。

## 4. 资源与算力

论文摘要中未明确说明具体的GPU型号、数量及训练时长。只提及：
- 使用了Llama 3.3 70B教师模型和Llama 3.1 8B学生模型。
- 进行了240组配置扫掠（configuration sweeps）和严格Bootstrap重抽样。
- 报告了GPU成本削减45%的相对收益，但未列出绝对硬件规格。

**注意**：若需要更详细的算力信息，需查阅论文全文。

## 5. 实验数量与充分性

- **实验数量**：共进行了240组配置扫掠（包括不同种子、超参数、计算分配比例等）。在此基础上，进行了Bootstrap重抽样统计分析。
- **充分性**：实验设计较为充分，覆盖了多种配置组合，并通过统计方法（Bootstrap）增强了可信度。但仅基于两个基准（MiniWoB++和WorkArena）的评估，在任务多样性上可能有限。
- **客观性与公平性**：采用了统计诊断方法，量化了种子和超参数敏感性，并通过计算分配曲线展示了效率。但与专有系统的对比可能存在不公平（如专有模型未公开训练细节），不过该对比旨在衡量差距而非直接比较。

## 6. 论文的主要结论与发现

- **关键发现**：将计算资源的三分之一用于教师轨迹生成（模仿学习），其余用于强化学习（RL），可在MiniWoB++上提升6个百分点的成功率，在WorkArena上缩小与GPT-4o之间60%的差距，同时GPU成本降低45%。
- **结论**：提供了一个原则性的、统计驱动的、成本效益高的开源Web智能体训练方法论，为未来Web智能体轨迹测试与研究奠定了基础。

## 7. 优点：方法或实验设计上的亮点

- **统计驱动**：通过240组扫掠和Bootstrap重抽样，系统揭示了训练中种子和超参数的敏感性，增强了结论的稳健性。
- **计算分配曲线**：首次为开源LLM Web智能体绘制计算分配曲线，直观展示了最佳资源分配策略。
- **两阶段流水线**：模仿学习+在线微调的组合有效利用教师知识，并通过GRPO在线优化，兼顾效率和性能。
- **实用指导**：提供了超参数敏感性分析，给出了可操作的建议（例如推荐的计算分配比例），对实际部署具有重要参考价值。
- **成本意识**：明确关注GPU成本削减，体现了实用导向。

## 8. 不足与局限

- **任务覆盖面**：仅使用了MiniWoB++和WorkArena两个基准，可能不足以全面反映真实Web场景的多样性（如动态页面、认证流程等）。
- **模型规模限制**：教师模型为70B，学生模型为8B，更大规模模型的训练行为可能不同，结论的泛化性待验证。
- **复现性风险**：尽管强调了统计诊断，但论文中未提供详细的种子和超参数列表（可能存在于全文），且RL训练对环境动态敏感，复现仍可能有挑战。
- **对比的局限性**：与GPT-4o的对比仅展示了部分差距的缩小，未完全达到专有系统水平；且专有系统可能使用了更多数据或更大模型。
- **应用限制**：方法依赖教师模型和特定RL算法（GRPO），在其他架构或任务上需重新适配。
- **算力细节缺失**：未明确GPU型号、数量及训练时长，不利于他人复现成本估算。

（完）
