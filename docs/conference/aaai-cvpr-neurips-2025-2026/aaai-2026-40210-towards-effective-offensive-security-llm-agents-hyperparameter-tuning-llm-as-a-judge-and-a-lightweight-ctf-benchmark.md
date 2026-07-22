---
title: "Towards Effective Offensive Security LLM Agents: Hyperparameter Tuning, LLM as a Judge, and a Lightweight CTF Benchmark"
title_zh: 面向高效进攻性安全LLM智能体：超参数调优、LLM评判与轻量级CTF基准
authors: "Minghao Shao, Nanda Rani, Kimberly Milner, Haoran Xi, Meet Udeshi, Saksham Aggarwal, Venkata Sai Charan Putrevu, Sandeep K. Shukla, Prashanth Krishnamurthy, Farshad Khorrami, Ramesh Karri, Muhammad Shafique"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40210/44171"
tags: ["query:agent-errors"]
score: 8.0
evidence: 使用LLM作为裁判分析agent轨迹
tldr: 本文针对CTF安全任务中的LLM agent，提出CTFJudge框架，利用LLM作为裁判分析agent轨迹并给出分步评价；同时提出CTF能力指数（CCI）衡量部分正确性。该方法不仅适用于安全场景，其轨迹分析思路可推广至一般agent错误检测和验证。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40210/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 858, \"height\": 447, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40210/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 634, \"height\": 457, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40210/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 864, \"height\": 416, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40210/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 872, \"height\": 589, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40210/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 874, \"height\": 523, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40210/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 878, \"height\": 522, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40210/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 874, \"height\": 518, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40210/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 714, \"height\": 710, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40210/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 855, \"height\": 440, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40210/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 855, \"height\": 439, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40210/fig-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 853, \"height\": 439, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40210/fig-012.webp\", \"caption\": \"\", \"page\": 0, \"index\": 12, \"width\": 869, \"height\": 908, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40210/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 840, \"height\": 540, \"label\": \"Table\"}]"
motivation: 现有LLM agent在CTF任务中成功因素不明确，缺乏细粒度轨迹评估方法。
method: 提出CTFJudge，利用LLM作为裁判分析agent执行轨迹，并设计CCI指标评估部分正确性。
result: 实验表明CTFJudge能有效识别agent各步骤错误，CCI指标可区分不同能力水平。
conclusion: CTFJudge提供了可迁移的agent轨迹验证方法，对agent错误检测具有借鉴意义。
---

## Abstract
Recent advances in LLM agentic systems have improved the automation of offensive security tasks, particularly for Capture the Flag (CTF) challenges. We systematically investigate the key factors that drive agent success and provide a detailed recipe for building effective LLM-based offensive security agents. First, we present CTFJudge, a framework leveraging LLM as a judge to analyze agent trajectories and provide granular evaluation across CTF solving steps. Second, we propose a novel metric, CTF Competency Index (CCI) for partial correctness, revealing how closely agent solutions align with human-crafted gold standards. Third, we examine how LLM hyperparameters, namely temperature, top-p, and maximum token length, influence agent performance and automated cybersecurity task planning. For rapid evaluation, we present CTFTiny, a curated benchmark of 50 representative CTF challenges across binary exploitation, web, reverse engineering, forensics, and cryptography. Our findings identify optimal multi-agent coordination settings and lay the groundwork for future LLM agent research in cybersecurity.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- LLM 智能体在进攻性安全（Capture the Flag, CTF）挑战中展现出自动化潜力，但其成功的关键因素尚不明确。
- 现有评估方法多采用二值化的通过/失败指标，无法衡量智能体的部分进度、推理质量和步骤级表现。
- 现有 CTF 基准数据集要么庞大笨重（如 NYU CTF Bench），要么缺乏系统的难度分析，不利于快速、资源受限的重复实验。
- 超参数（温度、top-p、最大 token 数）对智能体行为的影响在进攻性安全场景中缺乏系统研究。

整体含义：通过超参数调优、细粒度评估框架（LLM as Judge）和轻量级基准，为构建高效、可复现的 LLM 安全智能体提供方法论指导。

## 2. 方法论

### 2.1 核心思想
- **CTFJudge**：利用 LLM 作为裁判（judge），自动分析智能体的执行轨迹，并与人类专家的标准解决方案进行对齐，生成多维度分步骤评价。
- **CTF Competency Index (CCI)**：一个综合评分指标，衡量智能体解决方案与人类金标准在六个维度上的对齐程度，实现部分正确性量化。
- **CTFTiny**：从 NYU CTF Bench 中精选 50 个挑战组成的轻量级基准，覆盖二进制利用、逆向工程、密码学、Web 利用、取证和杂项，保证难度分布合理。
- **超参数调优**：系统改变温度、top-p 和最大 token 数，观察其对智能体成功率和推理质量的影响。

