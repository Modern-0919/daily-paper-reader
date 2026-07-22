---
title: "AgentAuditor: Human-level Safety and Security Evaluation for LLM Agents"
title_zh: AgentAuditor：LLM智能体的人类级安全与安全评估
authors: "Hanjun Luo, Shenyu Dai, Chiming Ni, Xinfeng Li, Guibin Zhang, Kun Wang, Tongliang Liu, Hanan Salam"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=2KKqp7MWJM"
tags: ["query:agent-errors"]
score: 9.0
evidence: LLM智能体的逐步安全与安全评估
tldr: 现有LLM智能体安全评估器常遗漏步骤级危险和细微含义，且难以处理问题累积。本文提出AgentAuditor，无需训练的记忆增强推理框架，通过自适应提取结构化语义特征和思维链推理轨迹，模拟人类专家评估员，全面覆盖逐步动作、细微含义和复合问题，显著提升评估可靠性。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有评估器无法可靠检测智能体逐步动作中的危险，忽略细微含义和复合问题。
method: 提出无需训练的记忆增强推理框架，自适应提取结构化语义特征并生成思维链推理。
result: 模拟人类专家评估员，在检测逐步危险、细微含义和复合问题上取得显著改进。
conclusion: AgentAuditor为LLM智能体安全评估提供了可靠且无需训练的通用框架。
---

## Abstract
Despite the rapid advancement of LLM-based agents, the reliable evaluation of their safety and security remains a significant challenge. Existing rule-based or LLM-based evaluators often miss dangers in agents' step-by-step actions, overlook subtle meanings, fail to see how small issues compound, and get confused by unclear safety or security rules. To overcome this evaluation crisis, we introduce AgentAuditor, a universal, training-free, memory-augmented reasoning framework that empowers LLM evaluators to emulate human expert evaluators. AgentAuditor constructs an experiential memory by having an LLM adaptively extract structured semantic features (e.g., scenario, risk, behavior) and generate associated chain-of-thought reasoning traces for past interactions. A multi-stage, context-aware retrieval-augmented generation process then dynamically retrieves the most relevant reasoning experiences to guide the LLM evaluator's assessment of new cases. Moreover, we developed ASSEBench, the first benchmark designed to check how well LLM-based evaluators can spot both safety risks and security threats. ASSEBench comprises 2293 meticulously annotated interaction records, covering 15 risk types across 29 application scenarios. A key feature of ASSEBench is its nuanced approach to ambiguous risk situations, employing "Strict" and "Lenient" judgment standards. Experiments demonstrate that AgentAuditor not only consistently improves the evaluation performance of LLMs across all benchmarks but also sets a new state-of-the-art in LLM-as-a-judge for agent safety and security, achieving human-level accuracy. Our work is openly accessible at https://github.com/Astarojth/AgentAuditor-ASSEBench.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：基于 LLM 的智能体在安全与安全评估方面存在严重缺陷。现有的规则型或 LLM 型评估器往往无法检测智能体逐步动作中的危险，忽视细微语义含义，难以识别小问题累积成复合风险的情况，并且在面对模糊的安全/安全规则时容易困惑。
- **研究动机**：随着 LLM 智能体的快速发展，迫切需要一个能够像人类专家一样全面、可靠地评估其安全风险的评估框架，以解决当前“评估危机”。
- **整体含义**：本文提出的 AgentAuditor 旨在为 LLM 智能体提供一种无需额外训练、可泛化的人类级安全与安全评估方案，填补了现有评估器在步骤级危险、细微含义和复合问题检测上的空白。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：AgentAuditor 是一个**无需训练的记忆增强推理框架**，通过模拟人类专家评估员的认知过程，提升 LLM 评估器的表现。
- **关键技术细节**：
  - **经验记忆构建**：利用 LLM 自适应地从历史交互中提取结构化语义特征（如场景、风险、行为），并生成相关的思维链（Chain-of-Thought）推理轨迹，形成经验记忆库。
  - **多阶段上下文感知检索增强生成（RAG）**：在评估新案例时，动态地从记忆库中检索最相关的推理经验，并依据多阶段、上下文感知的策略将这些经验引导到 LLM 评估器的判断中，从而覆盖逐步动作、细微含义和复合问题。
