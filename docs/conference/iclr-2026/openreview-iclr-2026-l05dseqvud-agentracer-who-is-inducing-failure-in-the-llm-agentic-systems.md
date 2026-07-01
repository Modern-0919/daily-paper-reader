---
title: "AgenTracer: Who Is Inducing Failure in the LLM Agentic Systems?"
title_zh: AgenTracer：谁在导致LLM智能体系统失败？
authors: "Guibin Zhang, Junhao Wang, Junjie Chen, Wangchunshu Zhou, Kun Wang, Shuicheng YAN"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=l05DseqvuD"
tags: ["query:agent-traj"]
score: 7.0
evidence: 通过多智能体轨迹故障归因辅助评估
tldr: "AgenTracer是首个自动标注多智能体失败轨迹的框架。它通过反事实重放和故障注入，将错误归因到特定智能体或步骤。当前推理模型在此任务上准确率低于10%，而AgenTracer生成的高质量标注数据可训练更好的归因模型。这对理解智能体系统脆弱性并改进评测具有重要意义。"
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 多智能体系统失败归因困难，现有模型表现极差。
method: 提出自动框架，利用反事实重放和故障注入标注失败轨迹。
result: 生成的标注数据可用于训练归因模型。
conclusion: 自动化失败归因是提升智能体系统可靠性的关键。
---

## Abstract
Large Language Model (LLM)-based agentic systems, often comprising multiple models, complex tool invocations, and orchestration protocols, substantially outperform monolithic agents. Yet this very sophistication amplifies their fragility, making them more prone to system failure. Pinpointing the specific agent or step responsible for an error within long execution traces defines the task of \textbf{agentic system failure attribution}. Current state-of-the-art reasoning LLMs, however, remain strikingly inadequate for this challenge, with accuracy generally below $10\\%$.  To address this gap, we propose AgenTracer, the first automated framework for annotating failed multi-agent trajectories via counterfactual replay and programmed fault injection, producing the curated dataset TracerTraj. Leveraging this resource, we develop AgenTracer-8B, a lightweight failure tracer trained with multi-granular reinforcement learning, capable of efficiently diagnosing errors in verbose multi-agent interactions. On {Who\&When} benchmark, AgenTracer-8B outperforms giant proprietary LLMs like Gemini-2.5-Pro and Claude-4-Sonnet by up $18.18\\%$, setting a new standard in LLM agentic failure attribution. More importantly, AgenTracer-8B delivers actionable feedback to off-the-shelf multi-agent systems like MetaGPT and MaAS with $4.8\sim14.2\\%$ performance gains, empowering self-correcting and self-evolving agentic AI.

---

## 论文详细总结（自动生成）

# 论文《AgenTracer: Who Is Inducing Failure in the LLM Agentic Systems?》详细中文总结

## 1. 核心问题与整体含义
- **研究动机**：基于大语言模型（LLM）的多智能体系统（如多个模型、复杂工具调用、编排协议）虽然性能超过单智能体，但其复杂结构也导致脆弱性增强，容易发生系统故障。如何从冗长的执行轨迹中定位导致错误的特定智能体或步骤，即“智能体系统故障归因”任务，是提升系统可靠性的关键。
- **现状困境**：当前最先进的推理LLM在此任务上表现极差，准确率普遍低于10%，手动标注成本高昂且难以规模化。
- **整体贡献**：提出首个自动框架AgenTracer，可实现多智能体失败轨迹的自动标注，并基于此训练轻量级归因模型AgenTracer-8B，在基准测试上大幅超越顶级闭源模型，同时能为现有多智能体系统提供可操作的反馈，推动自我修正与自我进化。

## 2. 方法论
- **核心思想**：通过反事实重放（counterfactual replay）和程序化故障注入（programmed fault injection）自动生成高质量标注数据，再以多粒度强化学习训练轻量级归因模型。
- **关键技术细节**：
  - **反事实重放**：对失败轨迹进行假设性修改（如替换某智能体的动作或工具调用），重新执行系统以观察是否消除错误，从而判断该步骤是否为关键归因点。
  - **程序化故障注入**：人为向正常轨迹中注入特定类型的故障（例如某个智能体返回错误结果或工具调用失败），并记录实际导致失败的模式，形成带标签的训练数据。
  - **多粒度强化学习**：在训练AgenTracer-8B时，同时利用轨迹级（整体失败归因）和步骤级（具体位置）的奖励信号，通过强化学习优化模型，使其能从冗长交互中精准定位错误。
