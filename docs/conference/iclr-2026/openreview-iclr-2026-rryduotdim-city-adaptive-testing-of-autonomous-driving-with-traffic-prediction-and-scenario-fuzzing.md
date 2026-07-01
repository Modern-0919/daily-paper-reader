---
title: City-Adaptive Testing of Autonomous Driving with Traffic Prediction and Scenario Fuzzing
title_zh: 城市自适应测试：结合交通预测与场景模糊测试的自动驾驶测试
authors: "xudong zhang, SHUAI LIU, Ning Cao, Zhiyang Zhou, Siwei Wei, Yan Cai, Long Zhang"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=RRyduOtDim"
tags: ["query:agent-traj"]
score: 6.0
evidence: 城市自适应测试自动驾驶系统行为
tldr: 该论文提出城市自适应测试框架，用于评估自动驾驶系统在复杂城市环境中的鲁棒性。方法集成时空交通预测（T-DDSTGCN）和多智能体行为建模，通过场景模糊测试生成贴合特定城市交通模式的压力测试用例。实验在METR-LA和PEMS-BAY数据集上展示了预测模型的最优性能，并验证了测试框架能有效暴露系统的城市特定缺陷。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 通用自动驾驶测试无法捕捉城市特有的交通模式。
method: 提出城市自适应测试框架，融合交通预测与多智能体场景模糊测试。
result: 有效的城市特定缺陷检测。
conclusion: 自适应测试对提升自动驾驶泛化性至关重要。
---

## Abstract
Autonomous Driving Systems (ADS) often struggle in complex urban environments because generic testing fails to capture city-specific traffic patterns and behaviors. To address this, we propose a city-adaptive testing framework that systematically evaluates ADS robustness by integrating spatiotemporal traffic prediction and multi-agent behavioral modeling. Our approach first introduces a novel traffic prediction model, called T-DDSTGCN, which combines graph and hypergraph representations to accurately forecast segment-level traffic speed and intersection turning probabilities. It achieves the best performance on both METR-LA and PEMS-BAY datasets, demonstrating its superior ability to capture spatiotemporal dependencies in traffic prediction tasks. Based on the predicted urban traffic flow, we construct diverse simulation scenarios enriched by a behavioral modeling framework called Primary Other Participants (POP), which simulates realistic motorcycle behavior using Level-K game theory and Social Value Orientation. To enhance scenario diversity, we further apply structured perturbations across traffic density, weather, and agent interactions. Our methodology is validated across 180 real-world urban scenarios on three industrial-scale simulation platforms, yielding 662 critical collision cases after multiple rounds of testing. We have conducted an initial manual screening of the 662 simulated accident scenarios, finding that 88.1\% of these accidents closely resemble real-world accident videos and reports. Furthermore, ablation studies highlight the critical role of human-like agent behavior in exposing ADS failures. Our findings suggest that incorporating traffic context and behavioral diversity into simulation testing is crucial for ensuring ADS safety and robustness in real-world deployments.

---

## 论文详细总结（自动生成）

# 中文详细总结：城市自适应测试结合交通预测与场景模糊测试的自动驾驶测试

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：自动驾驶系统（ADS）在复杂城市环境中表现不佳，现有通用测试方法无法捕捉城市特有的交通模式和行为，导致测试覆盖不足、鲁棒性评估不准确。
- **核心问题**：如何针对特定城市交通流特征，系统性地评估ADS在真实世界部署中的安全性。
- **整体含义**：提出城市自适应测试框架，通过融合时空交通预测与多智能体行为建模，生成贴合特定城市交通模式的压力测试场景，以暴露ADS的城市特定缺陷，提升泛化性。

## 2. 论文提出的方法论：核心思想、关键技术细节与算法流程

- **核心思想**：将交通预测与场景模糊测试结合，先预测城市级交通流（速度、转向概率），再基于预测结果构建多智能体行为模型，最后通过结构化扰动生成多样化测试场景。
- **关键技术细节**：
  - **交通预测模型 T-DDSTGCN**：结合图卷积与超图表示，预测路段级交通速度与交叉口转向概率。采用双重动态时空图卷积网络（DDSTGCN）变体，融合图与超图结构，捕获复杂时空依赖。
  - **多智能体行为建模 POP（Primary Other Participants）**：基于 Level-K 博弈论和社会价值取向（SVO）模拟逼真的摩托车行为，使仿真代理更贴近真实交通参与者。
  - **场景模糊测试**：在交通密度、天气、智能体交互三个维度施加结构化扰动，生成压力测试用例。
