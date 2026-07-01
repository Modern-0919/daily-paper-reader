---
title: "GUI-ReWalk: Massive Data Generation for GUI Agent via Stochastic Exploration and Intent-Aware Reasoning"
title_zh: "GUI-ReWalk: 基于随机探索和意图感知推理的GUI智能体大规模数据生成"
authors: "Taoran Lu, Musen Lin, Minghao Liu, Lichen Yuan, Yiwei Liu, Haonan Xu, Miao yu, Yuhao Chao, Zhaojian Li"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=FcZqZH0j1b"
tags: ["query:agent-traj"]
score: 7.0
evidence: 合成真实多样的GUI智能体轨迹用于测试
tldr: 针对GUI智能体缺乏高质量轨迹数据的问题，该论文提出GUI-ReWalk框架，通过随机探索阶段模拟人类试错行为，结合意图感知推理，合成真实多样的GUI轨迹。该方法在保证多样性的同时覆盖有意义的任务。实验证明生成的数据可用于训练和评估GUI智能体。该工作为智能体轨迹测试提供了数据生成方法。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有GUI智能体轨迹数据获取成本高或多样性不足。
method: 提出多阶段框架，结合随机探索和意图感知推理合成轨迹。
result: 生成的轨迹在多样性和任务覆盖上均优于现有方法。
conclusion: GUI-ReWalk为GUI智能体轨迹测试提供了可扩展的数据生成方案。
---

## Abstract
Graphical User Interface (GUI) Agents, powered by large language and vision-language models, hold promise for enabling end-to-end automation in digital environments. However, their progress is fundamentally constrained by the scarcity of scalable, high-quality trajectory data. Existing data collection strategies either rely on costly and inconsistent manual annotations or on synthetic generation methods that trade off between diversity and meaningful task coverage. To bridge this gap, we present \textbf{GUI-ReWalk}: a reasoning-enhanced, multi-stage framework for synthesizing realistic and diverse GUI trajectories. GUI-ReWalk begins with a stochastic exploration phase that emulates human trial-and-error behaviors, and progressively transitions into a reasoning-guided phase where inferred goals drive coherent and purposeful interactions. Moreover, it supports multi-stride task generation, enabling the construction of long-horizon workflows across multiple applications. By combining randomness for diversity with goal-aware reasoning for structure, GUI-ReWalk produces data that better reflects the intent-aware, adaptive nature of human-computer interaction. We further train Qwen2.5-VL-7B on the GUI-ReWalk dataset and evaluate it across multiple benchmarks, including Screenspot-Pro, OSWorld-G, UI-Vision, AndroidControl, and GUI-Odyssey. Results demonstrate that GUI-ReWalk enables superior coverage of diverse interaction flows, higher trajectory entropy, and more realistic user intent. These findings establish GUI-ReWalk as a scalable and data-efficient framework for advancing GUI agent research and enabling robust real-world automation.

---

## 论文详细总结（自动生成）

# 详细论文总结：GUI-ReWalk

## 1. 核心问题与整体含义（研究动机与背景）
- **问题**：GUI智能体（基于大语言模型/视觉语言模型）在实现端到端数字自动化方面潜力巨大，但其发展受到**高质量、可扩展的轨迹数据**稀缺的严重制约。
- **现有瓶颈**：人工标注成本高且不一致；已有的合成数据方法要么牺牲多样性，要么缺乏有意义的任务覆盖。
- **本文目标**：提出一种能够同时保证**多样性和任务意图相关性**的大规模轨迹数据生成方法，以推动GUI智能体的训练与评估。

## 2. 提出的方法论
- **核心思想**：构建一个**多阶段推理增强框架（GUI-ReWalk）**，将随机探索与意图感知推理有机结合，模拟人类试错与目标导向的交互行为。
- **关键技术细节**：
  - **第一阶段：随机探索（Stochastic Exploration）**：模拟人类“试错”行为，在界面中自由点击、滑动等，以捕获多样化的交互模式。
  - **第二阶段：意图感知推理（Intent-Aware Reasoning）**：逐步引入基于推断目标的引导，使生成的轨迹具有连贯性和目的性，符合用户真实意图。
  - **多步任务生成（Multi-stride Task Generation）**：支持跨多个应用的长时程工作流构建，生成更复杂的真实场景轨迹。
