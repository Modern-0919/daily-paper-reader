---
title: "AutoTool: Efficient Tool Selection for Large Language Model Agents"
title_zh: AutoTool：面向大语言模型智能体的高效工具选择
authors: "Jingyi Jia, Qinbin Li"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40389/44350"
tags: ["query:agent-errors"]
score: 7.0
evidence: 关注大语言模型智能体中的工具选择，与工具调用错误相关
tldr: 针对当前智能体框架中工具选择推理解释耗时的问题，提出基于图的AutoTool框架，利用工具使用惯性——工具调用遵循可预测顺序模式的观察，从历史智能体轨迹构建有向图，通过转移概率预测工具序列，避免重复调用LLM，显著降低推理成本。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40389/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 786, \"height\": 744, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40389/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1789, \"height\": 878, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40389/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 782, \"height\": 516, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40389/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1756, \"height\": 301, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40389/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1812, \"height\": 416, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40389/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 856, \"height\": 495, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40389/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1895, \"height\": 410, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40389/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1939, \"height\": 414, \"label\": \"Table\"}]"
motivation: 现有智能体框架如ReAct中工具选择时重复调用LLM导致高推理成本。
method: 构建基于历史轨迹的有向图，利用工具使用惯性预测工具序列，绕过重复LLM推理。
result: AutoTool在保持选择准确性的同时大幅降低了推理成本。
conclusion: 为LLM智能体提供了高效的工具选择方法，减少了计算开销。
---

## Abstract
Large Language Model (LLM) agents have emerged as powerful tools for automating complex tasks by leveraging the reasoning and decision-making abilities of LLMs. However, a major bottleneck in current agent frameworks lies in the high inference cost of tool selection, especially in approaches like ReAct that repeatedly invoke the LLM to determine which tool to use at each step. In this work, we propose AutoTool, a novel graph-based framework that bypasses repeated LLM inference by exploiting a key empirical observation: tool usage inertia—the tendency of tool invocations to follow predictable sequential patterns. AutoTool constructs a directed graph from historical agent trajectories, where nodes represent tools and edges capture transition probabilities, effectively modeling the inertia in tool selection. It further integrates parameter-level information to refine tool input generation. By traversing this structured representation, AutoTool efficiently selects tools and their parameters with minimal reliance on LLM inference. Extensive experiments across diverse agent tasks demonstrate that AutoTool reduces inference costs by up to 30% while maintaining competitive task completion rates, offering a practical and scalable enhancement for inference-heavy frameworks. Our work highlights the promise of integrating statistical structure into LLM agent design for greater efficiency without sacrificing performance.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在大语言模型（LLM）驱动的智能体框架（如ReAct）中，工具选择过程需要反复调用LLM进行推理，导致高昂的计算开销和延迟，成为实际部署的瓶颈。
- **整体含义**：现有方法普遍依赖LLM的推理能力来为每个步骤选择工具，但许多决策步骤实际上具有高度重复性和可预测性。论文提出利用**工具使用惯性**（tool usage inertia）——即工具调用遵循可预测的顺序模式——通过统计结构替代部分LLM推理，在保持任务完成率的同时大幅降低推理成本，为高效LLM智能体设计提供新思路。

## 2. 论文提出的方法论：核心思想、关键技术细节、算法流程

- **核心思想**：构建一个有向图（工具惯性图 TIG），记录历史智能体轨迹中工具间的顺序依赖关系和参数级数据流。在每次决策前，先尝试通过图搜索进行“惯性调用”以绕过LLM；若成功则直接执行，否则回退到标准LLM推理。
- **关键技术细节**：
  - **工具惯性图（TIG）**：层次化结构，包括工具节点（代表每个工具）和参数节点（代表工具的输入/输出参数）。边分为工具序列边（建模顺序依赖）和参数依赖边（建模数据流）。图在线增量构建，边权重基于执行反馈（成功/失败）动态更新，以区分有效和无效路径。
  - **惯性感知与工具选择**：使用**综合惯性潜力分数（CIPS）** 选择候选工具：
    \[
    \text{CIPS} = (1 - \alpha) \cdot \text{Score}_{\text{freq}} + \alpha \cdot \text{Score}_{\text{ctx}}
    \]
    其中 \(\text{Score}_{\text{freq}}\) 来自历史频率（图边权重），\(\text{Score}_{\text{ctx}}\) 基于当前任务直觉与工具描述的语义相似度（使用SimCSE计算）。若 CIPS 超过阈值 \(\theta_{\text{inertial}}\)，则接受此工具。
  - **参数填充**：采用层次化非LLM策略：首先回溯参数依赖边寻找来源；其次尝试与环境状态匹配；最后使用启发式填充。只有所有必需参数都能成功填充时才执行惯性调用。
  - **回退机制**：若惯性调用失败或参数不完整，则回退到标准LLM推理。额外设计了故障恢复路径（触发连续失败时执行检查操作），并设置了最大惯性调用比例（30%）和禁止连续惯性调用的约束。
- **算法流程**（文字说明）：
  1. 在线构建TIG（初始为空，从执行轨迹中学习）。
  2. 每一步，智能体首先进行Inertia Sensing：根据最近k个工具序列，在TIG中查找历史后继节点，计算CIPS，若超过阈值则进入Parameter Filling；否则直接调用LLM。
  3. Parameter Filling：按照回溯→环境匹配→启发式的顺序填充参数；若全部成功则执行工具并更新图（更新边权重，记录反馈）；若失败则回退LLM。
  4. LLM推理后，根据执行结果更新图（创建新边、调整权重、记录参数依赖）。

## 3. 实验设计：数据集、Benchmark、对比方法

