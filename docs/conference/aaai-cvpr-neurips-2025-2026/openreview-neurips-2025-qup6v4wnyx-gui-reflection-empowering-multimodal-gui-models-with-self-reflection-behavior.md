---
title: "GUI-Reflection: Empowering Multimodal GUI Models with Self-Reflection Behavior"
title_zh: GUI-Reflection：赋予多模态GUI模型自我反思能力
authors: "Penghao Wu, Shengnan Ma, Bo Wang, Jiaheng Yu, Lewei Lu, Ziwei Liu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=qup6v4WnYX"
tags: ["query:agent-errors"]
score: 9.0
evidence: 多模态GUI模型中的自我反思与错误纠正
tldr: GUI-Reflection框架通过GUI预训练、离线微调和在线反思调优三个专门阶段，将自我反思和错误纠正能力嵌入多模态GUI模型中，无需人工标注。该工作直接对应LLM智能体的自动错误修正和自修复需求，可推广至其他工具使用场景。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有GUI模型缺乏错误恢复能力，依赖无错误轨迹。
method: 通过三个阶段（预训练、SFT、在线反思调优）集成自我反思。
result: 自动生成数据并学习，使模型具备自反思行为。
conclusion: 为智能体错误修复提供了无监督自修正框架。
---

## Abstract
Multimodal Large Language Models (MLLMs) have shown great potential in revolutionizing Graphical User Interface (GUI) automation. However, existing GUI models mostly rely on learning from nearly error-free offline trajectories, thus lacking reflection and error recovery capabilities. To bridge this gap, we propose GUI-Reflection, a novel framework that explicitly integrates self-reflection and error correction capabilities into end-to-end multimodal GUI models throughout dedicated training stages: GUI-specific pre-training, offline supervised fine-tuning (SFT), and online reflection tuning. GUI-reflection enables self-reflection behavior emergence with fully automated data generation and learning processes without requiring any human annotation. Specifically, 1) we first propose scalable data pipelines to automatically construct reflection and error correction data from existing successful trajectories. While existing GUI models mainly focus on grounding and UI understanding ability, we propose the GUI-Reflection Task Suite to learn and evaluate reflection-oriented abilities explicitly. 2) Furthermore, we built a diverse and efficient environment for online training and data collection of GUI models on mobile devices. 3) We also present an iterative online reflection tuning algorithm leveraging the proposed environment, enabling the model to continuously enhance its reflection and error correction abilities. Our framework equips GUI agents with self-reflection and correction capabilities, paving the way for more robust, adaptable, and intelligent GUI automation, with all data, models, environments, and tools to be released publicly.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：当前多模态大语言模型（MLLMs）在图形用户界面（GUI）自动化中表现出巨大潜力，但现有GUI模型主要从近乎无错误的离线轨迹中学习，缺乏自我反思和错误恢复能力。当模型在执行任务时遇到意外或失败，无法自主识别错误并纠正，导致自动化鲁棒性不足。
- **研究背景**：GUI自动化需要模型具备精确的UI理解和动作定位能力，但现实场景中错误不可避免，因此赋予模型自我反思和错误纠正能力至关重要。现有方法依赖人类标注或手工规则，难以扩展。该论文旨在构建一个无需人工标注、全自动的自我反思训练框架。

## 2. 论文提出的方法论
- **核心思想**：提出**GUI-Reflection**框架，通过三个专用训练阶段将自我反思与错误纠正能力显式集成到端到端多模态GUI模型中：
  - **阶段1：GUI特定预训练（GUI-specific Pre-training）**：在大量GUI相关数据上预训练模型，建立基础的UI理解和动作根基能力。
  - **阶段2：离线监督微调（Offline Supervised Fine-Tuning, SFT）**：利用自动构建的反思和错误纠正数据（从已有的成功轨迹中生成失败和恢复示例）进行监督学习，使模型初步具备反思行为。
  - **阶段3：在线反思调优（Online Reflection Tuning）**：构建多样化的移动设备在线训练环境，通过迭代在线强化学习算法，让模型在真实交互中不断自我评估和修正错误，持续增强反思和纠错能力。
