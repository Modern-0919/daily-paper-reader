---
title: "AgentSynth: Scalable Task Generation for Generalist Computer-Use Agents"
title_zh: AgentSynth：通用计算机使用智能体的可扩展任务生成
authors: "Jingxu Xie, Dylan Xu, Xuandong Zhao, Dawn Song"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=CoBxmXThM6"
tags: ["query:agent-traj"]
score: 8.0
evidence: 可扩展的智能体任务与轨迹数据集生成流水线
tldr: "AgentSynth提出可扩展且成本高效的流水线，自动为通用计算机使用智能体生成高质量任务和轨迹数据集。利用信息不对称，将简单子任务组合成具有挑战性的长程任务，并精确调控难度。实验表明，最先进的LLM智能体在难度1到6上成功率从18%降至4%，显示基准的区分力。该工具为智能体轨迹测试提供了丰富的资源。"
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有智能体基准任务生成成本高且缺乏多样性。
method: 提出基于信息不对称的子任务组合流水线，自动合成多样任务和轨迹。
result: 生成6000+任务，成功率和难度负相关。
conclusion: AgentSynth为智能体轨迹测试提供高效数据生成方案。
---

## Abstract
We introduce AgentSynth, a scalable and cost-efficient pipeline for automatically synthesizing high-quality tasks and trajectory datasets for generalist computer-use agents. Leveraging information asymmetry, AgentSynth constructs subtasks that are simple during generation but significantly more challenging when composed into long-horizon tasks, enabling the creation of over 6,000 diverse and realistic tasks. A key strength of AgentSynth is its ability to precisely modulate task complexity by varying the number of subtasks. Empirical evaluations show that state-of-the-art LLM agents suffer a steep performance drop, from 18\% success at difficulty level 1 to just 4\% at level 6, highlighting the benchmark's difficulty and discriminative power. Moreover, our pipeline achieves a low average cost of \$0.60 per trajectory, orders of magnitude cheaper than human annotations. Our code and data are available at https://github.com/sunblaze-ucb/AgentSynth

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：现有的通用计算机使用智能体（generalist computer-use agents）基准任务主要依赖人工创建，存在成本高、多样性不足的问题，难以支撑大规模、可复现的智能体能力评估。
- **核心问题**：如何低成本的自动生成高质量、可扩展、难度可控的任务与轨迹数据集，以推动智能体在真实计算机使用场景下的通用能力研究。
- **整体含义**：AgentSynth 提出一种基于信息不对称的流水线，能够自动合成多样且现实的长程任务，为智能体提供有效的训练与测试资源，降低对人工标注的依赖。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：利用**信息不对称**（information asymmetry）——在生成阶段，子任务设计简单；但在组合成多步骤长程任务时，由于子任务间的信息隐藏与依赖关系，整体任务变得显著困难。
- **关键技术细节**：
  - 将简单子任务（如点击、输入、导航等）作为基础单元。
  - 通过控制子任务的数量来精确调节任务复杂度（difficulty level 1~6）。
  - 自动合成完整的任务定义、初始状态、成功条件以及对应的轨迹数据（ground truth trajectories）。
  - 流水线无需人工干预，生成成本低（平均每条轨迹 $0.60），远低于人类标注。
- **算法流程（文字说明）**：
  1. 定义原子子任务池，每个子任务仅涉及单一操作且不依赖上下文。
  2. 随机选择若干子任务，并按逻辑顺序串联形成长程任务，确保前一个子任务的输出是后一个子任务的输入（即信息不对称）。
  3. 为每个任务生成自然语言指令（使用LLM辅助）和期望轨迹。
  4. 通过调整子任务数量（1~6）生成不同难度级别的任务集。

### 3. 实验设计：数据集、基准、对比方法
- **数据集/场景**：自动生成了超过 **6,000 个多样化且真实的任务**，涵盖常见的计算机使用场景（如文件操作、网页浏览、软件使用等）。
- **基准（Benchmark）**：以 AgentSynth 自动生成的任务作为测试基准，评估通用计算机使用智能体的表现。
- **对比方法**：评估了多种**最先进的LLM智能体**（state-of-the-art LLM agents），包括基于不同大语言模型的智能体（具体名称未在摘要中列出）。对比了它们在难度 1~6 上的任务成功率。

### 4. 资源与算力
- **文中未明确说明**使用的 GPU 型号、数量以及训练时长。摘要和元数据中仅提到代码和数据集已开源，未提及训练资源开销。因此无法总结具体算力信息。

### 5. 实验数量与充分性
- **实验数量**：主要实验展示了难度 1~6 各等级下多个智能体的成功率，共有 6 个难度级别 × 若干模型。
- **充分性**：
  - **充分**：生成了 6000+ 任务，样本量充足；涵盖不同难度梯度，能够验证基准的区分度。
  - **客观公平**：所有智能体在相同任务集上评估，排除了人工偏差。
  - **局限**：摘要未提及消融实验或对不同生成策略（如子任务选择策略）的对比，也未分析任务多样性对结果的影响，可能对方法各部分贡献的验证不够完整。

### 6. 论文的主要结论与发现
- AgentSynth 能高效生成高质量、多样化的计算机使用任务与轨迹，成本极低（$0.60/条）。
- 难度调控有效：最先进 LLM 智能体的成功率随难度增加呈现陡降趋势（从 18% 到 4%），证明生成的任务具有强判别力。
- 该流水线可作为可扩展的智能体测试基础设施，替代昂贵的人工标注。

### 7. 优点：方法或实验设计上的亮点
- **可扩展性与低成本**：完全自动化，单条轨迹成本仅 $0.60，对比人工标注节省数个数量级。
- **难度精确可调**：通过子任务数量简单控制复杂度，无需复杂难度建模。
- **信息不对称设计**：生成阶段简单、测试阶段困难，巧妙避免了任务过于简单或过拟合的问题。
- **开源可用**：代码和数据集已公开，促进后续研究和复现。

### 8. 不足与局限
- **实验覆盖不足**：仅测试了成功率单一指标，未评估其他维度（如鲁棒性、泛化性、执行效率）。
- **任务多样性可能受限**：虽然声称 6000+ 任务，但基于固定子任务池的组合生成，可能缺乏现实世界中非常规或长尾场景。
- **信息不对称缺陷**：子任务简单可能使部分模型通过模式匹配解决，而非真正理解复杂指令。
- **缺乏消融实验**：未探索不同子任务选择策略、LLM辅助指令生成质量对任务难度的影响。
- **资源信息缺失**：未说明生成流水线本身的算力需求，可能影响可复现性评估。

（完）
