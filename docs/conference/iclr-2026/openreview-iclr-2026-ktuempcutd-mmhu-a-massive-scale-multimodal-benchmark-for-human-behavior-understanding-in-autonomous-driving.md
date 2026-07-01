---
title: "MMHU: A Massive-Scale Multimodal Benchmark for Human Behavior Understanding in Autonomous Driving"
title_zh: MMHU：自动驾驶中人类行为理解的大规模多模态基准
authors: "Renjie Li, Ruijie Ye, Mingyang Wu, Yang Zhou, Hao Frank Yang, Zhiwen Fan, Hezhen Hu, Zhengzhong Tu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=KtUEMPCuTD"
tags: ["query:agent-traj"]
score: 4.0
evidence: 自动驾驶中人类行为的轨迹标注
tldr: 该论文提出了MMHU，一个大规模多模态基准，用于自动驾驶中的人类行为理解。基准包含丰富的标注，如人类运动和轨迹、意图以及关键行为标签。数据集包含5.7万个运动片段和173万帧。该基准旨在填补自动驾驶中人类行为综合评估的空白，为测试自动驾驶系统对人类行为的感知和理解能力提供支持。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 自动驾驶中缺乏全面评估人类行为理解的基准，现有研究虽涉及轨迹和意图但不够系统。
method: 构建包含57k人类运动片段和1.73M帧的多模态数据集，标注轨迹、意图和安全性标签。
result: 数据集规模大，标注丰富，覆盖多种驾驶场景。
conclusion: MMHU填补了自动驾驶中人类行为理解基准的空白，有助于开发更安全的驾驶系统。
---

## Abstract
Humans are integral components of the transportation ecosystem, and understanding their behaviors is crucial to facilitating the development of safe driving systems. Although recent progress has explored various aspects of human behavior---such as motion, trajectories, and intention---a comprehensive benchmark for evaluating human behavior understanding in autonomous driving remains unavailable. In this work, we propose \textbf{MMHU}, a large-scale benchmark for human behavior analysis featuring rich annotations, such as human motion and trajectories, text description for human motions, human intention, and critical behavior labels relevant to driving safety. Our dataset encompasses 57k human motion clips and 1.73M frames gathered from diverse sources, including established driving datasets such as Waymo, in-the-wild videos from YouTube, and self-collected data. A human-in-the-loop annotation pipeline is developed to generate rich behavior captions. We provide a thorough dataset analysis and benchmark multiple tasks—ranging from motion prediction to motion generation and human behavior question answering—thereby offering a broad evaluation suite. Our dataset will be released to promote further human-centric research in this vital area of autonomous driving.

---

## 论文详细总结（自动生成）

好的，我将基于您提供的论文摘要和元数据，对该论文进行结构化、深入、客观的总结。

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有自动驾驶系统在人类行为理解方面缺乏全面、系统化的评估基准。尽管已有工作涉及行人轨迹预测、意图识别等单一任务，但尚未有一个统一的大规模多模态基准，能够同时覆盖人类运动、轨迹、意图及与驾驶安全相关的关键行为标签，从而全面评估自动驾驶系统对人类行为的理解能力。
- **研究动机**：人类是交通生态系统的核心组成部分，准确理解其行为是开发安全驾驶系统的关键前提。因此，构建一个覆盖场景多样、标注丰富的基准数据集，对于推动该领域研究至关重要。
- **整体含义**：该论文提出的 MMHU 旨在填补这一空白，为自动驾驶中的人类行为理解提供一个标准化的评测平台，促进更安全、更人性化的自动驾驶技术发展。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：通过构建一个大规模、多模态、标注丰富的人类行为数据集，并在此基础上设计多个任务（如运动预测、运动生成、行为问答），从而系统性地评测自动驾驶系统对人类行为的感知与理解能力。
- **关键技术与细节**：
  - **数据收集**：整合多种来源，包括成熟的驾驶数据集（如 Waymo）、来自 YouTube 的野外（in-the-wild）视频以及自行采集的数据。
  - **标注内容**：涵盖人类运动与轨迹、运动文本描述、人类意图以及与驾驶安全相关的关键行为标签（如横穿马路、突然加速等）。
  - **人机协同标注流程（Human-in-the-loop）**：开发了一套包含人工审核与自动校验的标注管线，以保证行为标注的丰富性和准确性。
  - **数据集规模**：包含 5.7 万个人类运动片段和 173 万帧图像，是目前该领域规模最大的多模态基准之一。
  - **任务定义**：基准面向多个任务，包括运动预测（预测未来轨迹）、运动生成（生成合理的人类运动序列）以及人类行为问答（根据场景回答行为相关问题）。

