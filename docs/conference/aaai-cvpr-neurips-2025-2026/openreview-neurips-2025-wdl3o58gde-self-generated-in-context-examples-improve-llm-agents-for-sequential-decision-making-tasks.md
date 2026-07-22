---
title: Self-Generated In-Context Examples Improve LLM Agents for Sequential Decision-Making Tasks
title_zh: 自我生成的上下文示例提升LLM智能体在序列决策任务中的表现
authors: "Vishnu Sarukkai, Zhiqiang Xie, Kayvon Fatahalian"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=WdL3O58gde"
tags: ["query:agent-errors"]
score: 5.0
evidence: 自我生成轨迹提升智能体在序列任务上的表现
tldr: "本文提出让LLM智能体自动从自身成功经验中学习，通过构建和精炼自我生成轨迹数据库作为未来任务的上下文示例，在ALFWorld、Wordcraft和InterCode-SQL三个基准上分别获得了73%到89%、55%到64%和75%到79%的性能提升。该方法与错误修正和轨迹优化相关，但主要关注改进而非检测。"
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: LLM智能体改进通常需要大量人工知识工程。
method: 自动积累成功轨迹作为上下文示例，无需干预。
result: 在三个基准上超越基线，性能提升显著。
conclusion: 自我生成示例是一种有效且无需人工的智能体改进方法。
---

## Abstract
Improving Large Language Model (LLM) agents for sequential decision-making tasks typically requires extensive task-specific knowledge engineering—custom prompts, curated examples, and specialized observation/action spaces. We investigate a different approach where agents automatically improve by learning from their own successful experiences without human intervention. Our method constructs and refines a database of self-generated trajectories that serve as in-context examples for future tasks. Even naive accumulation of successful trajectories yields substantial performance gains across three diverse benchmarks: ALFWorld (73\% to 89\%), Wordcraft (55\% to 64\%), and InterCode-SQL (75\% to 79\%). These improvements exceed those achieved by upgrading from gpt-4o-mini to gpt-4o and match the performance of allowing multiple attempts per task. We further enhance this approach with two innovations: database-level curation using population-based training to propagate high-performing example collections, and exemplar-level curation that selectively retains trajectories based on their empirical utility as in-context examples. With these enhancements, our method achieves 93\% success on ALFWorld—surpassing approaches that use more powerful LLMs and hand-crafted components. Our trajectory bootstrapping technique demonstrates that agents can autonomously improve through experience, offering a scalable alternative to labor-intensive knowledge engineering.

---

## 论文详细总结（自动生成）

# 论文总结：自我生成的上下文示例提升LLM智能体在序列决策任务中的表现

## 1. 核心问题与整体含义
- **研究动机**：当前提升LLM智能体在序列决策任务上的性能，通常需要大量任务特定的知识工程，如定制提示、精心挑选的示例、专门的观察与动作空间，这些方法人力成本高、可扩展性差。
- **核心问题**：能否让LLM智能体**自动从自身成功经验中学习**，无需人工干预即可改进性能？
- **整体含义**：本文提出一种**自我生成轨迹的自举方法**（trajectory bootstrapping），使智能体通过积累并精炼自身成功轨迹作为上下文示例，在多个基准任务上取得显著提升，甚至超越使用更强LLM和手工组件的方案，为智能体自主改进提供了一种可扩展的替代路径。

## 2. 方法论
- **核心思想**：构建并维护一个**自我生成的成功轨迹数据库**，在后续任务中将这些轨迹作为少样本上下文示例提供给LLM智能体，使其能参考自身过往成功经验。
- **关键技术细节**：
  - **朴素积累**：直接将所有成功轨迹加入数据库，不进行任何筛选。此方法已在ALFWorld（73%→89%）、Wordcraft（55%→64%）、InterCode-SQL（75%→79%）上取得显著提升。
  - **数据库级精选**：采用**基于种群的训练**（population-based training）迭代传播高性能的示例集合，自动搜索最优的轨迹子集。
  - **示例级精选**：根据每条轨迹作为上下文示例的实际效用（empirical utility）进行选择性保留，丢弃低效轨迹。
