---
title: How Dark Patterns Manipulate Web Agents
title_zh: 黑暗模式如何操纵Web Agent
authors: "Phil Cuvin, Hao Zhu, Diyi Yang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=G7Dan0L7ho"
tags: ["query:agent-traj"]
score: 8.0
evidence: 测试黑暗模式对web agent轨迹影响的基准与环境
tldr: "该论文揭示了黑暗模式能有效操纵web agent轨迹，对agent鲁棒性构成风险。为此构建DECEPTICON环境，包含700个带黑暗模式的网页导航任务，用于量化这种风险。实验发现超过70%的测试中，黑暗模式成功将agent导向恶意结果。该基准为测试agent面对对抗性UI时的行为提供了重要工具。"
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 黑暗模式对web agent轨迹存在严重操纵风险。
method: 构建包含700个任务的DECEPTICON环境测试黑暗模式影响。
result: "超过70%的成功率将agent导向恶意结果。"
conclusion: 为评估agent对抗性UI的鲁棒性提供了基准。
---

## Abstract
Deceptive UI designs, widely instantiated across the web and commonly known
as dark patterns, manipulate users into performing actions misaligned with their
goals. In this paper, we show that dark patterns are highly effective in steer-
ing agent trajectories, posing a significant risk to agent robustness. To quan-
tify this risk, we introduce DECEPTICON, an environment for testing individual
dark patterns in isolation. DECEPTICON includes 700 web navigation tasks with
dark patterns—600 generated tasks and 100 real-world tasks, designed to measure
instruction-following success and dark pattern effectiveness. Across state-of-the-
art agents, we find dark patterns successfully steer agent trajectories towards mali-
cious outcomes in over 70% of tested generated and real-world tasks—compared
to a human average of 31%. Moreover, we find that dark pattern effectiveness
correlates positively with model size and test-time reasoning, making larger, more
capable models more susceptible. Leading countermeasures against adversarial
attacks, including in-context prompting and guardrail models, fail to consistently
reduce the success rate of dark pattern interventions. Our findings reveal dark pat-
terns as a latent and unmitigated risk to web agents, highlighting the urgent need
for robust defenses against manipulative designs.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：网页中广泛存在的欺骗性 UI 设计（即“黑暗模式”）能够操纵用户行为，但尚未有人系统研究其对 **Web Agent** 行为轨迹的影响。
- **研究动机**：随着自动驾驶 Agent 逐步部署在真实网页任务中，黑暗模式可能成为潜在的安全威胁，导致 Agent 偏离用户意图，甚至导向恶意结果。现有对抗攻击研究主要针对图像、文本等输入，缺少对界面层面欺骗性设计的评估。
- **整体含义**：本文首次揭示黑暗模式是一种未被充分重视且尚未缓解的 Web Agent 风险，呼吁社区开发针对操纵性 UI 的鲁棒防御。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：构建一个标准化、可控的测试环境 **DECEPTICON**，将单一黑暗模式独立嵌入网页导航任务中，量化其影响。
- **关键技术细节**：
  - 环境包含 **700 个带黑暗模式的网页导航任务**（600 个生成 + 100 个真实世界任务）。
  - 任务设计：每个任务要求 Agent 遵循指令完成操作（如填写表单、点击按钮），同时黑暗模式（如误导性标签、隐藏选项、确认羞辱等）被植入以偏离目标。
  - 评估指标：**指令遵循成功率**（Agent 是否完成原始指令）与**黑暗模式有效性**（Agent 被导向恶意结果的比例）。
- 算法/流程文字说明：
  1. 定义一组常见黑暗模式（如强制行动、社会证明、价格比较等）。
  2. 对每个模式，构造多个网页导航场景，随机植入该模式。
  3. 规定原始正确目标路径与黑暗模式诱导的恶意目标路径。
  4. 运行不同 Agent 完成指令，记录实际轨迹与最终结果。
  5. 计算成功率、被诱导率等指标。

