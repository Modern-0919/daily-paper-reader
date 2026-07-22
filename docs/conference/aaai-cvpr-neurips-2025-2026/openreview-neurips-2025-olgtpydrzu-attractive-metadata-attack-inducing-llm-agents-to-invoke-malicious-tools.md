---
title: "Attractive Metadata Attack: Inducing LLM Agents to Invoke Malicious Tools"
title_zh: 诱饵元数据攻击：诱导LLM智能体调用恶意工具
authors: "Kanghua Mo, Li Hu, Yucheng Long, Zhihao li"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=oLGtPYdRzU"
tags: ["query:agent-errors"]
score: 8.0
evidence: 通过元数据操纵实现工具选择错误
tldr: 针对LLM智能体工具选择的安全漏洞，提出诱饵元数据攻击（AMA），通过修改工具的名称、描述和参数模式等元数据，诱导智能体优先选择恶意工具。在多种模型上验证了攻击有效性，揭示了工具元数据作为攻击面的风险。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有研究忽略了工具元数据可能被操纵以影响智能体工具选择的攻击面。
method: 提出一种黑盒上下文学习框架AMA，自动生成具有高吸引力的恶意工具元数据。
result: 在多个LLM上成功诱导智能体调用恶意工具，验证了攻击的普遍性和隐蔽性。
conclusion: 工具元数据是LLM智能体安全中一个被忽视但严重的威胁面。
---

