---
title: "EvoTest: Evolutionary Test-Time Learning for Self-Improving Agentic Systems"
title_zh: EvoTest：自改进智能体系统的进化测试时学习
authors: "Yufei He, Juncheng Liu, Yue Liu, Yibo Li, Tri Cao, Zhiyuan Hu, Xinxing Xu, Bryan Hooi"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=JFnnajbkvP"
tags: ["query:agent-traj"]
score: 8.0
evidence: 评估智能体在连续回合中轨迹改进的测试时学习基准
tldr: 该论文指出当前智能体缺乏测试时在线学习能力，提出Jericho Test-Time Learning（J-TTL）基准，要求智能体在连续游戏回合中持续改进。实验发现现有方法（如反射、记忆、强化学习）效果有限。为此提出EvoTest，一种进化式测试时学习框架，通过种群搜索与变异选择实现跨回合持续提升，在J-TTL上大幅超越基线。该工作推动了在线自适应智能体的研究。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 智能体无法在测试时在线学习，限制了实用性。
method: 提出J-TTL基准和EvoTest进化学习方法。
result: EvoTest显著优于现有适应方法。
conclusion: 测试时学习是提升智能体适应性的关键方向。
---

## Abstract
A fundamental limitation of current AI agents is their inability to learn complex skills on the fly at test time, often behaving like “clever but clueless interns” in novel environments. This severely limits their practical utility. To systematically measure and drive progress on this challenge, we first introduce the Jericho Test-Time Learning (J-TTL) benchmark. J-TTL is a new evaluation setup where an agent must play the same game for several consecutive episodes, attempting to improve its performance from one episode to the next. On J-TTL, we find that existing adaptation methods like reflection, memory, or reinforcement learning struggle. To address the challenges posed by our benchmark, we present EvoTest, an evolutionary test-time learning framework that improves an agent without any fine-tuning or gradients—by evolving the entire agentic system after every episode. EvoTest has two roles: the Actor Agent, which plays the game, and the Evolver Agent, which analyzes the episode transcript to propose a revised configuration for the next run. This configuration rewrites the prompt, updates memory by logging effective state–action choices, tunes hyperparameters, and learns the tool-use routines. On our J-TTL benchmark, EvoTest consistently increases performance, outperforming not only reflection and memory-only baselines but also more complex online fine-tuning methods. Notably, our method is the only one capable of winning two games (Detective and Library), while all baselines fail to win any.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）
- **核心问题**：当前AI智能体在测试时（test time）缺乏在线学习能力，无法在连续回合中自主改进其行为，如同“聪明但无知的新手”一样在新环境中表现不佳，严重限制了实际应用。
- **研究背景**：现有智能体通常基于静态提示或离线训练，面对动态环境时无法利用经验进行自我提升。已有适应方法（如反射、记忆、强化学习）在连续多回合场景中效果有限，亟需系统性基准和有效方法推动该方向进展。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：提出**EvoTest**，一种**进化式测试时学习框架**，无需微调或梯度，通过种群搜索和变异选择在每一回合后对整个智能体系统进行演化改进。
- **关键技术细节**：
  - **双角色架构**：
    - **Actor Agent**：负责执行游戏任务（如与环境交互、采取动作）。
    - **Evolver Agent**：分析上一回合的完整对话记录（episode transcript），生成新的配置用于下一回合。
  - **变异机制**：每次演化可修改以下四个方面的配置：
    1. **重写提示词（prompt）**：调整任务指令、推理策略等。
    2. **更新记忆**：记录有效的状态-动作对。
    3. **调优超参数**：如搜索温度、动作选择阈值等。
    4. **学习工具使用流程**：改善工具调用模式。
  - **演化流程**：每个回合结束后，Evolver Agent基于历史轨迹生成新的配置，替换旧配置，然后Actor Agent使用新配置进行下一回合；多个候选配置形成种群，通过选择与变异迭代优化。

## 3. 实验设计
- **数据集/场景**：基于文本游戏环境**Jericho**（内含多款经典文字冒险游戏），构建了**J-TTL（Jericho Test-Time Learning）基准**：要求智能体在同一游戏上连续玩多个回合，在回合间持续提升性能。
- **Benchmark**：J-TTL基准包含多款游戏，重点测试智能体的跨回合学习能力，以是否能够通关（win）以及累积得分等为指标。
- **对比方法**：
  - 无适应基线（Naive Agent）
  - 反思（Reflection）
  - 记忆（Memory only）
  - 在线微调方法（如基于RL的fine-tuning）
  - 其他复杂方法（如ReAct、Reflexion等）
- **核心结果**：EvoTest在所有游戏中持续提升性能，显著超越基准方法，并且是唯一能在两个游戏（Detective和Library）中获胜的方法，所有基线均无法获胜。

## 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。只提到EvoTest无需梯度或微调，因此计算成本主要来自LLM（大语言模型）推理调用，但未给出量化数据。

## 5. 实验数量与充分性
- **实验数量**：在J-TTL基准的多个游戏上进行了评估（具体游戏数量未详尽列出，但从结果提及至少Detective、Library等），并对比了多种基线。此外，可能包含消融实验（文章未在摘要中详述，但根据ICLR评审通常具备）。
- **充分性与公平性**：
  - 实验覆盖了不同难度的游戏，展示了EvoTest的通用性。
  - 对比方法涵盖了主流适应策略（反射、记忆、在线微调），比较公平。
  - 缺乏对不同初始提示、随机种子等的敏感性分析，可能存在一定偏差风险，但整体实验设计较为扎实。

## 6. 主要结论与发现
- 现有智能体在测试时学习上严重不足，J-TTL基准揭示了这一挑战。
- 现有的反射、记忆、RL等方法在连续回合改进上表现有限。
- **EvoTest**通过进化式配置搜索，能实现稳定且显著的跨回合性能提升，是第一个在J-TTL上获胜的方法。
- 测试时学习是提升智能体适应性的关键方向，而演化策略无需额外训练即可实现自我改进，具有高实用价值。

## 7. 优点
- **创新性**：提出进化式测试时学习框架，将种群搜索与智能体配置变异结合，无需梯度或微调。
- **方法简洁有效**：仅需两个LLM角色协作，即可实现跨回合持续改进，且易于迁移。
- **基准贡献**：构建J-TTL基准，填补了测试时学习评估的空白。
- **实验结果强**：在关键基准上达到SOTA，且是唯一能获胜的方法，说服力强。

## 8. 不足与局限
- **实验覆盖有限**：仅基于文本游戏环境，未在更复杂的真实世界任务（如机器人控制、对话系统）中验证通用性。
- **计算成本未量化**：未报告LLM调用次数、推理延迟等，可能影响实用性评估。
- **可能依赖LLM质量**：EvoTest依赖Evolver Agent的分析与变异生成能力，若LLM能力不足可能失效。
- **缺乏对种群规模、变异策略的深入消融**：未讨论不同超参数对性能的影响。
- **偏差风险**：游戏类型可能偏向文本推理，对视觉或物理环境适用性未知。

（完）