## 3. 实验设计
- **数据集/场景**：
  - **生成任务 (600个)**：基于模板自动生成，覆盖多种黑暗模式变体。
  - **真实世界任务 (100个)**：从现实网站中提取或模拟的带黑暗模式页面。
- **Benchmark**：DECEPTICON 本身作为基准环境。
- **对比方法**：
  - **状态最优 Web Agent**（具体模型未在摘要中列出，推测为基于 LLM 的 Agent，如 GPT-4、Claude 等驱动的 agent）。
  - **人类表现**：人类参与者在相同任务上的平均成功率（31%）作为基线。
  - **对抗防御措施**：测试了 in-context prompting（上下文提示）、guardrail models（护栏模型）等常见对抗攻击缓解手段是否有效。

## 4. 资源与算力
- 本文摘要及元数据**未明确说明**所使用的 GPU 型号、数量及训练时长。
- 只提到“state-of-the-art agents”，可能使用了外部 API 或本地部署的 LLM，但算力信息缺失。

## 5. 实验数量与充分性
- **实验数量**：
  - 主实验：700 个任务（600 生成 + 100 真实），覆盖不同黑暗模式。
  - 对比实验：包含人类表现（31%）与多种 Agent。
  - 消融/防御测试：测试了 in-context prompting 和 guardrail models 的效果。
- **充分性评估**：
  - **优势**：任务数量较大（700），涵盖生成和真实场景，并对比人类，能够初步说明问题普遍性。
  - **不足**：
    - 未提供不同黑暗模式类型的细分结果。
    - 缺少对 Agent 是否具备适应性学习的测试。
    - 未说明是否对每个任务重复多次以评估随机性影响。
    - 人类样本量和具体实验控制细节未给出。
  - **总体**：实验设计是合理的，但细节披露不足，客观公平性需要看完整论文。

## 6. 论文的主要结论与发现
1. **黑暗模式高度有效**：在超过 **70%** 的测试任务中，黑暗模式成功将 Agent 轨迹导向恶意结果，远高于人类平均的 31%。
2. **模型能力越强越易受影响**：黑暗模式的有效性与模型大小和测试时推理计算正相关，即更大、更聪明的模型反而更容易被欺骗。
3. **现有防御手段失效**：in-context prompting 和 guardrail models 等对抗攻击缓解方法未能持续降低黑暗模式的成功率。
4. **风险场景**：即使 Agent 本身鲁棒，欺骗性 UI 仍可成为潜在攻击面，对自动化任务构成严重威胁。

## 7. 优点：方法或实验设计上的亮点
- **首创性**：首次系统建立专门评估黑暗模式对 Web Agent 影响的基准环境，填补领域空白。
- **可控性**：将单一黑暗模式隔离测试，避免干扰因素，便于量化。
- **真实与生成结合**：同时使用合成任务和真实世界任务，兼顾覆盖率与生态效度。
- **反直觉发现**：揭示模型规模与脆弱性的正相关，对“越大越强”的常识提出挑战。
- **防御评估**：不仅识别问题，还检查了现有防御的有效性，为后续工作指明方向。

## 8. 不足与局限
- **实验覆盖有限**：
  - 仅测试了有限数量的黑暗模式类型（未列出具体清单），可能遗漏其他模式。
  - 真实世界任务仅 100 个，可能不足以覆盖现实多样性。
- **偏差风险**：
  - 生成任务基于模板，可能存在分布偏差，不能完全代表动态真实的黑暗模式。
  - 人类表现数据可能受参与者人数和任务难度影响，缺乏统计显著性检验报告。
- **应用限制**：
  - 环境为静态页面，未考虑动态交互或用户账户历史等复杂因素。
  - 未探讨 Agent 通过强化学习或在线适应是否会降低黑暗模式影响。
  - 防御措施测试仅限 prompt-level，未涉及微调或安全对齐等更强方法。
- **可重复性**：未公开硬件配置、超参数、种子等，难以完全复现。

（完）
