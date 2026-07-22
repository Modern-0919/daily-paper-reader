---
title: Fact or Hallucination? An Entropy-Based Framework for Attention-Wise Usable Information in LLMs
title_zh: 事实还是幻觉？基于熵的LLM注意力可用信息框架
authors: "Vishal Pramanik, Susmit Jha, Alvaro Velasquez, Sumit Kumar Jha"
date: 2025-05-11
pdf: "https://openreview.net/pdf?id=VMcClInij6"
tags: ["query:agent-output"]
score: 9.0
evidence: 利用注意力熵检测LLM输出中的幻觉
tldr: Shapley NEAR提出了一种基于熵的归因框架，通过分解所有层和注意力头的信息流来为LLM输出分配置信度分数，从而检测幻觉。该方法比传统基于logits的方法更细粒度，可泛化到智能体输出监控和一致性检查。
source: NeurIPS-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 现有幻觉检测方法忽略中间层语义信号，精度有限。
method: 提出Shapley NEAR框架，利用Shapley值分解注意力信息流并计算熵。
result: 产出置信度分数，分数越高表示幻觉风险越低。
conclusion: 提供了一种强有力的幻觉检测工具，可应用于智能体输出验证。
---

## Abstract
Large language models (LLMs) often generate confident yet inaccurate outputs, posing serious risks in safety-critical applications. Existing hallucination detection methods typically rely on final-layer logits or post-hoc textual checks, which can obscure the rich semantic signals encoded across model layers. Thus, we propose Shapley NEAR (Norm-basEd Attention-wise usable infoRmation), a principled, entropy-based attribution framework grounded in Shapley values that assigns a confidence score indicating whether an LLM output is hallucinatory. Unlike prior approaches, Shapley NEAR decomposes attention-driven information flow across all layers and heads of the model, where higher scores correspond to lower hallucination risk. It further distinguishes between two hallucination types: parametric hallucinations, caused by the model’s pre-trained knowledge overriding the context, and context-induced hallucinations, where misleading context fragments spuriously reduce uncertainty. To mitigate parametric hallucinations, we introduce a test-time head clipping technique that prunes attention heads contributing to overconfident, context-agnostic outputs. Empirical results in four QA benchmarks (CoQA, QuAC, SQuAD, and TriviaQA), using Qwen2.5-3B, LLaMA3.1-8B, and OPT-6.7B, demonstrate that Shapley NEAR outperforms strong baselines, without requiring additional training, prompting, or architectural modifications.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与整体含义（研究动机和背景）

大型语言模型（LLM）经常生成自信但错误的输出（幻觉），这在安全关键应用中构成严重风险。现有幻觉检测方法大多依赖最后一层的logits或事后文本检查，忽略了模型各层编码的丰富语义信号。因此，该论文旨在提出一种更细粒度、基于中间层注意力信息流的方法，以更准确地检测幻觉并区分其类型。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出 **Shapley NEAR**（Norm-basEd Attention-wise usable infoRmation）框架，一种基于熵和Shapley值的归因方法，为LLM输出分配一个置信度分数（分数越高，幻觉风险越低）。
- **关键技术细节**：
  - 利用Shapley值分解所有层和所有注意力头的信息流，提取每个头对输出的“可用信息”，并基于熵度量计算不确定性。
  - 区分两种幻觉类型：
    - **参数幻觉（parametric hallucinations）**：由模型预训练知识覆盖上下文导致。
    - **上下文诱导幻觉（context-induced hallucinations）**：由误导性上下文片段虚假地降低不确定性导致。
  - 针对参数幻觉，提出**测试时头部裁剪技术（test-time head clipping）**：修剪那些导致过度自信、与上下文无关输出的注意力头。
- **算法流程（文字说明）**：
  1. 输入提示和输出，遍历模型所有层和注意力头。
  2. 计算每个注意力头的Shapley值，量化其对最终输出的贡献。
  3. 基于Shapley值计算注意力熵，得到归一化的可用信息得分。
  4. 通过得分阈值判断是否为幻觉；对参数幻觉，采用头部裁剪进一步调整。
- **无需额外训练、提示或架构修改**。

## 3. 实验设计

- **数据集**：四个问答（QA）基准数据集：**CoQA、QuAC、SQuAD、TriviaQA**。
- **模型**：三种不同规模LLM：**Qwen2.5-3B、LLaMA3.1-8B、OPT-6.7B**。
- **对比方法（baselines）**：论文提到“优于强基线”，但未列出具体基线名称（推测包括基于logits的置信度方法、基于文本检查的方法等）。
- **评测指标**：基于置信度分数与幻觉标签的对比（准确率/召回率/F1等，具体未在摘要中详细说明）。

## 4. 资源与算力

论文未明确说明使用的GPU型号、数量或训练时长。仅指出方法不需要额外训练，因此测试阶段计算量相对较低，但未量化。

## 5. 实验数量与充分性

- **实验数量**：四个数据集 × 三种模型 = 至少12个主要实验；加上针对参数幻觉的头部裁剪技术消融实验（摘要提及），以及幻觉类型区分分析。
- **充分性评价**：数据集覆盖多种问答场景（对话、阅读理解、知识问答），模型尺寸跨度3B-8B，具有一定代表性。但缺乏开放生成任务（如摘要、翻译）的测试，且未对比更多基线方法（如基于概率的、基于采样的方法）。总体充分但可进一步扩展。

## 6. 主要结论与发现

- Shapley NEAR能有效检测幻觉，且比传统基于logits的方法更准确。
- 能够区分两种幻觉类型（参数性 vs. 上下文诱导性），有助于针对性缓解。
- 提出的测试时头部裁剪技术可有效减少参数幻觉。
- 方法不需要额外训练或模型修改，具有良好泛化性。

## 7. 优点

- **细粒度**：利用所有层和注意力头的信息，而非仅最后一层。
- **理论根基**：基于Shapley值进行公平归因，具有可解释性。
- **无训练开销**：即插即用，适合实际部署。
- **双重幻觉分类**：有助于理解幻觉根源并设计缓解策略。
- **实验覆盖多模型、多数据集**：验证了跨模型和跨域的有效性。

## 8. 不足与局限

- **实验覆盖有限**：仅涵盖问答任务，未在生成式任务（如翻译、摘要、代码生成）中验证。
- **基线对比不透明**：未列出具体对比方法名称，公平性难以评估。
- **计算复杂度**：Shapley值计算可能需要近似方法，实际推理速度可能受影响（文中未讨论运行时间）。
- **头部裁剪的鲁棒性**：测试时裁剪可能在不同分布下失效，未进行分布外测试。
- **缺乏开源代码和复现细节**：摘要未提及，可能影响可复现性。

（完）