- **流程概述**：初始阶段注重随机性以增加覆盖，随后“渐进过渡”至推理引导阶段，最终输出兼具多样性、结构性和意图感知的轨迹数据。

## 3. 实验设计
- **训练模型**：使用GUI-ReWalk生成的轨迹数据训练 **Qwen2.5-VL-7B**（7B参数的视觉语言模型）。
- **评估基准**：在以下5个公开基准上进行测评：
  - Screenspot-Pro
  - OSWorld-G
  - UI-Vision
  - AndroidControl
  - GUI-Odyssey
- **对比方法**：文中提及与“现有合成方法或人工标注方法”对比，但未明确列出具体对比模型或基线（如RPA-based、LLM-based等），仅声称在多样性和任务覆盖上优于现有方法。
- **评估指标**：交互流覆盖度、轨迹熵（衡量多样性）、用户意图真实性等。

## 4. 资源与算力
- **文中信息**：未明确计算资源（如GPU型号、数量、训练时长等）。
- **说明**：仅提及使用Qwen2.5-VL-7B进行训练，但未报告训练成本。读者需自行根据模型规模估算或联系作者获取。

## 5. 实验数量与充分性
- **实验组数**：
  - 在5个不同领域的基准上进行了测试，每个基准可能包含多个子任务或场景。
  - 采用单一模型（7B）进行训练和评估，未进行不同规模模型（如3B、14B等）的对比。
  - **消融实验**：文中未明确提及对框架各组件（随机探索、意图推理）的单独消融分析，因此组件贡献的量化证据不足。
- **充分性与客观性**：
  - 多基准覆盖了不同任务类型（屏幕定位、系统级操作、UI视觉理解、移动控制等），具有较好的泛化性。
  - 缺乏与现有最先进合成方法的公平对比（如基线方法未列明），声称“优于现有方法”需更具体证据。
  - 实验结论主要基于训练后智能体的性能，但对生成数据本身（如质量、噪声等）的统计分析尚不够细致。

## 6. 主要结论与发现
- GUI-ReWalk生成的轨迹数据**在多样性（更高轨迹熵）、任务覆盖度（更广的交互流）和用户意图真实性方面均显著优于现有方法**。
- 利用其数据训练的Qwen2.5-VL-7B在多个基准上表现良好，证明该方法**可扩展且数据高效**。
- 该框架能够模拟人类在GUI操作中的试错与目标导向两种模式，从而产生更具适应性的智能体行为。

## 7. 优点
- **方法设计亮点**：
  - 创新性地融合**随机探索**（保证多样性）与**意图推理**（保证任务相关性），打破了合成数据中“多样性 vs. 实用性”的权衡。
  - 支持**多应用长轨迹生成**，更贴近真实世界复杂任务。
  - 无需额外人工标注，可扩展性强。
- **实验亮点**：
  - 在多个不同领域的GUI基准（桌面、移动等）上进行验证，覆盖了主流评估场景。
  - 采用真实世界模型（Qwen2.5-VL）进行下游评测，增强了结果的说服力。

## 8. 不足与局限
- **实验覆盖局限**：
  - 仅使用7B规模模型进行训练，未探索其对不同规模（如3B/14B）的迁移效果或鲁棒性。
  - 未提供与现有SOTA合成数据方法（如自动规划、例程生成等）的详细对比表格。
  - 缺乏消融实验来量化随机探索和意图推理各自的贡献。
- **偏差风险**：
  - 生成的数据可能偏向特定类型的GUI环境（如Android、桌面Web），对其他环境（如iOS、嵌入式界面）的适用性未知。
  - 意图推理可能引入生成者的偏见，导致轨迹偏离真实用户分布。
- **应用限制**：
  - 框架依赖于对GUI环境的完整建模（如可交互元素、状态机），对于封闭或动态性极强的界面可能失效。
  - 未讨论数据生成的成本（时间、计算资源），可能影响实际部署的可复现性。
- **其他**：文中未明确对比方法的具体配置，使得“优于”结论的客观性需进一步验证；此外，未公开数据集或代码（需要确认OpenReview补充材料）。

（完）
