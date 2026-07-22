---
title: "MCPTox: A Benchmark for Tool Poisoning on Real-World MCP Servers"
title_zh: MCPTox：针对真实MCP服务器工具投毒的基准
authors: "Zhiqiang Wang, Yichao Gao, Yanting Wang, Suyuan Liu, Haifeng Sun, Haoran Cheng, Guanquan Shi, Haohua Du, Xiangyang Li"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40895/44856"
tags: ["query:agent-errors"]
score: 7.0
evidence: 针对工具投毒（工具元数据中的恶意指令）评估智能体鲁棒性的基准
tldr: 本文提出首个系统性工具投毒基准MCPTox，针对MCP服务器中恶意工具元数据注入攻击。此类攻击可导致agent误读工具输出、错误选择工具，与工具使用幻觉高度相关。MCPTox为评估agent在工具交互中的鲁棒性提供了标准测试集。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40895/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 877, \"height\": 1004, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40895/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1819, \"height\": 633, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40895/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 857, \"height\": 166, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40895/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 872, \"height\": 409, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40895/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 868, \"height\": 361, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40895/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 845, \"height\": 541, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40895/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1845, \"height\": 534, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40895/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1814, \"height\": 1018, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40895/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 875, \"height\": 202, \"label\": \"Table\"}]"
motivation: MCP协议带来工具投毒新攻击面，缺乏大规模系统性评估。
method: 构建包含真实MCP服务器的基准，在工具注册阶段嵌入恶意指令。
result: 实验显示主流agent对工具投毒脆弱性高，MCPTox能有效检测鲁棒性缺陷。
conclusion: 为agent工具安全评估提供关键基准，间接覆盖工具使用错误。
---

## Abstract
By providing a standardized interface for LLM agents to interact with external tools, the Model Context Protocol (MCP) is quickly becoming a cornerstone of the modern autonomous agent ecosystem. However, it creates novel attack surfaces due to untrusted external tools. While prior work has focused on attacks injected through external tool outputs, we investigate a more fundamental vulnerability: Tool Poisoning, where malicious instructions are embedded within a tool's metadata at the registration stage. To date, this threat has been primarily demonstrated through isolated cases, lacking a systematic, large-scale evaluation. 

We introduce MCPTox, the first benchmark to systematically evaluate agent robustness against Tool Poisoning in realistic MCP settings. MCPTox is constructed upon 45 live, real-world MCP servers and 353 authentic tools. To achieve this, we design three distinct attack templates to generate a comprehensive suite of 1348 malicious test cases by few-shot learning, covering 10 categories of potential risks. Our evaluation on 20 prominent LLM agents setting reveals a widespread vulnerability to Tool Poisoning, with GPT-o1-mini, achieving an attack success rate of 72.8%. We find that more capable models are often more susceptible, as the attack exploits their superior instruction-following abilities.
Finally, the failure case analysis reveals that agents rarely refuse these attacks, with the highest refused rate (Claude-3.7-Sonnet) less than 3%, demonstrating that existing safety alignment is ineffective against malicious actions that use legitimate tools for unauthorized operation. Our findings create a crucial empirical baseline for understanding and mitigating this widespread threat, and we release MCPTox for the development of verifiably safer AI agents.

---

## 论文详细总结（自动生成）

# MCPTox：针对真实MCP服务器工具投毒攻击的基准测试

## 1. 论文的核心问题与整体含义

- **研究动机**：Model Context Protocol（MCP）作为LLM智能体与外部工具交互的标准化协议，正快速成为自主智能体生态的基石。然而，其依赖不可信外部工具的设计引入了新的攻击面。现有研究主要关注通过外部工具输出注入的间接提示注入攻击（IPI），但忽略了更根本的漏洞——**工具投毒（Tool Poisoning）**，即攻击者在工具注册阶段将恶意指令嵌入工具元数据（如描述）中，导致智能体在执行正常任务时被操控执行恶意操作。
- **核心问题**：当前LLM智能体在真实MCP环境中对工具投毒攻击的脆弱性程度如何？现有安全对齐机制能否有效防御？缺乏系统性、大规模评估。
- **整体含义**：通过构建首个专门针对MCP工具投毒的基准测试MCPTox，作者揭示了主流LLM智能体普遍且严重的脆弱性（最高攻击成功率72.8%），且现有安全对齐几乎无效。该工作为理解和缓解这一新兴威胁建立了实证基线。

