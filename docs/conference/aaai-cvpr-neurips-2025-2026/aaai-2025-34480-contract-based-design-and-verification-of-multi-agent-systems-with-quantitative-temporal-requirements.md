---
title: Contract-based Design and Verification of Multi-Agent Systems with Quantitative Temporal Requirements
title_zh: 基于合约的含定量时间需求的多智能体系统设计与验证
authors: "Rafael Dewes, Rayna Dimitrova"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34480/36635"
tags: ["query:agent-errors"]
score: 6.0
evidence: 使用定量假设保证合约对多智能体系统进行形式化验证
tldr: "针对多智能体系统因局部和共享目标引起的复杂依赖导致设计易错、验证耗时的问题，提出定量假设保证合约概念，通过逻辑LTL[F]形式化定量需求，实现系统级验证，为多智能体系统的可靠性提供形式化保障。"
source: AAAI-2025-Accepted
selection_source: conference_retrieval
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34480/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 849, \"height\": 246, \"label\": \"Table\"}]"
motivation: 多智能体系统设计中局部与共享目标之间存在复杂依赖，易出错且验证耗时。
method: "提出定量假设保证合约，结合定量时序逻辑LTL[F]进行系统设计与验证。"
result: 该合约方法降低了验证复杂性，提高了设计可靠性。
conclusion: 提供了应对多智能体系统定量需求的形式化验证方法。
---

## Abstract
Quantitative requirements play an important role in the context of multi-agent systems, where there is often a trade-off between the tasks of individual agents and the constraints that the agents must jointly adhere to. 
We study multi-agent systems whose requirements are formally specified in the quantitative temporal logic LTL[F] as a combination of local task specifications for the individual agents and a shared safety constraint, 
The intricate dependencies between the individual agents entailed by their local and shared objectives make the design of multi-agent systems error-prone, and their verification time-consuming.
In this paper we address this problem by proposing a novel notion of quantitative assume-guarantee contracts, that enables the compositional design and verification of multi-agent systems with quantitative temporal specifications.
The crux of these contracts lies in their ability to capture the coordination between the individual agents to achieve an optimal value of the overall specification under any possible behavior of the external environment.
We show that the proposed  framework improves the scalability and modularity of formal verification of multi-agent systems against quantitative temporal specifications.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究背景**：多智能体系统（MAS）在机器人、分布式计算、自动驾驶等领域日益重要。系统包含多个自主智能体，它们需要协同完成共享目标，同时满足各自的局部任务。这种局部与全局目标之间的依赖关系使得系统设计容易出错，验证耗时。传统的形式化验证方法在处理定量需求时存在不足，例如假设-保证合约无法表达定量权衡和最优协作。
- **核心问题**：如何为具有定量时间规范（用LTL[F]逻辑表达）的多智能体系统提供一种结构化的合约框架，支持组合式设计与验证，使系统能在任何环境下达到“尽力而为”（best-effort）的整体最优满意度，同时保证可扩展性和模块化。
- **整体含义**：论文提出一种新颖的“定量假设-保证合约”（Good-Enough Decomposition Contracts, GEDC），能够捕获智能体间的协调，并将全局定量规范分解为局部合约，从而降低验证复杂性、提高设计可靠性。

## 2. 方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：
  - 采用定量时序逻辑LTL[F]作为规范语言，该逻辑对每条迹输出一个[0,1]的满意度值，支持加权求和等运算。
  - 定义“组合式规格”，由每个智能体的局部规范 \( \varphi_{local}^a \) 和一个共享安全规范 \( \Psi_{shared} \) 通过单调组合函数 \( comb \) 聚合而成。
  - 提出“尽力而为满足”（Good-Enough Satisfaction）：系统对于每个输入序列，输出必须达到该输入所能支持的最高值（即至少与任何可能输出一样好）。
- **关键技术细节**：
  - **允许分解映射 (ADec)**：对每个可能的全局满意度值 v，指定一组值分解（二元组包含各局部值及共享值），并对输入序列进行分组（H集合），说明哪些分解是允许的。
  - **假设-保证合约 AG**：对每个 v 赋予一组安全语言形式的假设 \( A_v^a \) 和保证 \( G_v^a \)，满足输入/输出独立性和非阻塞性。
  - **GEDC的定义**：要求对于每个 v-有希望的输入序列，存在值分解 d 和输出迹，使得该迹满足所有假设且达到 d 中的值。
  - **合约满足的条件**（Definition 5）：对于每个智能体 a，当其他智能体的行为是“协作的”（即允许 a 达到 d 中的值）时，a 必须提供输出以达到 d中的对应值并满足保证；否则 a 只需尽力满足自身局部规范。
