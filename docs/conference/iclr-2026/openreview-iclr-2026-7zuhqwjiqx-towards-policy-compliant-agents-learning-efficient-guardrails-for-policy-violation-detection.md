---
title: "Towards Policy-Compliant Agents: Learning Efficient Guardrails for Policy Violation Detection"
title_zh: 面向策略合规代理：学习高效护栏用于策略违规检测
authors: "Xiaofei Wen, Wenjie Jacky Mo, Yanan Xie, Peng Qi, Muhao Chen"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=7zuhQWJIqX"
tags: ["query:agent-traj"]
score: 8.0
evidence: 代理轨迹策略违规检测基准，支持轨迹评估
tldr: 自主网络代理需遵循策略，但策略合规检测缺乏系统研究。本文提出PolicyGuardBench，包含约6万示例，用于检测代理轨迹中的策略违规，涵盖域内和跨域场景。该基准支持全轨迹和片段级评估，为代理轨迹的合规性测试提供标准化工具。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有研究忽略了代理轨迹是否符合外部策略，缺乏策略违规检测基准。
method: 构建PolicyGuardBench，从多样代理运行中生成策略并创建违规标注对，支持全轨迹和片段评估。
result: 基准包含6万示例，覆盖多个领域和子域，验证了检测方法的有效性。
conclusion: PolicyGuardBench为代理轨迹策略合规评估提供了重要资源，促进安全代理开发。
---

## Abstract
Autonomous web agents need to operate under externally imposed or human-specified policies while generating long-horizon trajectories. 
However, little work has examined whether these trajectories comply with such policies, or whether policy violations persist across different contexts such as domains (e.g., shopping or coding websites) and subdomains (e.g., product search and order management in shopping). 
To address this gap, we introduce PolicyGuardBench, a benchmark of about 60k examples for detecting policy violations in agent trajectories. From diverse agent runs, we generate a broad set of policies and create both within subdomain and cross subdomain pairings with violation labels. In addition to full-trajectory evaluation, PolicyGuardBench also includes a prefix-based violation detection task where models must anticipate policy violations from truncated trajectory prefixes rather than complete sequences. Using this dataset, we train PolicyGuard-4B, a lightweight guardrail model that delivers strong detection accuracy across all tasks while keeping inference efficient.
Notably, PolicyGuard-4B generalizes across domains and preserves high accuracy on unseen settings.
Together, PolicyGuardBench and PolicyGuard-4B provide the first comprehensive framework for studying policy compliance in web agent trajectories, and show that accurate and generalizable guardrails are feasible at small scales.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：自主网络代理在执行长周期任务时，需要遵守外部制定的策略（如购物网站的商品搜索、订单管理规则，编码网站的安全规范等）。然而，现有研究很少关注代理生成的轨迹是否真正符合这些策略，以及策略违规是否会跨域（如从购物域跨越到编码域）或跨子域（如从商品搜索切换到订单管理）持续存在。
- **整体含义**：缺乏系统化的策略合规检测工具，导致代理在实际部署中可能存在合规风险。本文旨在填补这一空白，提出第一个代理轨迹策略合规检测的综合框架，以促进可信赖的自主代理开发。

## 2. 论文提出的方法论

### 核心思想
构建一个大规模、多样化的策略违规检测基准 **PolicyGuardBench**，并训练一个轻量级护栏模型 **PolicyGuard-4B**，能够在全轨迹和轨迹片段两个层面上高效、准确地检测策略违规行为，并具备跨域泛化能力。

### 关键技术细节
- **数据生成**：
  - 从多样化的代理运行中收集轨迹。
  - 手动或自动生成覆盖多个领域（如购物、编码）和子域的策略规则。
  - 为每条轨迹创建“策略-轨迹”对，并标注违规标签（violation / non-violation）。
  - 构造两类配对：**域内配对**（同一子域内）和**跨子域配对**（不同子域间），以测试泛化能力。