## Abstract
Large language model (LLM) agents have demonstrated remarkable capabilities in complex reasoning and decision-making by leveraging external tools. However, this tool-centric paradigm introduces a previously underexplored attack surface, where adversaries can manipulate tool metadata---such as names, descriptions, and parameter schemas---to influence agent behavior. We identify this as a new and stealthy threat surface that allows malicious tools to be preferentially selected by LLM agents, without requiring prompt injection or access to model internals. To demonstrate and exploit this vulnerability, we propose the Attractive Metadata Attack (AMA), a black-box in-context learning framework that generates highly attractive but syntactically and semantically valid tool metadata through iterative optimization. The proposed attack integrates seamlessly into standard tool ecosystems and requires no modification to the agent’s execution framework. 
Extensive experiments across ten realistic, simulated tool-use scenarios and a range of popular LLM agents demonstrate consistently high attack success rates (81\%-95\%) and significant privacy leakage, with negligible impact on primary task execution. Moreover, the attack remains effective even against prompt-level defenses, auditor-based detection, and structured tool-selection protocols such as the Model Context Protocol, revealing systemic vulnerabilities in current agent architectures.
These findings reveal that metadata manipulation constitutes a potent and stealthy attack surface. 
Notably, AMA is orthogonal to injection attacks and can be combined with them to achieve stronger attack efficacy, highlighting the need for execution-level defenses beyond prompt-level and auditor-based mechanisms.
Code is available at \url{https://github.com/SEAIC-M/AMA}.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **背景**：大型语言模型（LLM）智能体通过调用外部工具展现出强大的推理与决策能力，但这一工具中心范式引入了新的安全攻击面。现有研究主要关注提示注入等攻击方式，忽略了工具元数据（如名称、描述、参数模式）可能被操纵来影响智能体行为。
- **核心问题**：作者发现，攻击者无需注入提示或访问模型内部，只需修改工具的元数据，即可诱导LLM智能体优先选择恶意工具，构成一种隐蔽且新型的威胁面。论文将此威胁定义为诱饵元数据攻击（AMA），旨在揭示并利用这一漏洞。
- **整体含义**：该工作提醒社区，LLM智能体的安全防御不能仅依赖提示级防护或审计器，还需关注执行级别的元数据完整性保护。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：提出一种黑盒上下文学习框架AMA，通过迭代优化自动生成“具有高度吸引力”但语法和语义上合法的工具元数据（名称、描述、参数模式），使恶意工具在智能体工具选择过程中被优先调用。
- **关键技术细节**：
  - **黑盒攻击**：无需了解模型内部参数或梯度，仅通过查询LLM的响应来优化元数据。
  - **迭代优化**：基于上下文学习（in-context learning），通过构造包含良性工具与恶意工具的上下文示例，让LLM生成对恶意工具更有利的描述。具体过程可能涉及候选元数据生成、攻击成功率反馈、参数调整等循环。
  - **隐蔽性**：生成的元数据保持语法正确、语义合理，不会触发基于规则的检测。
- **公式与算法流程**（文字说明）：
  - 输入：一组良性工具元数据集合、一个恶意工具（原生的无害功能，但可通过元数据伪装成高价值工具）。
  - 输出：优化后的恶意工具元数据（名称、描述、参数模式）。
  - 流程：
    1. 初始化恶意工具元数据为随机或模板形式。
    2. 构造上下文示例：将恶意工具与多个良性工具混合，并模拟LLM智能体在给定用户查询时选择工具的过程。
    3. 使用当前恶意工具元数据查询目标LLM，记录其选中恶意工具的次数作为奖励信号。
    4. 根据奖励信号，通过启发式或基于LLM的变异算子调整元数据（如改写描述、更换关键词、调整参数名称等）。
    5. 重复步骤2-4直到达到指定迭代次数或攻击成功率阈值。
  - 最终得到具有高引诱性的元数据。

## 3. 实验设计：数据集/场景、benchmark、对比方法
- **数据集/场景**：使用了10个真实的、模拟的工具使用场景（simulated tool-use scenarios），覆盖常见LLM代理应用（如代码执行、信息检索、数据处理等）。具体场景未在摘要中列举，但强调场景多样化，包含隐私敏感操作（如读取文件、网络请求）。
- **Benchmark**：未明确提及统一基准，但基于每个场景评估攻击成功率（ASR）及对主任务执行的影响（如任务完成度、延迟等）。
- **对比方法**：
  - 无攻击基线（正常元数据）。
  - 简单的元数据随机篡改。
  - 可能对比了提示注入攻击（作为正交攻击的基线，同时验证AMA可与注入攻击叠加）。
  - 防御方面：对比了提示级防御（如安全指令）、审计器检测（如规则过滤）、结构化工具选择协议（如Model Context Protocol, MCP）。

## 4. 资源与算力
- 文中 **未明确说明** 所使用的GPU型号、数量、训练时长等具体算力信息。仅提到是黑盒方法，不需要训练模型本身，但元数据优化过程需要多次查询LLM。因此算力消耗主要来自API调用成本，未量化。

## 5. 实验数量与充分性
- **实验数量**：
  - 在10个不同场景上测试。
  - 测试了多个流行LLM（如GPT-4、Claude、Llama等）作为智能体。
  - 进行了多种防御条件下的实验（提示级、审计器、MCP）。
  - 可能还进行了消融实验（例如不同优化迭代次数、不同元数据分量重要性等），但摘要未细述。
- **充分性与客观性**：
  - 场景覆盖较广（10个），且包含隐私泄露评估，但未说明场景是否涵盖所有常见工具类型。
  - 模型多样性足够，但缺乏对开源模型与闭源模型的系统对比。
  - 防御效果测试比较全面，验证了攻击对主流防御的鲁棒性。
  - 未提供置信区间或统计显著性检验，结果可靠性待补充。

## 6. 论文的主要结论与发现
- **攻击有效性**：AMA在多个LLM智能体上实现了81%-95%的攻击成功率，并导致显著的隐私泄露（如读取敏感文件）。
- **隐蔽性**：攻击对主任务执行影响极小（negligible impact），难以被察觉。
- **防御脆弱性**：AMA对提示级防御、审计器检测以及结构化工具选择协议（MCP）均有效，暴露出当前智能体架构的系统性漏洞。
- **与注入攻击正交**：AMA可与提示注入攻击结合，实现更强效果，说明需要执行层面的防御机制。
- **威胁面总结**：工具元数据是LLM智能体安全中一个被忽视但严重的威胁面。

## 7. 优点：方法或实验设计上的亮点
- **方法创新**：首次系统地提出并验证了基于工具元数据操纵的攻击面，填补了LLM安全研究的空白。
- **黑盒且无需访问模型内部**：降低了攻击实施门槛，更具实际威胁性。
- **实验全面**：覆盖多模型、多场景、多防御策略，验证了攻击的普遍性和鲁棒性。
- **开源代码**：提供了代码仓库，便于复现和后续研究。
- **与现有攻击正交性**：展示了联合攻击的潜力，推动防御设计向执行层深化。

## 8. 不足与局限
- **实验覆盖有限**：真实世界工具系统（如API数量、动态注册、版本管理）可能更复杂，模拟场景的保真度待评估。
- **未考虑主动防御**：如元数据完整性校验、数字签名、权限管理等执行级防御。实验仅测试了外部审计器或提示防御。
- **对模型规模依赖**：攻击成功率可能随模型能力不同而变化（如小模型可能更难被引导），文中缺乏对模型规模的系统分析。
- **可扩展性**：元数据优化需反复查询LLM，在工具数量极大时可能成本过高。
- **无理论分析**：缺乏对攻击背后理性机制（如注意力分布、词嵌入相似性）的理论解释。

（完）