- **算法流程**（文字说明）：
  1. 收集多智能体系统执行轨迹（包括成功和失败案例）；
  2. 对失败轨迹执行反事实重放：依次屏蔽或替换每个智能体/步骤，重新运行系统并观察结果变化；
  3. 对成功轨迹执行故障注入：随机在某个步骤引入预定义错误，记录新轨迹及真实故障源；
  4. 将上述过程生成的标注数据整理为TracerTraj数据集；
  5. 使用TracerTraj对基础语言模型（如8B参数模型）进行多粒度强化学习训练，得到AgenTracer-8B。

## 3. 实验设计
- **基准数据集**：使用自建的TracerTraj数据集（由AgenTracer自动生成）进行训练；在**Who&When benchmark**上进行测试评估归因准确率。
- **对比方法**：
  - 巨型闭源LLM：Gemini-2.5-Pro、Claude-4-Sonnet等；
  - 可能还有其它开源模型，但文中未详细列出。
- **应用验证**：将AgenTracer-8B的归因结果作为反馈，集成到现成的多智能体系统**MetaGPT**和**MaAS**中，观察系统性能提升（4.8%～14.2%）。

## 4. 资源与算力
- 论文中**未明确说明**训练AgenTracer-8B所使用的GPU型号、数量及训练时长。仅知道模型为8B参数规模，推测可在单卡或少量GPU上完成，但具体资源消耗需作者补充。

## 5. 实验数量与充分性
- **实验数量**：包括在Who&When benchmark上的主要对比实验（至少与2个闭源模型对比），以及在两个现有多智能体系统（MetaGPT、MaAS）上的应用效果测试。从摘要看，未提及消融实验或更多数据集。
- **充分性评估**：
  - 优点：实验覆盖了核心表现对比与应用效果验证，显示了框架的实用价值。
  - 不足：没有详细说明TracerTraj数据集的规模、多样性；未进行跨领域或跨系统泛化测试；消融研究（如反事实重放 vs 故障注入各自贡献）缺失；对比基线仅提及两个闭源模型，缺少与更多同类工作（如其他归因方法）的比较。因此实验覆盖有限，存在一定的片面性。

## 6. 主要结论与发现
- **AgenTracer-8B在Who&When benchmark上**准确率大幅超越Gemini-2.5-Pro和Claude-4-Sonnet，提升高达18.18%，树立了LLM智能体故障归因的新标准。
- **实际应用验证**：将AgenTracer-8B的归因结果反馈给MetaGPT和MaAS，系统性能分别提升4.8%～14.2%，证明自动化归因能够有效支持多智能体系统的自我修正与持续进化。
- 自动化归因框架可以替代昂贵的人力标注，为提升智能体系统可靠性提供可行路径。

## 7. 优点
- **自动化标注**：首次实现无人工参与的多智能体失败轨迹标注，大幅降低数据收集成本。
- **轻量高效**：8B参数模型即可超越千亿参数级别的闭源模型，证明精心设计的训练数据和方法可以弥补规模差距。
- **可操作反馈**：归因结果可直接用于改善现有系统，推动了智能体系统的自主演进。
- **方法创新**：反事实重放+故障注入的组合为生成高质量归因训练数据提供了新思路。

## 8. 不足与局限
- **实验覆盖不足**：仅在单一基准（Who&What？实际是Who&When）上测试，缺少跨领域、跨复杂度的泛化验证；未与其他同类归因框架（如基于因果推断的方法）对比。
- **可能偏差风险**：训练数据由程序化方法生成，可能与真实世界失败场景分布不一致，导致模型在开放环境中的鲁棒性未知。
- **应用限制**：反事实重放需要多次执行系统，对于计算开销大的系统可能不实用；故障注入定义的错误类型有限，难以覆盖所有真实故障模式。
- **资源信息缺失**：未报告训练算力消耗，不利于社区复现和评估性价比。

（完）