- **算法流程**（文字描述）：
  1. 初始化空数据库。
  2. 智能体在任务上运行，若成功则将该轨迹存入数据库。
  3. 在后续任务中，从数据库中检索相似轨迹作为上下文示例，提升新任务的成功率。
  4. 周期性运行数据库级精选和示例级精选，优化数据库内容。
- **无需人工干预**，无需额外标注或手工策划。

## 3. 实验设计
- **数据集/场景**：
  - **ALFWorld**：家居环境下的指令跟随任务（6个子类）。
  - **Wordcraft**：文字游戏任务（创意写作）。
  - **InterCode-SQL**：基于自然语言生成SQL查询的任务。
- **基准**：在各自领域已有的标准评估设置下进行，对比：
  - 基线模型（默认无上下文示例）。
  - 更强LLM（如从gpt-4o-mini升级到gpt-4o）。
  - 允许每任务多次尝试（multiple attempts）的设置。
  - 使用更强大LLM和手工组件的现有方法。
- **对比方法**：朴素积累、数据库级精选、示例级精选，以及它们的组合。

## 4. 资源与算力
- 论文未明确说明使用的GPU型号、数量或训练时长。仅提及使用GPT系列模型（gpt-4o-mini和gpt-4o）作为LLM后端，但未披露具体的计算资源消耗。因此无法量化算力需求。

## 5. 实验数量与充分性
- **实验数量**：
  - 在3个完全不同领域的基准上进行了测试（ALFWorld、Wordcraft、InterCode-SQL）。
  - 对比了多种变体：朴素积累 vs. 强LLM基线 vs. 多次尝试基线。
  - 对提出的两种精选策略（数据库级、示例级）进行了消融实验（提升至93%成功率的最终结果）。
- **充分性与公平性**：
  - 覆盖物体操作、文本生成、代码生成三类任务，多样性较好。
  - 与提升LLM模型规模、增加尝试次数这两种常见替代方案进行公平对比，成本可控。
  - 消融实验验证了精选策略的贡献。但缺乏更多基线的比较（如手工策划示例方法的具体数据），也未在更多类型智能体（如ReAct、Reflexion）上测试。

## 6. 主要结论与发现
- **自我生成的轨迹作为上下文示例有效**：即使不做任何精选，朴素积累成功轨迹也能在三个基准上带来7%至16%的绝对性能提升。
- **超越模型缩放和多次尝试**：此提升幅度超过将LLM从gpt-4o-mini升级到gpt-4o的效果，并可达到与允许每任务多次尝试相当的性能。
- **精选进一步增强**：通过数据库级和示例级精选，在ALFWorld上达到93%成功率，超越了使用更强LLM和手工组件的现有方法。
- **核心发现**：LLM智能体可以**自主通过经验改进**，提供了一种无需人力知识工程的可扩展替代方案。

## 7. 优点
- **自动化程度高**：完全无需人工标注或任务特定工程，仅依赖智能体自身的成功经验。
- **通用性强**：在三个差异极大的任务领域均有效，表明方法具有跨领域适用性。
- **性能显著**：在ALFWorld上达到93%，超越了依赖更强模型和手工组件的方案。
- **方法简洁**：核心思想简单（积累成功轨迹作为示例），但通过精选策略实现了显著增益。
- **公平比较**：直接与模型升级、多次尝试等常见改进手段对比，证明了方法的价值。

## 8. 不足与局限
- **实验覆盖有限**：仅在三个基准上验证，未在更多类型或更复杂的决策任务（如机器人控制、多轮对话等）上测试。
- **偏差风险**：智能体自身生成的轨迹可能包含系统性的错误模式，长期自举可能导致“自我强化”偏差（如仅在简单任务上成功，复杂任务失败）。
- **应用限制**：依赖LLM的上下文窗口大小，成功轨迹数量过多时可能无法全部纳入；检索相似轨迹的机制未详细说明，可能影响可复现性。
- **未讨论失败轨迹的利用**：仅使用成功轨迹，未能从失败中学习（如错误修正方法），可能限制了改进上限。
- **算力资源未报告**：无法评估方法的实际计算开销，特别是基于种群的训练可能引入额外成本。
- **缺乏与更多基线对比**：例如与手动构建示例、主动学习、强化学习等方法的全方位对比。

（完）