- **算法流程**（验证过程）：
  - 为每个智能体 a 构造一个GNBA（广义非确定性Büchi自动机） \( C_a \)，其语言包含所有违反 Definition 5 条件的迹。
  - 检查 \( C_a \) 与该智能体实现 \( M_a \) 的语言交集是否为空：若为空则合约被满足。
  - 可分解为逐个 v 的检查，支持增量模式。
- **公式与复杂度**：
  - \( C_a \) 的状态数上限为 \( 2^{2^{O(|\varphi_{local}^a|^2 + |\Psi_{shared}|^2)}} \cdot \sum_{(v,H,D) \in ADec} |H| \cdot |A_v^a| \cdot |G_v^a| \)。
  - 验证复杂度为 \( O(|M_a| \times 状态数) \)，对每个智能体线性于实现大小。

## 3. 实验设计
- **实验场景与benchmark**：论文使用了6个多智能体系统实例，包括：
  - “Delivery vehicles”（3智能体，运输车辆）——基础示例。
  - “Tasks scaled”（6智能体，扩展任务场景）等。
  - 每个场景均用LTL[F]组合规范描述局部和共享目标。
- **对比方法**：
  - **组合式验证**（Comp.）：通过检查GEDC满足性进行推断。
  - **直接单片式验证**（Mono.）：构造所有智能体的乘积系统后直接验证全局规范。
  - **局部修改验证**（Rev.）：仅检查单个智能体实现修改后的合约满足性。
- **benchmark数据**：每个实例包含智能体数量（|Agt|）、公式规模（|Φ|）等，具体参见论文表1。

## 4. 资源与算力
- 论文未明确说明使用的GPU型号、数量或训练时长。实验运行在一台“consumer laptop with 16 GB of memory”上，使用Python和Spot自动机库。所有运行时数据在表1中给出（单位：秒），最大超时时间为30分钟。

## 5. 实验数量与充分性
- **实验数量**：表格包含6个不同配置的MAS实例（3~6个智能体），每个实例都对比了组合式验证和单片式验证的运行时，部分实例还报告了局部修改验证的结果。未进行消融实验或其他变体分析。
- **充分性与公平性**：
  - 实验覆盖了不同规模和复杂度的场景，能体现组合式方法在规模增大时的优势。
  - 对比的两种方法（组合 vs 单片）在相同硬件下运行，条件公平。
  - 但实例数量较少（6个），且未提供统计显著性检验或置信区间，可能不足以全面评估框架的普适性。

## 6. 主要结论与发现
- 提出的GEDC合约框架对于定量时间规范的多智能体系统是 **sound**（若系统满足合约则满足全局规范）和 **complete**（若系统满足全局规范则可构造合约使其满足）。
- 组合式验证相比单片式验证具有更好的 **可扩展性**：在复杂场景中单片式超时（TO），而组合式能在秒级完成。
- 局部修改验证（仅检查单个智能体）显著快于完全验证，支持模块化设计迭代。
- 合约的灵活度（通过允许分解映射）能够表达不同的协调策略，例如指定哪些智能体承担坏天气下的任务。

## 7. 优点
- **方法创新性**：首次将“尽力而为”满意度与定量假设-保证合约结合，提出了“良好-足够分解合同”（GEDC），能处理定量权衡和动态环境。
- **理论完备性**：证明了soundness与completeness，确保合约分解不丢失全局最优性。
- **验证过程模块化**：每个智能体独立验证，避免全局状态爆炸，支持增量变更。
- **实验对比清晰**：通过运行时对比展示了组合式方法在扩展性上的优势，并指出了单片式在大型实例中的内存瓶颈。
- **工具链实现**：基于Spot自动机库实现了原型，开源了benchmark代码，增强可重复性。

## 8. 不足与局限
- **实验规模有限**：仅测试了6个实例，最大智能体数为6，未在更大规模（如10+智能体）或更复杂的环境上验证扩展性。
- **资源信息缺失**：未提供GPU型号、训练时间等计算资源细节，仅提及内存16GB的消费级笔记本，难以复现精确性能。
- **未考虑非合作场景**：模型假设智能体是协作的，不适用于包含敌对或自利智能体的MAS（如博弈论场景）。
- **合约表示假设**：假设允许分解映射和假设/保证自动机已以有限形式给出，但生成这些合约的方法未深入讨论，实际应用中可能需要额外设计。
- **缺乏消融实验**：未单独研究不同组件（如允许分解映射的粒度、假设的严格程度）对验证效率与满足性的影响。
- **应用限制**：LTL[F]的自动机构造复杂度为双指数级，对于非常复杂的公式可能导致状态爆炸，虽然论文声称组合方法能缓解，但对极复杂公式仍存在风险。

（完）