### 2.2 关键技术细节
- **CTFJudge 工作流程**：
  1. 将专家答案（write-ups）转化为结构化的步骤摘要，包含每个决策的意图、关键动作和推理依据。
  2. 将智能体的执行轨迹（包括计划、命令、输出、资源使用、时间）抽象为摘要格式。
  3. 让 LLM 裁判（使用 Claude 3.7 Sonnet，温度 0.1）沿着六个维度打分：漏洞理解、侦察全面性、利用方法、技术准确性、效率、适应性。
  4. 输出量化的 CCI 分数和定性反馈。
- **CCI 计算公式**：
  \[
  \text{CCI}(T, G) = \sum_{i=1}^n w_i F_i(T, G), \quad \sum w_i = 1
  \]
  其中 \(T\) 为智能体轨迹摘要，\(G\) 为人类金标准，\(F_i\) 为第 \(i\) 个评价因子，默认 \(n=6\)，初始权重均等。
- **CTFTiny 难度分类**：基于 12 个先前配置（D-CIPHER 和 CRAKEN）在 NYU CTF Bench 上的解决次数，划分为非常容易、容易、中等、困难四档，确保基准具有区分度。
- **超参数设置**：
  - 温度：{0, 0.2, 0.4, 0.6, 0.8, 1.0}
  - top-p：{0.25, 0.5, 0.75, 0.8, 0.85, 0.9, 0.95, 1.0}
  - 最大 token 数：{2048, 4096, 8192}
  - 默认基线：温度=1.0，top-p=1.0，max tokens=4096

### 2.3 算法/流程描述（文字说明）
1. 使用 D-CIPHER 多智能体框架（规划、执行、反馈）作为被评估的 agent。
2. 在 CTFTiny 基准上，对每个模型和超参数组合运行单次求解，记录轨迹和标志提取结果。
3. 对每个轨迹，CTFJudge 生成六个维度的 CCI 分数，并汇总为总体 CCI。
4. 通过 pass@1 和 CCI 两个指标，分析模型能力、超参数影响和失败模式。

## 3. 实验设计

### 3.1 数据集/场景
- **CTFTiny**：50 个真实 CTF 挑战，来自 NYU CTF Bench，涵盖 6 个领域：二进制利用 (pwn, 11个)、逆向工程 (rev, 16个)、密码学 (cry, 12个)、Web 利用 (web, 3个)、取证 (for, 2个)、杂项 (misc, 6个)。难度分布：非常容易 (6-9 配置解决)、容易 (4-6)、中等 (0-3)、困难 (0-3) 兼顾。

### 3.2 Benchmark
- 主要基准为 CTFTiny 本身，用于超参数扫描和模型对比。
- 基线方法：默认超参数下的 D-CIPHER 多智能体框架。

### 3.3 对比方法/模型
- **专有模型**：Claude 4 Sonnet (claude-sonnet-4-20250514)、GPT-4.1 (gpt-4.1-2025-04-14)、Gemini 2.5 Pro、Gemini 2.5 Flash
- **开源模型**：Llama-4-Maverick-17B、Qwen3-235B、DeepSeek-V3-0324
- 总共 7 个 LLM 作为智能体的基座模型。

### 3.4 评估指标
- pass@1：首次解决率
- CCI：部分正确性评分（0-1）
- 失败类别分析：21 种预定义失败原因

## 4. 资源与算力

- 论文未明确说明使用的 GPU 型号、数量和训练时长。
- 提到使用官方 API（Anthropic, OpenAI, Google）和 Together AI 平台进行推理。
- 仅提供了每个模型的单次运行成本（如 Claude 4 Sonnet $1.16/挑战，DeepSeek V3 $0.02/挑战），但未详述总计算资源。
- 结论：资源细节缺失，不利于复现能耗和算力需求。

## 5. 实验数量与充分性

### 实验数量
- **基线实验**：7 个模型 × 50 个挑战 = 350 次运行（每个模型一次，默认超参数）。
- **超参数扫描**：
  - 温度：6 个值，在 Claude 4 Sonnet 和 GPT-4.1 上测试（每个值跑 50 个挑战）→ 约 600 次运行。
  - top-p：8 个值，同样两个模型 → 约 800 次运行。
  - max tokens：3 个值 → 约 300 次运行。