- **数据集与场景**：
  - **AlfWorld**：基于文本的模拟家务任务（多步操作）。
  - **ScienceWorld**：科学实验流程的复杂程序性任务。
  - **ToolQuery-Academic**：面向学术数据库的多步API调用基准（来自AgentBoard）。
- **Benchmark**：使用AgentBoard的**进度率（Progress Rate, PR）** 作为任务正确性度量，同时统计平均LLM调用次数（LLMC）和平均令牌消耗（tok-in, tok-out）作为效率指标。
- **对比方法**：
  - **基线**：ReAct（基础框架）和Reflexion（引入自我反思的增强框架）。
  - **增强版本**：将AutoTool集成到上述基线中（ReAct+AutoTool, Reflexion+AutoTool），与原版对比。
  - 论文在相关工作表中（Table 1）定性比较了DFS-DT、AnyTool、ToolChain、ToolNet、ToolPlanner、LLMCompiler等方法，但未在实验中直接复现对比，主要聚焦于对ReAct和Reflexion的增强效果。

## 4. 资源与算力

- **硬件**：服务器配备 4 个 Intel Xeon Gold 5117 CPU 和 4 个 NVIDIA Tesla V100-SXM2-32GB GPU。
- **默认模型**：Llama4-Scout-17b（采样温度=0）。
- **训练/构建时间**：论文明确为 **无训练（training-free）方法**，TIG在线构建，无需预训练；初始化SimCSE模型耗时约4秒。单次任务的图构建和搜索开销极低（秒级），主要耗时来自语义模块的上下文相关性计算（占总任务执行时间的约2.7%）。
- **训练时长**：未明确说明，因为不需要离线训练。

## 5. 实验数量与充分性

- **主要实验**：在3个数据集上对比了ReAct和Reflexion及其+AutoTool版本（共6组配置），报告了PR、tok-in、tok-out、LLMC四项指标（Table 3）。
- **开销分析**：详细记录了语义模块耗时（Table 4）和非语义模块耗时、动作次数（Table 5）。
- **敏感性分析**：针对ScienceWorld数据集，变化惯性触发阈值 \(\theta_{\text{inertial}}\) (0.1-0.2) 和语义权重 \(\alpha\) (0.3-0.7) 进行了9组组合实验（Table 6）。
- **额外实验（附录提及）**：在ScienceWorld上使用不同LLM（Llama-3.3-70B, Qwen2.5-72B, DeepSeekV3）验证模型无关性；进行了惯性分析、案例研究、动态分析和参数填充准确率分析（因页面限制移至附录）。
- **充分性评估**：实验覆盖了多样化任务领域（文本交互、科学推理、API调用），且考虑了不同基线（是否含反思）。敏感性分析验证了超参数鲁棒性。整体实验设计较为全面，但缺少与同类效率改进方法（如LLMCompiler）的直接数值对比；另外仅测试了单一模型（Llama4-Scout-17b）的主实验，但附录弥补了模型多样性。实验客观公平，严格控制提示和组件一致。

## 6. 论文的主要结论与发现

- **效率提升显著**：AutoTool在三个数据集上平均将LLM调用次数减少15%-25%，令牌消耗降低10%-40%（最大达到2.87倍提升）。
- **任务正确率保持甚至提升**：在AlfWorld上，ReAct+AutoTool的进度率从0.394提升到0.531（归功于故障恢复机制）；在其他数据集上基本持平或略有波动。
- **工具使用惯性确实存在且可利用**：通过熵分析和似然比检验，验证了工具序列具有低熵、高可预测性。
- **参数流动具有集中性**：大多数参数可从少数前驱工具的输出直接获取（如目标参数的来源44.8%集中在一个前驱动作）。
- **AutoTool开销极低**：语义计算仅占任务总时间约2.7%，工程开销可忽略。

## 7. 优点：方法或实验设计上的亮点

- **创新性**：首次提出“工具使用惯性”概念，并设计对应的图结构进行建模，将统计学习与LLM智能体系统巧妙结合，绕过大量不必要的LLM调用。
- **轻量且无训练**：无需微调或预训练，仅依赖在线构建的图，适用于任何LLM后端和工具环境。
- **参数级自动化**：不仅预测工具，还自动填充参数，降低了手动规则或LLM推理的负担。
- **鲁棒性设计**：包含故障恢复机制、惯性调用比例上限、权重基于反馈动态更新，避免错误传播。
- **实验设计细致**：除了主要性能对比，详细报告了时间开销分解和敏感性分析，验证了方法的实际可行性。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **对惯性模式的依赖性**：方法有效性建立在工具使用存在明显顺序模式的前提下；对于完全随机、非结构化的任务或新领域，图中缺乏历史信息，惯性调用成功率低，可能退化为LLM基线而无法节省成本。
- **冷启动问题**：初始无历史轨迹时图为空，需要一定数量的LLM执行来积累数据，短期收益有限（论文采用冷启动设置，图在线构建）。
- **超参数敏感性**：尽管敏感性分析显示一定鲁棒性，但 \(\theta_{\text{inertial}}\) 和 \(\alpha\) 仍需针对任务调整（论文通过目标10-30%调用减少设定）。
- **缺乏与先进效率框架的定量对比**：仅与ReAct和Reflexion对比，未在相同设置下与LLMCompiler、ToolNet等同样注重效率的方法进行数值比较，限制了说服力。
- **环境通用性**：仅测试三个特定领域，对于更复杂的工具集（如数百个API）或因特网规模的工具调用场景，图规模和搜索效率可能受限。
- **缺乏对长轨迹退化分析**：当任务步骤较多时，历史序列矩阵可能增长，但论文未分析图大小对性能的影响。
- **附录中局限性讨论未包含在提取文本中**：论文提到“Appendix G”包含当前框架局限性的详细讨论，但提供内容中缺失，因此无法全面了解作者自认的局限。

（完）