- **算法流程（文字描述）**：
  1. 输入目标城市的历史交通数据，训练 T-DDSTGCN 预测未来交通流（速度、转向概率）。
  2. 利用预测结果配置仿真环境中的初始交通流参数（车辆密度、速度分布、转向行为）。
  3. 将 POP 行为模型嵌入仿真平台，控制其他交通参与者的决策（尤其是摩托车）。
  4. 在预设基础场景上迭代应用模糊扰动（密度、天气、交互策略的组合），生成大量测试用例。
  5. 运行 ADS 在仿真平台上完成驾驶任务，记录碰撞等失败事件。
  6. 对检测到的碰撞进行人工筛选，与实际事故视频/报告对比验证逼真度。

## 3. 实验设计：数据集、场景、基准与对比方法

- **数据集**：
  - **METR-LA**（洛杉矶高速公路交通速度数据）和 **PEMS-BAY**（湾区高速公路交通速度数据），用于训练和评估交通预测模型 T-DDSTGCN。
- **场景**：180 个真实世界城市场景，涵盖多个城市（具体城市名未提及）。在三个工业级仿真平台上进行多轮测试，共生成 662 个关键碰撞案例。
- **Benchmark**：无明确指定外部基准任务，但交通预测部分对比了其他时间序列预测方法（未列出具体名称，仅声称 T-DDSTGCN 在 METR-LA 和 PEMS-BAY 上取得最优性能）。
- **对比方法**：文案仅提到“ablation studies”强调了类人智能体行为的关键作用，未列出具体对比模型。可以推测对比了去掉 POP 或模糊扰动的消融变体。

## 4. 资源与算力

- 文中**未明确说明**所使用的 GPU 型号、数量、训练时长等具体算力资源。仅提及在三个工业级仿真平台上进行验证，但未提供硬件细节。

## 5. 实验数量与充分性

- **实验数量**：交通预测在两个标准数据集（METR-LA、PEMS-BAY）上评估；场景测试使用了 180 个真实城市场景，多轮测试产出 662 个碰撞案例。此外还进行了消融实验以验证 POP 模型的效果。
- **充分性与公平性**：
  - **优点**：数据集选择具有代表性（高速/城市路网），场景数量较丰富（180 个），且碰撞案例经过人工筛选验证逼真度（88.1% 与实际事故相似）。
  - **不足**：消融实验仅提及 POP 部分，未对其他模块（如 T-DDSTGCN 与简化预测器的对比、不同模糊策略的独立影响）进行系统消融；实验未与现有的自动驾驶测试方法（如生成对抗网络、搜索方法）进行直接比较，因此公平性证据不足。此外，期刊投稿状态为 ICLR-2026-Rejected-Public，说明评审可能认为实验设计存在缺陷。

## 6. 论文的主要结论与发现

- T-DDSTGCN 在交通预测任务上优于现有方法，能够精确捕捉城市级时空依赖。
- 城市自适应测试框架有效暴露了 ADS 的城市特定缺陷，共发现 662 个碰撞案例，其中 88.1% 与真实世界事故特征吻合。
- 类人智能体行为（基于 Level-K 和 SVO 的 POP 模型）是导致 ADS 失败的关键因素，消融实验证明其不可或缺。
- 结合交通预测与行为多样性对于提升 ADS 在实际部署中的安全鲁棒性至关重要。

## 7. 优点

- **方法论创新**：将交通预测（图/超图时空建模）与多智能体博弈论行为模型结合，形成城市定制的测试生成思路。
- **逼真度验证**：人工对比仿真碰撞与真实事故录像/报告，提供高比例（88.1%）的相似性证据，增强了场景的真实性可信度。
- **多平台验证**：在三个工业级仿真平台上进行实验，增加了结果的泛化性。
- **场景多样性**：通过结构化扰动（密度、天气、交互）覆盖多个关键维度，提升了测试的全面性。

## 8. 不足与局限

- **实验覆盖不全**：仅针对高速公路和城市交通数据集（METR-LA、PEMS-BAY），未覆盖非典型路网（如乡村、山地区域）；城市场景仅 180 个，可能不足以代表所有复杂工况。
- **偏差风险**：T-DDSTGCN 在标准数据集上的比较未提供具体指标和 baseline 列表；性能声称缺乏可复现的细节。
- **算力信息缺失**：无法评估训练成本及是否可复现。
- **应用限制**：框架依赖事先预测交通流，对于实时性要求极高的在线测试或突发事件适应性不足；仅考虑了摩托车、轿车等少数交通参与者，未覆盖行人、自行车等其他元素。
- **评估恶意性**：碰撞案例仅定性匹配真实事故，缺乏定量评估（如误报率、漏报率），且否定结果（未发生碰撞的场景）未被分析。
- **被拒现实**：论文在 ICLR-2026 被拒，可能表明评审认为方法新颖性、实验严谨性或写作质量尚有提升空间。

（完）