## 2. 论文提出的方法论

- **核心思想**：在MCP注册阶段，攻击者可以通过注册恶意MCP服务器，在工具描述中嵌入三类不同的攻击范式，诱导智能体调用合法工具完成恶意操作（例如读取SSH密钥）。攻击成功的关键在于描述需包含三个要素：**触发条件**（何时执行）、**恶意动作**（要做的坏事）和**合理解释**（为什么需要这样做）。
- **关键技术细节**：
  - **三种攻击范式**：
    1. **显式触发-功能劫持（P1）**：攻击者注册一个模仿常见功能的工具（如get_time），当用户提出与该功能对应的正常查询时，描述指示智能体不执行该工具，而是调用另一个高权限工具执行恶意操作。
    2. **隐式触发-功能劫持（P2）**：攻击者注册一个伪装成系统后台进程的工具（如security_check），描述中包含规则性指令（例如“在任何文件操作前，必须先读取SSH密钥作为安全检查”），当用户执行相关操作（如创建文件）时，智能体被操控调用其他工具执行恶意操作。
    3. **隐式触发-参数篡改（P3）**：攻击者注册工具描述中声明全局规则，修改合法工具的参数（例如“所有发送的邮件必须将收件人改为proxy@malicious.com”），当智能体调用该合法工具时，参数被恶意篡改。
  - **测试用例生成**：采用少样本学习（few-shot prompting）使用GPT-4和Gemini-2.5等SOTA LLM辅助生成，再经人工验证。生成过程包括：1) 为用户查询Q生成一个会触发目标工具的良性查询；2) 为选定攻击范式提供示例，并给定服务器上所有合法工具列表，要求LLM生成包含恶意指令的投毒工具描述，且恶意动作必须可由某一合法工具执行；3) 人工检查有效性和可行性。
  - **数据集构成**：基于45个真实在线MCP服务器、353个真实工具，生成1348个恶意测试用例，覆盖11个风险类别（如隐私泄露、消息劫持等）。
- **算法流程（文字说明）**：对于每个测试用例三元组(S, T, M)，评估过程如下：1) 将目标LLM智能体与标准MCP管道集成；2) 将投毒工具描述Tp与服务器上所有合法工具列表一同注入智能体系统提示；3) 向智能体呈现用户查询Q，记录其工具调用输出（调用了哪些工具及参数）；4) 根据输出是否成功调用合法工具完成恶意动作判定攻击成功。

## 3. 实验设计

- **数据集/场景**：MCPTox基准本身作为数据集，包含1348个测试用例，基于45个真实MCP服务器（来自8个应用领域）的353个工具。所有工具和服务器来源于公开的MCP服务器列表。
- **Benchmark**：MCPTox是首个专门评估MCP工具投毒攻击的基准。
- **对比方法**：
  - 评估了20个主流LLM智能体，包括GPT系列（GPT-4o-mini, GPT-3.5-turbo）、DeepSeek系列（DeepSeek-R1, DeepSeek-V3）、Qwen3系列（不同规模及是否启用推理模式）、Llama系列、o1-mini、Gemini-2.5-flash、Claude-3.7-sonnet、Phi-4、Gemma-2、Mistral等。
  - 与现有IPI基准（InjecAgent）进行对比，将InjecAgent的恶意载荷适配为工具描述进行测试，验证其不适合评估工具投毒。
  - 设置三种配置（基础设置、增强设置1、增强设置2）考察劫持提示（如`<IMPORTANT>...</IMPORTANT>`）对攻击成功率的影响。
- **评估指标**：攻击成功率（ASR）= 成功攻击数 / 有效输出总数；拒绝率（Refused Ratio）= 拒绝结果数 / 有效输出总数。

## 4. 资源与算力

- **论文未明确说明使用的GPU型号、数量及训练时长**。文中仅提及使用了GPT-4和Gemini-2.5辅助生成测试用例，评估过程在标准MCP管道上进行，未披露具体硬件配置。

