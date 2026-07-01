---
title: "GUI-360° : A Comprehensive Dataset and Benchmark for Computer-Using Agents"
title_zh: GUI-360°：面向计算机使用代理的全面数据集与基准
authors: "Jian Mu, Chaoyun Zhang, Chiming Ni, Lu Wang, Bo Qiao, Kartik Mathur, Qianhui Wu, Yuhang Xie, Xiaojun Ma, Mengyu Zhou, Si Qin, Liqun Li, Yu Kang, Minghua Ma, Qingwei Lin, Saravan Rajmohan, Dongmei Zhang"
date: 2025-09-13
pdf: "https://openreview.net/pdf?id=JLEneHy8qC"
tags: ["query:agent-traj"]
score: 8.0
evidence: 面向计算机使用代理的全面基准，包含多模态轨迹评估
tldr: 计算机使用代理面临真实任务缺乏、自动标注管线缺失等问题。本文提出GUI-360，包含自动化收集标注管线和超过120万执行步骤的轨迹，统一评估GUI基础、屏幕解析和动作预测。该基准为代理轨迹测试提供了大规模、多模态的标准化平台。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 缺乏真实世界计算机使用任务、自动化轨迹收集管线和统一基准。
method: 构建自动化管线，从查询源到环境模板、任务实例化，批量执行并LLM过滤，产出大规模轨迹数据集。
result: 发布超过120万执行步骤的轨迹，覆盖Windows办公应用，为代理评估提供资源。
conclusion: GUI-360填补了计算机使用代理基准的空白，推动轨迹评估标准化。
---

## Abstract
We introduce GUI-360°, a large-scale, comprehensive dataset and benchmark suite designed to advance computer-using agents (CUAs). CUAs present unique challenges and is constrained by three persistent gaps: a scarcity of real-world CUA tasks, the lack of automated collection-and-annotation pipelines for multi-modal trajectories, and the absence of a unified benchmark that jointly evaluates GUI grounding, screen parsing, and action prediction.

GUI-360° addresses these gaps with a largely automated pipeline for query sourcing, environment-template construction, task instantiation, batched execution, and LLM-driven quality filtering. The released corpus contains over 1.2M executed action steps across thousands of trajectories in popular Windows office applications, and includes full-resolution screenshots, accessibility metadata when available, instantiated goals, intermediate reasoning traces, and both successful and failed action trajectories. The dataset supports three canonical tasks, GUI grounding, screen parsing, and action prediction, and a hybrid GUI+API action space that reflects modern agent designs. Benchmarking state-of-the-art vision--language models on GUI-360° reveals substantial out-of-the-box shortcomings in grounding and action prediction; supervised fine-tuning yield significant gains.

---

## 论文详细总结（自动生成）

# GUI-360°：面向计算机使用代理的全面数据集与基准

## 1. 核心问题与整体含义（研究动机与背景）
- **研究动机**：计算机使用代理（CUA）在自动化图形界面操作方面具有巨大潜力，但当前研究受到三个关键瓶颈的制约：
  - 缺乏真实的、大规模的实际计算机使用任务数据；
  - 缺少自动化的多模态轨迹收集与标注管线；
  - 没有一个统一的基准能够同时评估 GUI 基础定位（GUI grounding）、屏幕解析（screen parsing）和动作预测（action prediction）。
- **整体含义**：现有基准要么规模小、任务单一，要么依赖人工标注，限制了 CUA 的发展。GUI-360° 致力于构建一个大规模、自动化、多任务统一的评测平台，推动 CUA 从学术原型迈向实际应用。

## 2. 方法论
- **核心思想**：设计一个高度自动化的管线，从查询生成到环境构建、任务实例化、批量执行再到质量过滤，全流程无需或极少人工干预，产出包含成功与失败轨迹的大规模多模态数据集。
- **关键技术细节**：
  - **查询源（Query Sourcing）**：从公开的办公软件教程、问答社区等获取真实用户意图。
  - **环境模板构建（Environment Template Construction）**：为 Windows 办公应用（如 Word、Excel、PowerPoint）构建模板化的虚拟环境，确保可重复执行。
  - **任务实例化（Task Instantiation）**：将查询转化为具体的、可执行的指令，并确定初始状态与目标状态。
  - **批量执行（Batched Execution）**：使用自动化脚本（如 UI Automation 和截图驱动的动作模拟）在模板环境中执行任务，记录每一步的屏幕截图、可访问性元数据、中间推理轨迹等。
  - **LLM 驱动的质量过滤（LLM-driven Quality Filtering）**：利用大语言模型判断轨迹是否合理、动作是否对应目标，剔除无效或错误轨迹。