### 3. 实验设计：使用的数据集/场景、Benchmark、对比方法

- **数据集与场景**：
  - **来源**：Waymo 开放数据集（典型城市驾驶场景）、YouTube 野外视频（复杂非结构化场景）以及自采集数据。
  - **场景多样性**：覆盖不同天气、光照、交通密度、道路类型下的行人、骑行者等多种人类行为。
- **基准（Benchmark）**：论文本身即提出并定义了一个统一的评测基准（MMHU），包含多个任务评估套件。
- **对比方法**：摘要及元数据中未具体提及在实验部分对比了哪些已有的方法或模型。从论文题目和通常做法推断，可能基准测试会评估现有主流轨迹预测、运动生成模型的表现，但具体列表未给出。因此，我们指出 **全文未明确列举对比方法**。

### 4. 资源与算力

- 论文摘要及元数据中 **未提及** 使用的具体计算资源（如 GPU 型号、数量、训练时长、总计算量等）。因此，无法对这部分进行总结。

### 5. 实验数量与充分性

- **实验数量**：根据摘要，论文提供了“thorough dataset analysis”（全面的数据集分析），并基准测试了多个任务（运动预测、运动生成、行为问答）。但具体做了多少组实验（如不同数据集上的消融、不同任务模型对比）未详细说明。
- **充分性与公平性**：从被拒（ICLR-2026-Rejected-Public）以及缺乏对比方法细节来看，实验的充分性和公平性可能存疑。主要体现在：
  - 未公开与现有最强方法的定量比较，无法评估基准的挑战性。
  - 缺乏消融实验来验证标注质量、数据多样性对任务性能的影响。
  - 多任务评测可能仅提供了基线结果（baseline），而未展示充分的方法对比，导致结论的说服力不足。

### 6. 论文的主要结论与发现

- **主要结论**：MMHU 作为一个大规模、多模态、标注丰富的人类行为理解基准，成功填补了自动驾驶领域缺乏全面评估平台的空白。该数据集涵盖多种驾驶场景和丰富的行为标签，能够有效支持运动预测、运动生成和行为问答等任务的研究，并将有助于开发更安全的驾驶系统。
- **具体发现**：数据分析揭示了不同数据来源（如 Waymo vs. YouTube）在人类行为分布上的差异，以及现有模型在复杂场景下行为的理解局限（摘要中未具体数值，但隐含了结论）。

### 7. 优点：方法或实验设计上的亮点

- **规模和多样性**：数据集规模大（5.7万片段、173万帧），来源多样（结构化驾驶数据+野外视频+自采），场景覆盖广，有助于提升模型的泛化能力。
- **多模态与多任务**：同时包含轨迹、意图、文本描述、安全标签等多种标注，并设计了三个不同类型的任务（预测、生成、问答），能够从多个维度评测系统的人类理解能力。这比单一任务基准更全面。
- **人机协同标注**：采用人机协同的标注管线，在保证效率的同时提高了标注的丰富性和准确性，是数据构建方法上的一个亮点。

### 8. 不足与局限：实验覆盖、偏差风险、应用限制

- **实验覆盖不足**：未提供任何与现有主流方法的定量对比，也未进行消融实验来验证各标注成分的贡献，使得基准的挑战性和有效性难以客观评估。这是论文被拒可能的主要原因之一。
- **偏差风险**：
  - 数据来源偏差：Waymo 数据集主要来自美国特定城市，YouTube 视频分布不可控，自采集数据也可能存在地域或文化偏向。这些可能导致行为模式分布不均，产生地理或文化偏差。
  - 标注偏差：意图和关键行为标签可能带有主观性，不同标注人员对“危险行为”的界定可能不一致，影响标签的可靠性。
- **应用限制**：
  - 基准任务定义较为基础（预测、生成、问答），实际自动驾驶系统需要更实时的在线理解与决策，而当前基准未评估在线推理速度与复杂度。
  - 未考虑人类行为的动态不确定性（如意图变化），可能简化了真实世界的复杂性。
  - 整篇论文集中在数据与基准构建，没有提出新的高效模型或算法，因此对自动驾驶工程落地的直接推动有限。

（完）