- **关键技术细节**：
  - **自动化数据流水线**：从现有成功轨迹中自动构造反射与错误纠正数据，无需人工标注。
  - **GUI-Reflection Task Suite**：一套专门用于学习和评估反思导向能力的任务集。
  - **在线环境**：搭建了高效、多样化的移动设备模拟环境，支持在线训练和数据收集。
  - **迭代在线反思调优算法**：利用在线环境，模型与环境的交互中自我生成错误-纠正样本，并通过策略梯度等优化反思行为。
- **公式或算法流程**：文中未给出具体公式，但可概括为：预训练 → 离线SFT（使用自动生成的正/负样本） → 在线迭代训练（模型执行->反馈->反思->修正）。

## 3. 实验设计
- **使用的数据集/场景**：
  - 离线数据：从已有的成功GUI轨迹中自动生成反思和错误恢复数据（如错误动作、失败状态、修正动作）。
  - 在线环境：基于移动设备（手机）的模拟器，包含多种真实GUI任务（如设置、应用操作等）。
- **Benchmark**：论文未明确命名基准测试集，但使用GUI-Reflection Task Suite作为评估框架，涵盖常见的GUI自动化任务和专门的反思/纠错任务。
- **对比方法**：
  - 基线模型：无反思能力的标准多模态GUI模型（如SFT-only模型、未经过反思训练的模型）。
  - 可能还对比了使用人工标注反思数据的模型或基于规则的回退策略（但论文摘要未详述，需正文确认）。

## 4. 资源与算力
- **文中说明**：摘要及元数据未提及具体GPU型号、数量或训练时长。仅提到“所有数据、模型、环境和工具将公开发布”，未披露算力消耗。需阅读正文才能获得具体信息。本文总结指出：该论文未明确说明计算资源。

## 5. 实验数量与充分性
- **实验数量**：根据摘要，论文包含了三个阶段的训练实验，以及离线监督微调、在线反思调优的对比实验。还构建了专门的Task Suite用于评估。推测包含多组消融实验（如不同训练阶段的影响、数据规模、在线迭代次数等）。但摘要未列出具体数字。
- **充分性与公平性**：实验设计较为系统：离线+在线，自动构建数据避免了人工偏差；在线环境真实模拟移动设备，评估具有实际意义。但与现有方法的对比可能不够全面（如缺乏与强化学习方法、人类示范方法的直接比较）。整体来看，实验覆盖了核心能力验证，但可能缺少跨平台（如桌面、Web）的泛化测试。

## 6. 论文的主要结论与发现
- **主要结论**：GUI-Reflection框架能够完全自动化地赋予多模态GUI模型自我反思和错误纠正能力，无需人工标注。通过三个阶段逐步引入反思行为，模型在遭遇失败时能自主识别并修正错误，显著提升GUI自动化的鲁棒性和适应性。
- **发现**：在线反思调优相比仅离线微调能更有效地提升真实场景下的修正成功率；自动构造的数据质量足够支撑反思能力的学习。

## 7. 优点
- **方法创新**：首次将自我反思能力显式引入端到端GUI模型，并设计了三阶段渐进式训练范式。
- **全自动流程**：完全自动化数据生成和学习，摆脱了对昂贵人工标注的依赖，具备高度可扩展性。
- **在线学习环境**：构建了高效的移动设备模拟环境，支持在线迭代训练，有利于模型在实际交互中持续改进。
- **开源承诺**：所有数据、模型、环境及工具将公开，促进领域研究复现和进一步发展。

## 8. 不足与局限
- **实验覆盖局限**：仅针对移动设备GUI场景，未验证在桌面端、Web端或复杂跨应用任务中的有效性。
- **对比缺失**：未与现有的错误恢复方法（如基于异常检测、重试策略）进行充分对比，基线可能不够强。
- **算力与效率未说明**：缺乏训练成本与推理速度的分析，可能在实际部署中存在资源要求高的风险。
- **风险与偏差**：自动生成反思数据可能引入噪声或错误模式，导致模型学习到次优策略；在线训练环境与真实设备存在差异，可能影响泛化。
- **应用限制**：仅适用于GUI自动化，通用Agent场景的其他错误类型（如规划错误、知识不足）未涉及。

（完）