- **CCI 分析**：对所有成功/失败的轨迹进行打分，产生约 350 个 CCI 分数。
- **失败分析**：对未解决的挑战分类（每个挑战可能对应多个原因），统计 21 个类别。
- 总计约 2000 次以上的独立 agent 运行，实验量充足。

### 充分性、客观性与公平性
- 覆盖了多种模型族、多种超参数组合，使用统一的多智能体框架 D-CIPHER，控制变量较严格。
- 使用 LLM as judge 进行细粒度打分，消除了人工主观偏差，但 LLM 裁判本身可能引入偏见（论文未深入讨论）。
- 所有实验在相同的 CTFTiny 基准上运行，难度分布合理，支持跨模型公平对比。
- 缺点：仅测试了 D-CIPHER 一种 agent 架构，未对比其他 agent 框架（如单 agent、ReAct 等），限制了结论的泛化性。

## 6. 主要结论与发现

1. **模型性能排名**：Claude 4 Sonnet 最好（pass@1=76%），其次是 Gemini 2.5 Flash（64%）、Gemini 2.5 Pro（48%）、GPT-4.1（40%）、Qwen3（28%）、DeepSeek V3（22%）、LLaMA-4 Maverick（8%）。说明专有模型在 CTF 任务上仍显著领先开源模型。
2. **超参数影响**：
   - 温度：高温度（1.0）对强模型（如 Claude）有益，可提高探索性；弱模型对温度不敏感或负影响。
   - top-p：接近 1.0 时性能最好，全分布访问有助于生成多样性。
   - max tokens：4096 为“金发姑娘”区间，过长（8192）导致注意力分散，过短（2048）限制推理深度。
3. **CCI 有效性**：CCI 能清晰区分成功与失败解：成功解 CCI 高且方差低，失败解 CCI 中等且存在明显短板维度（特别是利用方法、效率、适应性）。揭示了“通过/失败”指标隐藏的推理质量差异。
4. **失败模式**：最主要原因为“知识与领域经验差距”（Knowledge or Domain Expertise Gap），其次是“利用开发失败”和“侦察不足”。尽管 D-CIPHER 是多智能体协调框架，但“任务委派错误”和“环境失败”并不突出。
5. **领域差异**：逆向工程、密码学得分较高；杂项和二进制利用得分较低，因为需要更多与挑战服务器交互，增加不稳定因素。Web 利用相对模式化，表现较好。

## 7. 优点

- **细粒度评估框架**：CTFJudge 不依赖单一标志提取，而是从六个维度评估推理过程，提供可解释的诊断反馈，远超传统二值指标。
- **部分正确性指标 CCI**：能够量化智能体的“接近程度”，对训练和调试有实质指导意义。
- **轻量级基准 CTFTiny**：仅 50 个挑战，却保持了难度和领域的多样性，大幅降低评估成本，有利于超参数扫描和快速迭代。
- **超参数调优的系统性**：首次在 CTF 安全智能体上对温度、top-p、max tokens 进行联合扫描，提供了实用的配置指南。
- **多维度失败分析**：21 个失败原因分类，直观显示不同模型和领域的薄弱环节，有助于针对性地改进 agent 设计。

## 8. 不足与局限

- **评估框架通用性有限**：CTFJudge 使用固定、专家预定义的权重（六个维度均等），未验证在其他 CTF 或非 CTF 任务上的泛化能力。
- **仅测试单一 agent 架构**：所有实验基于 D-CIPHER 多智能体框架，未与单智能体、ReAct、反思型 agent 等对比，结论可能受框架特异性影响。
- **LLM 裁判偏差**：使用 Claude 3.7 Sonnet 作为裁判，可能对自身家族模型存在偏好或遗漏非标准推理方式（但论文未深入分析裁判偏差）。
- **资源消耗未透明**：尽管提出低成本基准，但未报告完整实验的总 GPU 小时数或 API 调用次数，不利于其他研究者估算复现成本。
- **超参数扫描不完整**：温度、top-p、max tokens 仅扫描两个模型（Claude 4 Sonnet 和 GPT-4.1），其他五个模型的敏感度未报告，结论代表性有局限。
- **实验重复性**：未说明是否进行了多次运行（如不同随机种子），不能评估超参数设置的稳定性。
- **应用场景限制**：方法基于 CTF 挑战，与现实渗透测试相比，环境可控、规模小，结论向真实网络攻防迁移时有风险。

（完）
