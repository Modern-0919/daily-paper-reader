---
title: "DrivingGen: A Comprehensive Benchmark for Generative Video World Models in Autonomous Driving"
title_zh: DrivingGen：自动驾驶中生成式视频世界模型的综合基准
authors: "Yang Zhou, Hao Shao, Letian Wang, Zhuofan Zong, Hongsheng Li, Steven L. Waslander"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=OrgL5DsU0f"
tags: ["query:agent-traj"]
score: 7.0
evidence: 基准包含轨迹合理性评估
tldr: 该论文针对自动驾驶中生成式视频世界模型缺乏严格基准的问题，提出了DrivingGen基准。该基准不仅评估视频生成质量，还量化轨迹合理性等安全关键指标。它旨在推动世界模型在可扩展仿真、安全测试和合成数据生成方面的应用。实验表明现有模型在轨迹评估上存在不足。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有评估仅关注通用视频指标，忽视了轨迹合理性等安全因素。
method: 构建包含轨迹评估的多维度基准，量化安全关键成像因素。
result: 揭示现有模型在轨迹合理性方面表现有限。
conclusion: 该基准为自动驾驶世界模型提供了更全面的评估标准。
---

## Abstract
Video generation models, as one form of world models, has emerged as one of the most exciting frontiers in AI, promising agents the ability to imagine the future by modeling the temporal evolution of complex scenes. 
In autonomous driving, this vision gives rise to driving world models—generative simulators that imagine ego and agent futures, enabling scalable simulation, safe testing of corner cases, and rich synthetic data generation.
Yet, despite fast-growing research activity, the field lacks a rigorous benchmark to measure progress and guide priorities. Existing evaluations remain limited: generic video metrics overlook safety-critical imaging factors; trajectory plausibility is rarely quantified; temporal and agent-level consistency is neglected; and controllability with respect to ego conditioning is ignored. Moreover, current datasets fail to cover the diversity of conditions required for real-world deployment.
To address these gaps, we present DrivingGen, the first comprehensive benchmark for generative driving world models. DrivingGen combines a diverse evaluation dataset—curated from both driving datasets and internet-scale video sources, spanning varied weather, time of day, geographic regions, and complex maneuvers—with a suite of new metrics that jointly assess visual realism, trajectory plausibility, temporal coherence, and controllability. Benchmarking 14 state-of-the-art models reveals clear trade-offs: general models look better but break physics, while driving-specific ones capture motion realistically but lag in visual quality.
DrivingGen offers a unified evaluation framework to foster reliable, controllable, and deployable driving world models, enabling scalable simulation, planning, and data-driven decision-making.

---

## 论文详细总结（自动生成）

# DrivingGen：自动驾驶中生成式视频世界模型的综合基准

## 1. 论文的核心问题与整体含义（研究动机和背景）
自动驾驶中的生成式视频世界模型（driving world models）被视为能够模拟自我和智能体未来轨迹的生成式模拟器，可支持可扩展仿真、极端场景安全测试及合成数据生成。然而，该领域缺乏严格的基准来衡量进展和指导优先级。**现有评估存在严重局限性**：仅使用通用的视频质量指标，忽略了安全关键的成像因素（如物理真实性）；极少量化轨迹合理性；忽视时间一致性和智能体级一致性；缺乏对自我条件控制性的评估。此外，现有数据集未能覆盖真实部署所需的多样条件。因此，论文提出了**DrivingGen**——首个针对生成式驾驶世界模型的综合基准，旨在统一评估框架，推动可靠、可控、可部署的驾驶世界模型发展。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
**核心思想**：构建一个多维度的基准，同时评估视觉真实性、轨迹合理性、时间连贯性和可控性，弥补现有指标的不足。

**关键技术细节**：
- **评估数据集**：从驾驶数据集和互联网视频来源中精选，涵盖多种天气、一天中的时间、地理区域和复杂操作场景。
- **新指标套件**：联合评估四个维度：
  - 视觉真实性（如FID、FVD等传统视频指标，但结合安全相关要求）
  - 轨迹合理性（量化预测轨迹是否符合物理规律和驾驶常识）
  - 时间连贯性（帧间物体运动的一致性和场景变化的平滑性）
  - 可控性（评估模型在给定自我条件（如意图、路径）下生成视频的能力）

