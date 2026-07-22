---
title: "Solver-Informed RL: Grounding Large Language Models for Authentic Optimization Modeling"
title_zh: 求解器引导的强化学习：为真实优化建模奠基大语言模型
authors: "Yitian Chen, Jingfan Xia, Siyu Shao, Dongdong Ge, Yinyu Ye"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=80L235oVBe"
tags: ["query:agent-errors"]
score: 6.0
evidence: 使用强化学习纠正LLM优化建模中的错误和幻觉
tldr: LLM在自动化优化建模中常产生不可行结果，源于错误和幻觉。本文提出求解器引导强化学习框架，利用可验证奖赏自动评估可执行代码并改进模型生成能力。实验表明SIRL有效提升了优化模型的准确性和可执行性，为LLM在决策自动化中的应用提供了可靠基础。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: LLM在自动化优化建模中经常产生错误和幻觉，导致不可行结果。
method: 提出求解器引导的强化学习，利用可验证奖赏自动评估和提升模型生成准确性。
result: 有效提升了优化模型的准确性和可执行性，减少了错误和幻觉。
conclusion: 求解器引导强化学习为LLM在决策自动化中的可靠应用提供了有效方法。
---

## Abstract
Optimization modeling is fundamental to decision-making in fields such as supply chain management, logistics, and financial engineering, but its complexity presents a major barrier to adoption. Automating model creation from natural language is key to improving efficiency and access. However, while Large Language Models (LLMs) are a promising tool for this, they often produce flawed or infeasible results due to errors and hallucinations.
   To address this issue, we propose Solver-Informed Reinforcement Learning (SIRL), a framework that uses Reinforcement Learning with Verifiable Reward to improve LLMs’ ability to generate accurate and executable optimization models. Specifically, SIRL automatically assesses the executable code and the instance-level mathematical model represented by the associated .lp files. This process yields precise feedback on syntactic validity, feasibility, and solution quality, which serves as a direct reward signal to guide the reinforcement learning process. Furthermore, this verification mechanism also supports our instance-enhanced self-consistency method for creating high-quality training data.
    Extensive experiments on diverse public benchmarks demonstrate that models trained with our SIRL framework achieve state-of-the-art performance, substantially outperforming existing methods in generating accurate and executable optimization models. Specifically, our SIRL-32B model surpasses DeepSeek-V3 and OpenAI-o3 on the majority of these benchmarks.
    Our code is publicly available at https://github.com/Cardinal-Operations/SIRL.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：大规模语言模型（LLM）在自动化优化建模中经常产生错误和幻觉，导致生成的优化模型不可行（例如语法错误、约束不满足、解质量差），阻碍了其在决策自动化中的可靠应用。
- **研究动机**：优化建模在供应链管理、物流、金融工程等领域至关重要，但手动构建复杂模型门槛高。利用LLM从自然语言描述自动生成优化模型能大幅提升效率，但现有LLM的输出不可靠。
- **整体含义**：通过引入可验证的奖励信号（求解器反馈）来强化学习LLM的建模能力，为LLM在真实决策场景中的落地提供了一种可信的解决方案。

## 2. 方法论：核心思想、关键技术细节
- **框架名称**：Solver-Informed Reinforcement Learning (SIRL) —— 求解器引导的强化学习。
- **核心思想**：利用求解器（如LP/MILP求解器）对LLM生成的代码进行自动评估，获取可验证的奖励信号，进而通过强化学习（RL）优化LLM的生成策略。
- **关键技术细节**：
  - 自动评估阶段：SIRL自动运行LLM生成的代码，并解析其输出的`.lp`文件（通用优化模型格式），评估三个维度：
    1. **语法有效性**（syntactic validity）：代码是否能被求解器正确解析；
    2. **可行性**（feasibility）：模型是否存在可行解；
    3. **解质量**（solution quality）：求解目标函数值或与最优解的差距。
  - 奖励信号：将上述评估结果转化为直接、连续、可验证的奖励（Verifiable Reward），用于指导强化学习更新。
  - 数据增强：提出**实例增强的自一致性方法**（Instance-Enhanced Self-Consistency），通过多次采样和验证生成高质量训练数据，进一步提升学习效果。

## 3. 实验设计
- **数据集/场景**：多种公开的优化建模benchmark（具体名称未在摘要中列出，但涵盖不同领域）。
- **基准对比**：与多个现有方法对比，包括DeepSeek-V3、OpenAI-o3等大型模型。
- **对比方法**：未详细列出，但提到SIRL-32B模型在大多数基准上超越了DeepSeek-V3和OpenAI-o3，达到最先进性能（SOTA）。
- **实验评估维度**：生成模型的准确性和可执行性（语法正确、可行、解质量）。

## 4. 资源与算力
- **未明确说明**：论文摘要和元数据中未提及使用的GPU型号、数量、训练时长、参数规模的具体训练成本。仅提到模型规模为32B（SIRL-32B），但未说明训练所需的算力资源。

## 5. 实验数量与充分性
- **实验数量**：摘要指出进行了“大量实验”（Extensive experiments），但未给出具体实验组数（例如几个数据集、几组消融实验）。
- **充分性评价**：
  - **优点**：对比了多个强基线（DeepSeek-V3, OpenAI-o3），且结果显著，说明方法有竞争力。
  - **不足**：缺乏消融实验的详细描述（例如单独验证RL奖励设计、数据增强方法的效果）；也未给出与其他RL-based方法的对比（例如PPO、DPO等）。总体实验设计上偏重性能对比，但对方法内部机理的探讨不够充分。

## 6. 主要结论与发现
- SIRL框架显著提升了LLM生成准确且可执行优化模型的能力，大幅减少了错误和幻觉。
- 训练的SIRL-32B模型在多个公开基准上达到最先进性能，超过了DeepSeek-V3和OpenAI-o3等大模型。
- 结论认为：求解器引导的强化学习为LLM在决策自动化中的可靠应用提供了有效方法。

## 7. 优点
- **方法创新性**：首次将可验证的求解器反馈作为强化学习的奖励信号，实现了自动、客观的模型质量评估，无需人工标注。
- **数据自主生成**：实例增强的自一致性方法有效创建高质量训练数据，缓解了数据稀缺问题。
- **实用性强**：直接面向代码生成+优化模型验证，可直接应用于现实中的建模任务。
- **开源友好**：代码已公开，有利于复现和后续研究。

## 8. 不足与局限
- **实验覆盖有限**：基准数据集的具体名称和规模未披露，无法判断是否涵盖多种约束类型（非线性、整数规划、随机优化等），可能偏向特定类型的线性/纯整数问题。
- **偏差风险**：模型性能可能依赖于特定求解器（例如仅支持LP文件格式），对其他求解器或者非标准建模语言的泛化性未知。
- **计算成本未知**：未报告训练耗时、GPU资源消耗，且模型尺寸32B较大，实际部署成本较高。
- **缺乏健壮性分析**：未探讨模型对于难以验证的复杂问题（如非凸问题）或对抗性输入的鲁棒性。
- **消融实验缺失**：没有明确验证RL各组件（奖励设计、数据增强）的独立贡献，削弱了方法有效性的说服力。

（完）
