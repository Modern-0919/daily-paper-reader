---
title: "RealEngine: Simulating Autonomous Driving in Realistic Context"
title_zh: RealEngine：在真实环境中模拟自动驾驶
authors: "Junzhe Jiang, Nan Song, jingyu li, Xiatian Zhu, Li Zhang"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=bXRrwS4ViR"
tags: ["query:agent-traj"]
score: 7.0
evidence: 自动驾驶模拟，支持闭环轨迹行为评估
tldr: 自动驾驶代理的可靠性依赖高质量仿真环境。本文提出RealEngine，集成多模态传感、真实场景渲染、闭环轨迹评估和多样交通场景，支持多代理协作。该仿真器满足自由形式轨迹行为评估需求，为自动驾驶代理的轨迹测试提供全面平台。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有驾驶模拟器不能满足多模态感知、闭环评估和场景多样性等关键要求。
method: RealEngine集成多模态传感、真实场景渲染、闭环评估和多样交通场景，用于自动驾驶代理测试。
result: 在多个驾驶任务上验证，能支持自由形式轨迹行为和交互动力学评估。
conclusion: RealEngine为自动驾驶代理轨迹测试提供高质量仿真环境。
---

## Abstract
Driving simulation plays a crucial role in developing reliable driving agents by providing controlled, evaluative environments. To enable meaningful assessments, a high-quality driving simulator must satisfy several key requirements: multi-modal sensing capabilities (e.g., camera and LiDAR) with realistic scene rendering to minimize observational discrepancies; closed-loop evaluation to support free-form trajectory behaviors; highly diverse traffic scenarios for thorough evaluation; multi-agent cooperation to capture interaction dynamics; and high computational efficiency to ensure affordability and scalability. However, existing simulators and benchmarks fail to comprehensively meet these fundamental criteria. To bridge this gap, this paper introduces RealEngine, a novel driving simulation framework that holistically integrates 3D scene reconstruction and novel view synthesis techniques to achieve realistic and flexible closed-loop simulation in the driving context. By leveraging real-world multi-modal sensor data, RealEngine reconstructs background scenes and foreground traffic participants separately, allowing for highly diverse and realistic traffic scenarios through flexible scene composition. This synergistic fusion of scene reconstruction and view synthesis enables photorealistic rendering across multiple sensor modalities, ensuring both perceptual fidelity and geometric accuracy. Building upon this environment, RealEngine supports three essential driving simulation categories: non-reactive simulation, safety testing, and multi-agent interaction, collectively forming a reliable and comprehensive benchmark for evaluating the real-world performance of driving agents.

---

## 论文详细总结（自动生成）

由于提供的论文内容仅包含摘要及元数据，缺乏完整的方法细节、实验设置和数据集信息，以下总结严格基于已有内容进行分析。

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：现有自动驾驶模拟器无法同时满足**多模态感知**（如相机、LiDAR）、**真实场景渲染**、**闭环评估**（支持自由形式轨迹）、**场景多样性**、**多智能体协作**以及**高计算效率**等关键需求，导致对自动驾驶代理的可靠性评估存在偏差。
- **背景意义**：高质量的仿真环境是发展可靠驾驶代理的基础，能够提供受控且可复现的评估条件。RealEngine旨在填补现有模拟器无法全面满足这些基本标准的空白。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：通过**3D场景重建**与**新视图合成**技术的有机融合，实现真实、灵活的闭环仿真。将背景场景与前景交通参与者分别重建，通过灵活的场景组合生成高度多样、真实的交通场景。
- **关键技术细节**：
  - 利用真实世界多模态传感器数据（相机、LiDAR）进行重建。
  - **前景/背景分离重建**：分别重建静态背景和动态交通参与者，提升场景组合的灵活性。
  - **多模态逼真渲染**：结合场景重建与视图合成，实现跨传感器模态（图像、点云）的光照逼真渲染，同时保证感知保真度和几何精度。
- **算法流程**（文字描述）：
  1. 输入真实多模态传感数据。
  2. 分别重建背景场景（静态环境）和前景对象（车辆、行人等动态参与者）。
  3. 通过灵活组合生成多样化交通场景。
  4. 基于生成的环境，提供三种仿真模式（非响应式仿真、安全测试、多智能体交互）。
- **公式或伪代码**：未在摘要中提供，不可得。

### 3. 实验设计：数据集、Benchmark、对比方法
- **数据集**：未明确提及具体数据集名称。仅说明使用“真实世界多模态传感器数据”。
- **Benchmark**：论文将RealEngine本身定位为一个“可靠且全面的基准”，用于评估驾驶代理在真实世界场景中的表现。
- **对比方法**：未提及任何对比基线或已有模拟器（如CARLA、MetaDrive等）。因此无法判断其比较的客观性。

### 4. 资源与算力
- **未明确说明**：摘要及元数据中未提及GPU型号、数量、训练时长等算力信息。需指出该部分缺失。

### 5. 实验数量与充分性
- **实验数量**：仅提到“在多个驾驶任务上验证”（来自元数据），未提供具体任务类型、消融实验或统计数值。
- **充分性评估**：
  - **不足**：缺乏定量结果、误差分析、与现有方法的对比实验，无法独立判断方法有效性。
  - **客观性**：未公布实验设置细节，存在主观偏倚风险。由于缺少公开基准对比，其公平性难以保证。

### 6. 论文的主要结论与发现
- RealEngine能够支持自由形式的轨迹行为评估和交互动力学分析。
- 该仿真器为自动驾驶代理的轨迹测试提供了高质量的仿真环境。
- 通过集成分离重建和视图合成，实现了多模态逼真渲染与闭环评估，填补了现有模拟器的不足。

### 7. 优点
- **全面覆盖关键需求**：同时整合多模态感知、真实渲染、闭环评估、场景多样性和多智能体协作。
- **创新技术融合**：将3D重建与新视图合成结合用于驾驶仿真，避免了传统基于游戏引擎的渲染与真实传感器数据的分布差异。
- **灵活的场景组合**：通过前景/背景分离重建，可柔性生成多种交通场景，提高测试覆盖度。
- **高计算效率**：文中提到“高计算效率”作为设计目标，但未提供具体数值。

### 8. 不足与局限
- **实验证据薄弱**：仅靠“多个驾驶任务验证”等模糊表述，缺乏量化指标和可复现的实验对比。
- **数据集未公开**：未说明使用的真实数据来源，限制了同行验证和推广。
- **缺乏与现有模拟器（如CARLA、SUMO、MetaDrive）的定量比较**：无法证明其实际性能优越性。
- **技术细节缺失**：未提供任何公式、网络架构、损失函数或算法伪代码，导致方法可复现性低。
- **应用限制**：只提及三种仿真类别（非响应式、安全测试、多智能体交互），未讨论极端天气、传感器失效等边缘场景的处理能力。

（完）