- **算法流程简述**：
  1. 对过去交互记录，使用 LLM 自动提取 `(场景, 风险, 行为)` 三元组，并伴随 CoT 推理链。
  2. 存储这些经验到记忆库。
  3. 对于新案例，先进行多阶段检索（如初步筛选、语义匹配、风险类型过滤），获取最相关经验。
  4. 将检索到的经验与当前案例一起输入 LLM，生成最终评估判断。

### 3. 实验设计：数据集 / 场景、Benchmark、对比方法

- **Benchmark**：作者开发了 **ASSEBench**，这是首个专门用于检测 LLM 评估器能否同时识别**安全风险**和**安全威胁**的基准。
- **数据集规模与构成**：包含 **2293 条**精心标注的交互记录，覆盖 **15 种风险类型**，跨越 **29 个应用场景**。该基准的一个关键特点是对模糊风险情况采用“严格”和“宽松”两种判断标准。
- **对比方法**：实验对比了多种现有基于规则或 LLM 的评估器（具体名称未在摘要中列出，但提及“所有基准”），评估 AgentAuditor 带来的性能提升。
- **实验结果**：AgentAuditor 在所有基准上一致提升了 LLM 的评估性能，并在 LLM-as-a-judge 的安全与安全评估中达到**新 SOTA**，实现**人类级精度**。

### 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及所使用的 GPU 型号、数量、训练时长等具体算力信息。由于 AgentAuditor 是**无需训练**的框架，其主要计算开销来自 LLM 推理和检索阶段，但作者未提供相关细节。

### 5. 实验数量与充分性

- **实验数量**：基于 ASSEBench（2293 条记录）进行了多组对比实验，并在多个 LLM 评估器（至少包括一个基础 LLM 作为基底）上测试了 AgentAuditor 的增强效果。摘要提到“所有基准”均表现提升，暗示可能包含不同风险类型、不同应用场景下的子实验，以及严格/宽松标准下的对比。
- **充分性评价**：基准设计考虑了多种风险类型和应用场景，并引入模糊标准的差异化评估，实验设计较为全面。但摘要未给出消融实验细节（如是否对比不同检索策略、记忆大小的影响等），因此充分性有一定保留。整体上实验较充分，结论客观。

### 6. 论文的主要结论与发现

- AgentAuditor 作为一个无需训练的通用框架，能够显著提升 LLM 在智能体安全与安全评估中的可靠性。
- 通过记忆增强和思维链推理，AgentAuditor 成功弥补了现有评估器在逐步危险、细微含义和复合问题检测上的不足。
- 在 ASSEBench 上达到人类级准确率，为 LLM 智能体安全评估设立了新标准。

### 7. 优点

- **无需训练**：直接利用预训练 LLM 的能力，避免了额外数据标注和模型微调成本。
- **模拟人类专家**：通过结构化语义特征抽取和 CoT 推理经验记忆，使评估过程更具解释性和认知合理性。
- **多阶段上下文检索**：能够动态适应不同案例，精准获取相关经验，提升评估准确性。
- **细粒度评估**：支持逐步动作、细微含义和复合问题的检测，超越了传统二分类或粗粒度打分。
- **首个统一基准**：ASSEBench 同时涵盖安全和安全两方面，并引入严格/宽松标准，促进更公平的比较。

### 8. 不足与局限

- **依赖 LLM 内在能力**：AgentAuditor 的效果受限于所选 LLM 的语义理解、推理和上下文窗口大小，对于弱 LLM 可能改进有限。
- **计算开销**：虽然无需训练，但多阶段检索和多次 LLM 调用（记忆构建+评估）可能带来较高的实际推理延迟和成本，文中未进行分析。
- **记忆库质量**：经验记忆依赖于初始历史交互的标注（可能仍需要人工或自动标注），且随着记忆增长，检索效率可能下降。
- **未覆盖所有风险**：ASSEBench 虽含 15 种风险类型，但可能无法涵盖新兴或特定领域的风险（如金融、医疗）。
- **应用限制**：目前仅针对评估环节，未涉及如何利用评估结果进行防御或修复。此外，对于完全新颖、无历史相似的案例，检索可能失效。
- **可复现性**：代码已开源，但未提供详细的超参数设置和计算资源说明，可能影响复现和公平比较。

（完）