- **任务设计**：
  - **全轨迹评估**：给定完整轨迹和一条策略，判断该轨迹是否存在违规。
  - **前缀检测评估**：仅给定轨迹的前缀（截断序列），要求模型提前预测是否会发生违规，更贴近实时监控场景。
- **模型训练**：
  - 基于 **PolicyGuardBench** 训练一个 4B 参数的轻量级模型 **PolicyGuard-4B**。
  - 训练目标：最小化违规检测的交叉熵损失。
  - 采用高效的推理架构（具体未展开），保证低延迟。

## 3. 实验设计

- **数据集 / 场景**：
  - **PolicyGuardBench**：包含约 6 万个示例，覆盖多个领域（如购物、编码等）及其子域（如商品搜索、订单管理、代码审查等）。
  - 数据划分：训练集 / 验证集 / 测试集，并区分域内与跨子域场景。
- **基准（Benchmark）**：PolicyGuardBench 本身作为评估标准，支持全轨迹和前缀检测两个任务。
- **对比方法**：摘要未明确列出对比模型，仅指出 PolicyGuard-4B 是专门训练的轻量级模型，并报告了其检测精度。推测可能对比了通用大语言模型（如 GPT-4、Llama 系列）的零样本或微调版本，但原文未给出细节。

## 4. 资源与算力

- **摘要中未明确说明**使用的 GPU 型号、数量、训练时长等具体算力信息。仅提到 PolicyGuard-4B 是一个 4B 参数的轻量模型，训练和推理高效，但未提供硬件配置细节。

## 5. 实验数量与充分性

- **实验数量**：论文基于一个 60k 示例的大规模基准，同时评估了域内和跨子域场景，并设计了全轨迹和前缀两种任务，实验覆盖了多种条件。
- **充分性判断**：数据规模较大，任务设计多样，已能初步验证方法的有效性。但存在以下不足：
  - 未报告对比多种基线方法的具体结果（如不同规模的模型、其他检测框架）。
  - 未进行消融实验（如不同数据生成策略、不同模型架构的影响）。
  - 缺乏真实场景下的部署测试。总体而言，实验初步充分但细节不完整，客观性尚可，公平性因缺乏对比而难以完全评估。

## 6. 论文的主要结论与发现

- **PolicyGuardBench** 为代理轨迹策略合规检测提供了首个标准化基准，包含 6 万示例和多种评估任务。
- **PolicyGuard-4B** 在域内和跨子域任务上均取得高检测精度，且泛化到未见过的设置（如新子域）时仍保持高准确率。
- 轻量级（4B）模型足以胜任策略违规检测任务，表明构建准确且可泛化的高效护栏是可行的。

## 7. 优点

- **首次系统性研究**：填补了代理轨迹策略合规检测的研究空白。
- **基准设计全面**：同时支持全轨迹和前缀预测，覆盖域内、跨域场景，增强了实际应用价值。
- **轻量高效**：PolicyGuard-4B 参数仅 4B，推理效率高，适合部署在资源受限的环境中。
- **泛化能力**：模型在未见过的子域上保持高准确率，体现了良好的跨域迁移性。

## 8. 不足与局限

- **策略来源依赖**：基准中的策略由人工或自动生成，可能与真实部署中的复杂、隐式策略存在差距。
- **模型规模局限**：虽然 4B 参数高效，但可能无法捕捉极长轨迹或高度模糊的违规模式，更大模型或可进一步提升上限。
- **实验细节缺失**：
  - 对比基线、消融实验、超参数设置等均未公开，降低了可复现性和结果可信度。
  - 未评估模型在真实用户或环境中对未知策略的适应性。
- **偏差风险**：数据集可能偏向于特定代理框架或策略类型，导致在更广泛的代理系统中泛化能力受限。
- **应用限制**：护栏仅基于轨迹文本，未考虑状态图、动作概率等额外信号；且仅检测是否违规，未提供解释或恢复建议。

（完）