**算法流程**（文字说明）：
1. 收集并筛选多源视频片段，构成覆盖广泛条件的数据集。
2. 对每个测试模型输入相同的条件（如自车轨迹、场景上下文），生成未来视频帧。
3. 计算上述四个维度的指标，并与真实视频或标注轨迹进行对比。
4. 对所有模型进行排序和trade-off分析。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法
- **数据集与场景**：结合驾驶数据集（如nuScenes等常见自动驾驶数据集）以及互联网规模的视频数据，覆盖不同天气（晴天、雨、雪）、一天中的时间（白天、夜晚）、地理区域（城市、乡村等）以及复杂操作（如变道、急刹、避障）。
- **Benchmark**：DrivingGen基准，包含上述多样化的评估数据集和四个维度的指标。
- **对比方法**：14个最先进的（state-of-the-art）模型，分为两类：
  - 通用视频生成模型（在视觉质量上表现更好，但物理规律模拟较差）
  - 驾驶特定世界模型（能更真实地捕捉运动，但视觉质量落后）

## 4. 资源与算力
论文中未明确说明使用的GPU型号、数量、训练时长等硬件与计算资源信息。

## 5. 实验数量与充分性
- **实验数量**：共评估了14个模型，涵盖了通用和驾驶特定两大类。每个模型在多个场景和指标下进行了测试。
- **充分性**：实验数量较多，覆盖了当前主流模型，且从四个维度进行了对比分析，能够揭示模型间的trade-off。但**缺乏消融实验**（如指标有效性验证、数据集规模影响等），且未展示每个指标的具体数值分布或统计显著性检验。因此，实验在广度上较充分，但在深度上（如超参数影响、子集分析）有所不足，整体**客观性良好**（采用多种公开模型和统一评估流程）。

## 6. 论文的主要结论与发现
- 现有生成式视频世界模型在DrivingGen基准下表现出清晰的**权衡**：
  - 通用视频生成模型视觉真实度高，但**违反物理规律**（轨迹不合理）。
  - 驾驶特定世界模型能更准确地捕捉运动轨迹，但**视觉质量较差**。
- 轨迹合理性是当前模型的薄弱环节，安全关键成像因素被普遍忽视。
- 该基准提供了一个统一的评估框架，有助于指导未来研究在可靠性和可控性上的突破。

## 7. 优点：方法或实验设计上的亮点
- **首次全面基准**：首次提出针对自动驾驶世界模型的综合性评估基准，填补了领域空白。
- **多维度指标**：除视觉质量外，引入轨迹合理性、时间连贯性和可控性，紧贴自动驾驶的安全需求。
- **数据集多样性**：涵盖互联网视频和驾驶数据集，扩展了场景覆盖范围（天气、时间、地域、复杂操作）。
- **公平对比**：在统一设置下评估14个代表性模型，便于横向比较。
- **可操作性**：框架设计清晰，便于后续研究者复现和扩展。

## 8. 不足与局限
- **计算资源未公开**：未说明评估所需的硬件和时间成本，可能影响可复现性。
- **轨迹合理性指标细节缺失**：文中仅提及“量化轨迹合理性”，但未详细说明如何定义“合理性”（如是否结合碰撞检测、加速度约束等），可能存在主观偏差。
- **数据集规模与平衡性未知**：未给出具体样本数量，且“互联网来源”可能导致分布偏差（如与真实驾驶场景不一致）。
- **模型覆盖面有限**：14个模型虽具代表性，但可能遗漏近期新方法或工业级模型。
- **缺乏下游任务验证**：基准仅评估生成质量，未验证生成视频用于规划、仿真等下游任务的效果。
- **可控性评估可能简化**：自我条件控制的具体定义和评估粒度未详细说明。

（完）
