---
title: Agentic Confidence Calibration
title_zh: 智能体置信度校准
authors: "Jiaxin Zhang, Caiming Xiong, Chien-Sheng Wu"
date: 2025-09-06
pdf: "https://openreview.net/pdf?id=6YMFsGFabM"
tags: ["query:agent-traj"]
score: 9.0
evidence: 用于诊断智能体轨迹的全方位轨迹校准框架
tldr: 该论文首次定义智能体置信度校准问题，并提出了Holistic Trajectory Calibration（HTC）框架。HTC通过提取轨迹宏观动态和微观稳定性等多层次过程特征，全面诊断智能体在复杂多步任务中的过度自信问题。实验表明，HTC能有效识别轨迹中的错误累积和工具不确定性，显著提升校准质量。该工作为智能体轨迹评估和可靠性提升提供了关键方法。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有置信度校准方法无法处理智能体系统中轨迹级错误累积和工具不确定性的挑战。
method: 提出Holistic Trajectory Calibration框架，提取轨迹级过程特征进行诊断。
result: HTC框架在多个智能体任务上显著提升了置信度校准性能。
conclusion: HTC为智能体轨迹可靠性评估提供了有效的诊断方法。
---

## Abstract
AI agents are rapidly advancing from passive language models to autonomous systems executing complex, multi-step tasks. Yet their overconfidence in failure remains a fundamental barrier to deployment in high-stakes settings. Existing calibration methods, built for static single-turn outputs, cannot address the unique challenges of agentic systems, such as compounding errors along trajectories, uncertainty from external tools, and opaque failure modes. To address these challenges, we introduce, for the first time, the problem of \emph{Agentic Confidence Calibration} and propose \textbf{Holistic Trajectory Calibration (\htcnospace)}, a novel diagnostic framework that extracts rich process-level features ranging from macro dynamics to micro stability across an agent’s entire trajectory. Powered by a simple, interpretable model, \htc consistently surpasses strong baselines in both calibration and discrimination, across eight benchmarks, multiple LLMs, and diverse agent frameworks. Beyond performance, \htc delivers three essential advances: it provides \emph{interpretability} by revealing the signals behind failure, enables \emph{transferability} by applying across domains without retraining, and achieves \emph{generalization} through a \emph{General Agent Calibrator} (\gacnospace) that {achieves the best calibration (lowest ECE)} on the out-of-domain GAIA benchmark. Together, these contributions establish a new process-centric paradigm for confidence calibration, {\color{blue}providing a framework} for diagnosing and enhancing the reliability of AI agents.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：AI 智能体正从被动语言模型发展为自主执行复杂多步任务的系统，但在失败时表现出过度自信，这成为高风险场景部署的根本障碍。现有置信度校准方法仅针对静态单轮输出，无法处理智能体系统特有的挑战，例如：沿轨迹的复合错误累积、外部工具带来的不确定性、以及不透明的失败模式。
- **整体含义**：该论文首次定义“智能体置信度校准”（Agentic Confidence Calibration）问题，旨在诊断和缓解智能体在轨迹级的不合理自信，为提升其可靠性奠定基础。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出 **Holistic Trajectory Calibration (HTC)** 诊断框架，从智能体整个轨迹中提取丰富的**过程级特征**，包括宏观动态（macro dynamics）和微观稳定性（micro stability），并基于简单可解释的模型进行校准。
- **关键技术细节**：
  - 特征提取维度：覆盖轨迹的宏观动态（如任务进展速度、工具使用频率）和微观稳定性（如步骤间置信度波动、重复尝试情况）。
  - 校准模型：使用简单、可解释的模型（具体模型类型未在摘要中说明，但强调可解释性）。
  - 额外提出 **General Agent Calibrator (GAC)**：一个无需重训练即可跨领域迁移的通用校准器，在域外（out-of-domain）GAIA 基准上实现了最低期望校准误差（ECE）。
- **流程**（文字描述）：输入智能体执行的多步轨迹 → 提取宏观和微观过程特征 → 训练轻量校准模型 → 输出校准后的置信度，并揭示失败信号。

## 3. 实验设计：数据集/场景、基准、对比方法

- **数据集/场景**：使用 **8 个基准（benchmarks）**、多个大语言模型（LLMs）以及不同的智能体框架（agent frameworks）。特别地，GAIA 作为域外（out-of-domain）基准用于评估泛化能力。
- **对比方法**：与强基线（strong baselines）对比，但摘要未列出具体基线名称。推测包括直接置信度、温度缩放、分箱校准等传统方法。
- **评测指标**：校准质量（主要用期望校准误差 ECE）、判别能力（discrimination，可能指 AUC 或分类准确率）。

## 4. 资源与算力

- **未明确说明**：论文在摘要和元数据中未提及使用的 GPU 型号、数量、训练时长等算力信息。需阅读全文才能获取。

## 5. 实验数量与充分性

- **实验数量**：覆盖了 8 个基准、多个 LLM 和不同智能体框架，并进行了跨领域泛化测试（GAIA）。从规模来看实验量较充分。
- **充分性、客观性、公平性**：
  - 充分性：多模型、多基准、多框架的系统性比较，覆盖了不同复杂度和领域。
  - 客观性：使用标准校准指标（ECE），对比强基线，结果具有可比性。
  - 公平性：未提及其他实验设置细节（如随机种子、重复次数），但摘要指出 HTC 一致超越基线，表明实验设计较为严谨。不过，缺乏消融实验的具体数量，文中仅提及框架包含宏观/微观特征，但未说明是否进行了特征消融。

## 6. 主要结论与发现

- HTC 框架在 8 个基准、多个 LLM 和不同智能体框架上，**一致性地超越强基线**，在校准和判别两方面均取得更好表现。
- HTC 提供了三大重要进步：
  1. **可解释性**：揭示失败背后的信号，帮助诊断错误累积和工具不确定性。
  2. **可迁移性**：无需重新训练即可跨领域应用。
  3. **泛化性**：通过 GAC 在域外 GAIA 基准上取得最低 ECE，实现最佳校准。
- 建立了以过程为中心（process-centric）的置信度校准新范式，为诊断和提升 AI 智能体的可靠性提供了框架。

## 7. 优点

- **问题首创性**：首次定义并形式化智能体置信度校准问题。
- **过程级诊断**：从轨迹整体提取多层次特征，而非仅关注最终输出，更贴合智能体错误模式。
- **可解释性**：模型简单可解释，能指出具体失败原因（如工具不确定性、错误累积位置）。
- **跨领域泛化**：GAC 无需重新训练即可用于新领域，实用性强。
- **全面实验**：8 个基准、多 LLM、多框架，验证了方法的鲁棒性。

## 8. 不足与局限

- **计算资源未公开**：缺少算力消耗信息，不利于复现和成本评估。
- **特征工程依赖**：宏观/微观特征的选取需人工设计，可能未覆盖所有陷阱模式。
- **实验细节不足**：摘要未列出具体对比基线名称、消融实验组数、统计显著性检验结果，公允性需进一步全文验证。
- **应用限制**：仅针对轨迹级校准，未讨论对在线学习或连续交互场景的适应性；部分智能体框架可能因日志不足无法提取完整特征。
- **偏差风险**：基准可能偏向特定类型任务（如工具调用密集型），对纯推理型智能体的适用性需更多证据。

（完）