- **数据产出**：超过 120 万执行步骤，数千条完整轨迹，覆盖 Windows 办公应用。每条轨迹包含全分辨率截图、可访问性树、实例化目标、中间推理步骤、动作序列（成功与失败）。
- **任务支持**：统一支持三个经典任务：
  - GUI grounding（定位界面元素）
  - Screen parsing（解析屏幕状态）
  - Action prediction（预测下一步动作）
- **动作空间**：混合 GUI+API 动作空间，既允许传统鼠标键盘操作，也允许调用系统 API，反映现代代理设计趋势。

## 3. 实验设计
- **数据集 / 场景**：使用 GUI-360° 自身产出的数据集，覆盖 Windows 版 Office 套件（Word、Excel、PowerPoint）的常见操作。
- **Benchmark 设置**：
  - 评估任务：GUI grounding、screen parsing、action prediction 三种典型任务。
  - 对比方法：主要评估当前最先进的视觉语言模型（VLM）的零样本（out-of-the-box）表现，并额外进行监督微调（SFT）对比。
- **主要实验结果**：当前 SOTA VLM 在 grounding 和 action prediction 上存在显著不足；通过监督微调（在 GUI-360° 数据上训练）可以带来大幅性能提升。

## 4. 资源与算力
- 论文中**未明确说明**所使用的 GPU 型号、数量、训练时长等具体算力信息。仅提到使用了某个规模的 VLM 进行 benchmark 和微调，但未给出详细硬件配置。

## 5. 实验数量与充分性
- **实验组数**：论文摘要没有提供具体实验数量统计，但从描述可推断至少包括：
  - 在三个任务上对多个 SOTA VLM 进行零样本评估；
  - 对同一个或多个模型进行监督微调后的对比实验。
- **充分性分析**：
  - **优点**：覆盖三种不同任务，评估维度较全面；同时比较零样本与微调效果，具有一定说服力。
  - **不足**：未提及消融实验（如不同质量过滤策略、不同任务组合等）；未暴露模型规模或训练数据量对结果的影响。在目前的摘要信息下，实验设计的细节尚不够透明，可能需要在全文阅读后进一步判断。

## 6. 主要结论与发现
- GUI-360° 填补了计算机使用代理领域中真实任务数据、自动化标注管线、统一基准的三项空白，提供了一个大规模、多模态、标准化的评测平台。
- 当前最先进的视觉语言模型在 GUI grounding 和 action prediction 上表现欠佳，说明该领域仍有较大提升空间。
- 利用 GUI-360° 的数据进行监督微调能够显著改善模型性能，证明了数据集的实用价值。

## 7. 优点
- **大规模自动化**：管线高度自动，产出超过 120 万步骤，是已知规模最大的 GUI 代理轨迹数据集之一。
- **多模态融合**：同时提供截图、可访问性树、推理文本等多种模态数据，支持多模态学习与评估。
- **统一评测**：将 GUI grounding、screen parsing、action prediction 整合进一个基准，便于横向比较。
- **混合动作空间**：兼顾 GUI 操作与 API 调用，更贴近真实代理设计。
- **包含失败轨迹**：同时保留成功与失败样本，有助于研究故障恢复与鲁棒性。

## 8. 不足与局限
- **领域覆盖有限**：仅针对 Windows 办公应用，未覆盖 macOS、Linux、Web 或其他主流桌面环境，迁移性待验证。
- **LLM 过滤偏差**：质量过滤依赖 LLM，可能引入模型自身的偏好或错误，导致数据集存在系统性偏差。
- **缺乏随机性与难度控制**：任务来自公开查询，可能难度分布不均，缺少对任务复杂度的显式标注。
- **算力细节缺失**：未提供训练/推理所需的硬件资源信息，不利于同行复现与成本估算。
- **实验细节不完整**：摘要中未交代消融实验、不同模型规模对比等，评估的充分性需要在全文确认。
- **无人工验证**：虽然声称自动化，但未提及对生成轨迹进行人工抽样验证，数据质量的可信度缺少独立评估。

（完）
