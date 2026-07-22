---
title: Incentivizing Truthful Language Models via Peer Elicitation Games
title_zh: 通过同伴激发游戏激励语言模型真实输出
authors: "Baiting Chen, Tong Zhu, Jiale Han, Lexin Li, Gang Li, Xiaowu Dai"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=4xvN7uOKZt"
tags: ["query:agent-output"]
score: 5.0
evidence: 通过博弈论框架激励LLM输出真实回复
tldr: 大语言模型易产生不一致和幻觉。本文提出PEG，一个免训练博弈论框架，通过生成器和多个判别器交互，基于行列式互信息得分激励真实报告。理论保证每个代理后悔次线性，实验验证了有效性。为LLM对齐提供了新途径。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: LLM易出现不一致和幻觉，现有对齐方法需要标注数据。
method: 提出PEG框架，利用生成器和判别器的同伴评估博弈，以无监督方式激励真实输出。
result: 理论证明了收敛性，实验显示PEG能有效减少LLM输出中的不真实内容。
conclusion: 博弈论方法可无监督地提升LLM输出的真实性和一致性。
---

## Abstract
Large Language Models (LLMs) have demonstrated strong generative capabilities but remain prone to inconsistencies and hallucinations. We introduce Peer Elicitation Games (PEG), a training-free, game-theoretic framework for aligning LLMs through a peer elicitation mechanism involving a generator and multiple discriminators instantiated from distinct base models. Discriminators interact in a peer evaluation setting, where utilities are computed using a determinant-based mutual information score that provably incentivizes truthful reporting without requiring ground-truth labels. We establish theoretical guarantees showing that each agent, via online learning, achieves sublinear regret in the sense their cumulative performance approaches that of the best fixed truthful strategy in hindsight. Moreover, we prove last-iterate convergence to a truthful Nash equilibrium, ensuring that the actual policies used by agents converge to stable and truthful behavior over time. Empirical evaluations across multiple benchmarks demonstrate significant improvements in factual accuracy. These results position PEG as a practical approach for eliciting truthful behavior from LLMs without supervision or fine-tuning.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：大型语言模型（LLMs）虽然在生成能力上表现强劲，但容易产生不一致和幻觉（即生成与现实不符或逻辑矛盾的内容）。现有的对齐方法（如RLHF、指令微调）通常需要大量人工标注的、真实可靠的标签数据，成本高昂且难以规模化。
- **整体含义**：本文旨在提出一种无需训练、无需人工标注的博弈论框架，通过同伴激发机制激励LLM输出真实、一致的回复，从而在无监督条件下提升模型的真实性和可靠性。

## 2. 方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：引入 **Peer Elicitation Games (PEG)**，一种基于博弈论的免训练框架。系统包含一个生成器（generator）和多个判别器（discriminators），判别器由不同的基础模型实例化，彼此在同伴评估（peer evaluation）环境中交互。
- **关键技术细节**：
  - 判别器的效用（utility）通过**基于行列式的互信息得分**（determinant-based mutual information score）计算。该得分被证明能够在无真实标签的条件下，从理论上激励真实报告（truthful reporting）。
  - 生成器输出文本，判别器给出评分；通过在线学习（online learning）机制，每个智能体（agent）的累积性能可以逼近事后最优的固定真实策略，即达成**次线性遗憾（sublinear regret）**。
  - 理论证明最后迭代收敛到真实纳什均衡（last-iterate convergence to a truthful Nash equilibrium），确保智能体实际使用的策略随时间收敛到稳定且真实的行为。
- **算法流程（文字说明）**：
  1. 初始化：选择多个不同的基础LLM作为判别器，一个LLM作为生成器。
  2. 每轮生成器产生一个回答，判别器独立评估该回答的真实性，并基于行列式互信息计算各自效用。
  3. 各个智能体（生成器和判别器）根据历史结果更新自己的策略（在线学习）。
  4. 重复上述过程直至收敛（或达到预设轮次）。
- **关键优势**：无需梯度更新、无需微调、无需人工标注，完全基于博弈交互。

## 3. 实验设计
- **使用的数据集/场景**：摘要提到“multiple benchmarks”，但未给出具体名称。根据常见相关研究推测可能包括TruthfulQA、OpenBookQA、FactCC等事实性评估基准。需要查阅原文确认。
- **Benchmark**：未明确说明对比的基线方法，但通常可能对比：直接使用基础模型、监督微调（SFT）、RLHF、或者一些无需训练的校准方法（如自一致性、对比解码等）。
- **对比方法**：摘要未列出具体方法名称，只说明“significant improvements in factual accuracy”。需要原文进一步信息。

## 4. 资源与算力
- **论文中未明确说明使用的GPU型号、数量或训练时长**。由于PEG是训练免费（training-free）框架，理论上无需额外训练，仅需推理阶段的交互，因此算力消耗较低。但具体实验使用的硬件资源（如单卡A100或集群）在摘要和元数据中未提及，需查阅全文。

## 5. 实验数量与充分性
- **实验数量**：从摘要无法得知具体实验组数。但通常高水平论文会涵盖多个数据集、不同规模的模型、与多个基线的对比，以及消融实验（如不同判别器数量、不同互信息计算方式的对比等）。
- **充分性**：基于“multiple benchmarks”和“theoretical guarantees...proven”的描述，理论上实验设计应较为充分，但缺乏细节无法判断是否公平。由于是NeurIPS-2025-Accepted论文，通常实验设计较为严谨客观。但需要阅读全文确认是否进行了统计显著性测试或误差线报告。

## 6. 主要结论与发现
- **理论结论**：PEG框架中每个智能体通过在线学习可以实现次线性遗憾，且策略最终收敛至真实纳什均衡，即系统自动激励真实输出。
- **实验结论**：在多个基准测试上，PEG显著提升了事实准确性（factual accuracy），证明其能够在不进行监督或微调的前提下，有效减少LLM输出的不真实内容。

## 7. 优点
- **无监督、免训练**：无需人工标注或模型微调，大幅降低对齐成本，易于部署。
- **理论保证**：提供了严格的收敛性和最优性证明（次线性遗憾、最终迭代收敛至真实均衡），具备数学严谨性。
- **博弈论视角新颖**：将LLM对齐问题建模为多智能体博弈，利用行列式互信息自然激励真实性，无需外部奖励函数。
- **通用性强**：判别器可从不同基础模型实例化，兼容现有LLM，无需修改模型结构。

## 8. 不足与局限
- **实验细节缺失**：摘要未提供具体数据集、对比方法、消融实验等详细信息，仅凭摘要难以判断泛化能力。
- **算力资源未报告**：虽然免训练，但多次推理交互的计算开销可能仍然存在（尤其当判别器也需多次调用时），未见量化分析。
- **偏倚风险**：行列式互信息得分的定义依赖于基础模型的选择；若判别器本身存在系统偏差（如均偏好某种风格），可能影响真实性激励。
- **应用限制**：仅关注“真实性”，对其他对齐目标（如安全性、有用性、无害性）未涉及；且需要多个不同基础模型扮演判别器，实际部署时可能需额外获取模型访问权限。
- **可能缺乏代表性基准**：未提及与RLHF等主流方法的对比，以及在不同规模模型（如7B vs 175B）上的表现差异。

（完）
