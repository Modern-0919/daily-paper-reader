---
title: "Poison as Cure: Visual Noise for Mitigating Object Hallucinations in LVMs"
title_zh: 毒药即良药：利用视觉噪声缓解大视觉语言模型中的目标幻觉
authors: "Kejia Zhang, Keda TAO, Jiasheng Tang, Huan Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=quKHZ3fcgx"
tags: ["query:agent-errors"]
score: 4.0
evidence: 利用视觉对抗扰动减轻大视觉语言模型的目标幻觉
tldr: 该研究提出视觉对抗扰动方法VAP，通过在输入图像中添加经过优化的视觉噪声来减缓大视觉语言模型中的目标幻觉。该方法将幻觉抑制形式化为优化问题，不修改基础模型，直接针对误读视觉信息导致的幻觉。虽不直接涉及工具使用，但可启发多模态智能体输出中的幻觉缓解。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 大视觉语言模型存在目标幻觉，生成看似合理但事实不准确的内容。
method: 通过对抗策略生成有益的视觉扰动，增强事实基础。
result: 有效降低了视觉语言模型的目标幻觉率。
conclusion: 视觉对抗扰动是一种无需修改模型即可缓解幻觉的有效方法。
---

## Abstract
Large vision-language models (LVMs) extend large language models (LLMs) with visual perception capabilities, enabling them to process and interpret visual information. A major challenge compromising their reliability is object hallucination that LVMs may generate plausible but factually inaccurate information. We propose a novel \textit{visual adversarial perturbation (VAP)} method to mitigate this hallucination issue. VAP alleviates LVM hallucination by applying strategically optimized visual noise without altering the base model. Our approach formulates hallucination suppression as an optimization problem, leveraging adversarial strategies to generate beneficial visual perturbations that enhance the model's factual grounding and reduce parametric knowledge bias. Extensive experimental results demonstrate that our method consistently reduces object hallucinations across 8 state-of-the-art LVMs, validating its efficacy across diverse evaluations.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：大视觉语言模型（LVMs）在生成视觉内容相关的描述时，常出现“目标幻觉”（object hallucination）现象——即生成看似合理但事实不准确的信息。这严重影响了模型的可靠性和实际应用价值。
- **研究动机**：现有缓解幻觉的方法通常需要修改模型架构或重新训练，成本高昂且可能影响原有能力。作者希望找到一种**无需修改模型**、**轻量级**且**可迁移**的幻觉抑制方法。
- **整体含义**：提出将“对抗扰动”（通常被视为有害的噪声）转化为“有益视觉噪声”的新视角，通过优化输入图像来增强模型的事实基础，减少参数化知识偏差，从而在不改变模型参数的前提下有效降低幻觉率。

## 2. 方法论：核心思想、关键技术细节、算法流程

- **核心思想**：利用对抗性策略生成有益视觉扰动（Visual Adversarial Perturbation, VAP），将幻觉抑制形式化为**优化问题**。具体来说，系统地在输入图像中添加经过精心优化的视觉噪声，使得模型更倾向于依赖视觉信息而非错误的参数化记忆，从而减少幻觉输出。
- **关键技术细节**：
  - **无需修改基础模型**：VAP仅在推理阶段对输入图像施加扰动，不改变模型权重或结构。
  - **优化目标**：设计损失函数，最小化模型对幻觉目标的响应，同时最大化对真实视觉特征的依赖。
  - **对抗性策略**：借鉴对抗攻击中的梯度优化方法，但目标是生成“有益”噪声而非有害攻击。
- **算法流程（文字说明）**：
  1. 输入原始图像和提示文本。
  2. 初始化一个微小噪声扰动（通常为随机噪声）。
  3. 将扰动叠加到原始图像上，送入LVM生成输出。
  4. 根据输出中的幻觉程度（如不存在的物体名称）计算损失。
  5. 通过反向传播计算梯度，更新噪声参数（通常采用投影梯度下降或类似方法）。
  6. 重复迭代直至达到停止条件（如损失收敛或达到最大步数）。
  7. 输出最终的扰动图像用于模型推理。

> **注**：论文PDF提取文本仅包含摘要，未提供详细公式和算法步骤，以上描述基于摘要和常见对抗扰动方法的合理推断。

## 3. 实验设计

- **使用的数据集/场景**：摘要未明确说明具体数据集名称，但提到在**8种最先进的大视觉语言模型**上进行评估，覆盖多种评估场景（如不同任务、不同图像分布）。
- **Benchmark**：未提及特定benchmark名称，可能采用公开的幻觉评估基准（如POPE、CHAIR等常见指标）。
- **对比方法**：未列出具体对比基线。根据常见做法，可能对比了无扰动基线、随机噪声扰动、其他幻觉缓解方法（如重新训练、解码策略等）。

## 4. 资源与算力

- **文中未明确说明**：给定的PDF提取文本中没有提及任何GPU型号、数量、训练时长或算力成本信息。无法从当前信息推断。

## 5. 实验数量与充分性

- **实验数量**：摘要提到“across 8 state-of-the-art LVMs”和“diverse evaluations”，表明实验覆盖了多个主流模型和多种评估指标，但未给出具体实验组数（如消融实验、不同噪声强度、不同任务等）。
- **充分性与公平性**：
  - **正面**：覆盖8个模型，具有较好的泛化性验证。
  - **不足**：未说明是否进行了**消融实验**（如噪声幅度、优化迭代次数的影响）、**跨任务泛化**（如分类、检测、问答等）、**与现有方法的严格对比**。实验设计的客观性和公平性仅凭摘要无法全面判断。

## 6. 主要结论与发现

- VAP方法能够**一致性地降低**多个最先进LVMs的目标幻觉率，验证了其有效性。
- 表明**对抗性策略**可以转化为有益工具，用于增强模型的事实基础，而非仅用于攻击。
- 提供了一种**无需修改模型**的轻量级幻觉缓解途径，具有实际部署潜力。

## 7. 优点

- **创新性**：将通常被视为有害的对抗扰动转化为有益噪声，概念新颖，从“毒药”到“良药”的视角转变。
- **实用性**：不修改模型权重，直接作用于输入，可即插即用，适用于任何LVM，迁移性高。
- **有效性**：在多个主流模型上实验验证，展示出广泛适用性。
- **形式化**：将幻觉抑制建模为优化问题，为后续研究提供数学框架。

## 8. 不足与局限

- **实验细节缺失**：未提供具体数据集、对比方法、评估指标等关键信息，从摘要难以判断实验的全面性。
- **算力成本未知**：未说明生成扰动所需的计算资源，可能带来额外推理开销（需迭代优化）。
- **应用限制**：
  - 可能仅针对目标幻觉，对其他类型幻觉（如关系幻觉、属性幻觉）效果未知。
  - 扰动图像可能引入可见伪影，影响用户观感。
  - 优化过程需要白盒访问模型梯度，黑盒场景下可能不适用。
- **偏差风险**：未讨论若扰动被对抗性利用（如恶意攻击者利用类似技术产生误导）的风险。

（完）