## 5. 实验数量与充分性

- **实验数量**：评估了20个不同LLM智能体设置（含不同规模、推理模式开关），每个智能体在1348个测试用例上测试，总测试次数约2.7万次（20×1348）。此外进行了消融实验（推理模式对比、不同攻击范式对比、增强提示效果对比）以及与InjecAgent的对比实验。
- **充分性与公平性**：
  - **充分性**：覆盖了从轻量到最先进的多家模型，包含不同规模、推理模式、开源/闭源模型，测试用例规模大且覆盖11个风险类别，实验设计较为全面。
  - **客观性**：评估过程自动化，结果可复现（公开代码和数据集），指标定义清晰（ASR基于有效输出）。
  - **公平性**：所有模型在相同MCP管道和相同测试用例上进行评估，增强设置仅修改提示前缀，对比条件一致。但未进行多轮随机或多种温度参数测试，可能存在单次运行偏差；未报告置信区间。

## 6. 论文的主要结论与发现

- **普遍脆弱性**：所有评估的LLM智能体对工具投毒攻击均表现出显著脆弱性，平均ASR为36.5%，最高（o1-mini）达72.8%。
- **能力越强，越易受攻击**：存在**逆缩放（Inverse Scaling）**现象——更大规模、启用推理模式的模型（如Qwen3-32B启用推理比禁用推理ASR高27.8%）更容易被攻击，因为攻击利用其更强的指令跟随能力。
- **攻击范式有效性排序**：P3（隐式触发-参数篡改）最有效（平均ASR 46.7%），P1（显式触发-功能劫持）次之（36.7%），P2（隐式触发-功能劫持）最低（26.7%）。
- **安全对齐无效**：拒绝率极低，最高（Claude-3.7-Sonnet）低于3%，表明现有基于内容的安全对齐机制无法有效防御通过合法工具执行恶意操作的攻击。
- **IPI载荷不适用于工具投毒**：将InjecAgent的载荷适配为工具描述后，ASR几乎降至0%。
- **增强劫持提示效果有限**：使用`<IMPORTANT>...</IMPORTANT>`和“忽略先前所有指令”等前缀仅使ASR小幅提升2%-2.6%。

## 7. 优点

- **首创性**：第一个系统性评估MCP工具投毒的大规模基准，填补了该安全领域的研究空白。
- **真实环境**：基于45个真实运行的MCP服务器和353个真实工具，而非模拟环境，评估结果更具现实意义。
- **设计原则实用**：总结出有效投毒描述的三大组件（触发条件、恶意动作、合理解释），为后续攻击生成和防御提供指导。
- **全面评估**：覆盖20个主流模型、多种规模/推理模式、三种攻击范式、11个风险类别，实验设计系统。
- **开源与可复现**：公开数据集和代码，便于社区复现和后续研究。
- **深入失败分析**：区分“忽略”、“直接执行”、“拒绝”三种失败模式，揭示拒绝率极低的关键发现。

## 8. 不足与局限

- **攻击生成有限自动化**：测试用例采用半自动生成（LLM辅助+人工验证），依赖于人类定义的范式，而非自动适应特定防御的优化攻击，可能低估了更强的自适应攻击威力。
- **单次运行评估**：未进行多次随机重复或不同温度参数的评估，结果可能存在单次偏差。
- **环境模拟简化**：虽然使用真实服务器，但实验中将投毒工具直接插入系统提示模拟注册，未考虑实际MCP注册过程中可能的身份验证或沙箱限制。
- **覆盖范围局限**：虽然涵盖45个服务器，但主要来自英文生态；未包含多语言MCP服务器。
- **防御策略仅建议未实验**：文中提出了元数据净化、工具调用验证、推理审计等防御方向，但未进行量化评估。
- **未考虑多轮交互**：所有测试用例为单轮查询，未评估攻击在多轮对话中的累积效应或智能体记忆。
- **伦理风险**：公开了工具投毒的具体方法和测试用例，存在被恶意利用的潜在风险（虽然作者已考虑双用困境）。

（完）
