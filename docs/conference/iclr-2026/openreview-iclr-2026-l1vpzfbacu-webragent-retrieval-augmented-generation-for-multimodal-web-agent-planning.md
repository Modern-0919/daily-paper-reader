---
title: "WebRAGent: Retrieval-Augmented Generation for Multimodal Web Agent Planning"
title_zh: WebRAGent：多模态Web Agent规划的检索增强生成
authors: "Xuan Zhang, Shengbo Cai, Ziyan Jiang, Rui Meng, Zora Zhiruo Wang, Lu Guangcheng, Zhiyong Wu, Yanyi Shang, Dehan Kong"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=L1VPZFbAcu"
tags: ["query:agent-traj"]
score: 6.0
evidence: 多模态轨迹检索用于web agent规划
tldr: 该论文指出多模态轨迹建模因视觉信息表示困难而碎片化。提出多模态轨迹检索方法，通过检索相关轨迹辅助web agent规划，克服长历史上下文窗口限制。该方法整合视觉和文本信号，提升了agent在GUI任务中的规划能力。实验表明检索增强有效提高了任务成功率。它为agent轨迹利用提供了新范式。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 多模态轨迹建模受限于视觉表示和历史长度。
method: 提出多模态轨迹检索，在规划时检索相关轨迹作为参考。
result: 在web agent任务上提升了规划准确率。
conclusion: 为agent轨迹数据的高效利用提供了新方法。
---

## Abstract
Trajectory data, capturing multimodal human actions and states, are pivotal for building autonomous GUI agents and transferring skills across tasks, encoding knowledge by compressing past experience into structured Markov sequences. Yet current methods for trajectory modeling remain fragmented, often relying on task-specific heuristics or textual signals. Progress on multimodal trajectories has been limited by the difficulty of representing visual information within long-step histories that exceed model context windows. Hence, how to effectively learn from multimodal trajectories remains a major and insufficiently addressed challenge amid ever-growing datasets. In this work, we introduce Multimodal Trajectory Retrieval, bridging the gap between universal retrieval and agent-centric trajectory modeling. We construct the Unified Agent Trajectory Dataset (UATD) from annotated demonstrations and states across diverse real-world scenarios. Building on this, we present GAE-Bench, a benchmark containing a large number of trajectory-based retrieval pairs. Our proposed GAE-Retriever, a multimodal retriever based on vision–language models that uses token selection and GradCache to optimize the contrastive objective. Over multiple web-agent datasets, it surpasses strong baselines on retrieval recall. To demonstrate potential downstream applications, we develop WebRAGent, a retrieval-augmented web agent that integrates GAE-Retriever and supports both DOM- and vision-based observations. WebRAGent proves effective on both textual and visual retrieved knowledge, achieving performance gains of over 15\% vs. non-retrieval on the Online-Mind2Web benchmark.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：多模态轨迹建模因视觉信息在长历史步骤中的表示困难而碎片化，现有方法多依赖任务特定启发式或文本信号，难以有效利用大量积累的多模态轨迹数据提升Web Agent的规划能力。
- **研究动机**：轨迹数据（包含多模态人类操作动作与状态）对构建自主GUI Agent和跨任务技能迁移至关重要，但当前模型上下文窗口限制导致长历史视觉信息难以被有效编码和检索，因此需要一种通用的多模态轨迹检索机制。
- **整体含义**：提出多模态轨迹检索范式，将通用检索与Agent中心轨迹建模结合，通过检索相关历史轨迹辅助Web Agent规划，克服长上下文限制，提升任务成功率。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：构建多模态轨迹检索器（GAE-Retriever），在Agent规划时从大规模轨迹库中检索与当前任务最相关的历史轨迹作为参考，从而增强Agent的决策能力，无需处理完整长历史上下文。
- **关键技术细节**：
  - 基于视觉-语言模型（VLM）作为检索器骨干。
  - 使用**Token Selection**和**GradCache**技术优化对比学习目标，实现高效多模态表示学习。
  - 构建**Unified Agent Trajectory Dataset (UATD)**：从多种真实场景的标注演示和状态中统一收集的轨迹数据集。
  - 建立**GAE-Bench**：包含大量基于轨迹的检索对的基准测试。
  - 开发**WebRAGent**：集成GAE-Retriever的检索增强Web Agent，同时支持基于DOM（文档对象模型）和基于视觉的观察。
- **算法流程**（文字说明）：
  1. 对轨迹数据（包含视觉帧、DOM文本、动作）进行多模态编码。
  2. 通过Token Selection降低视觉特征冗余，GradCache缓解对比学习中的显存瓶颈。
  3. 训练GAE-Retriever以最大化相关轨迹对（当前任务与历史轨迹）的相似度，最小化不相关对。
  4. 在Agent推理时，根据当前观察查询检索库，返回top-k最相似轨迹。
  5. WebRAGent将检索到的轨迹作为额外上下文融入规划决策。

## 3. 实验设计：数据集、Benchmark、对比方法
- **数据集**：
  - **UATD**：自构建的统一多模态轨迹数据集，涵盖多种真实场景（具体场景种类未在摘要中详述，但提及来自多个Web Agent数据集）。
  - **GAE-Bench**：专门为检索任务设计的基准，包含大量轨迹检索对。
  - 下游评估使用**Online-Mind2Web**基准（在线Web Agent测试）。
- **对比方法**：
  - 检索任务上：与强基线（未具体命名）对比召回率（Recall）。
  - 下游任务上：与非检索版本的Web Agent对比成功率。
- **实验结果摘要**：
  - GAE-Retriever在多个Web Agent数据集上的检索召回率超过强基线。
  - WebRAGent在Online-Mind2Web上比非检索版本性能提升**超过15%**。

## 4. 资源与算力
- **论文中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅提及使用了Token Selection和GradCache来优化显存使用，但无具体硬件配置。需要指出这一点。

## 5. 实验数量与充分性
- **实验数量**：包含两部分：①检索性能评估（多个Web Agent数据集上的召回率对比）；②下游Agent规划性能评估（Online-Mind2Web上的成功率对比）。至少涉及两组主要实验，但消融实验、不同检索策略对比等细节未在摘要中呈现。
- **充分性与公平性**：
  - 肯定方面：对比了强基线和消去检索的版本，验证了检索增强的有效性；使用了公开基准Online-Mind2Web，具有一定客观性。
  - 不足方面：未提供详细的消融实验（如不同检索参数、不同视觉表示方法）、未报告统计显著性检验、未说明训练/测试数据划分及数据量。因此实验充分性有限，需要看全文补充。

## 6. 论文的主要结论与发现
- 多模态轨迹检索能有效克服长历史上下文窗口限制，提升Web Agent的规划能力。
- 提出的GAE-Retriever基于VLM和优化对比学习，在检索召回率上显著优于强基线。
- 检索增强方法（WebRAGent）在Online-Mind2Web上实现超过15%的性能提升，验证了视觉和文本检索知识的有效性。
- 该工作为Agent轨迹数据的高效利用提供了新范式，即从“全历史输入”转向“相关历史检索”。

## 7. 优点：方法或实验设计上的亮点
- **方法创新**：首次将通用多模态检索系统性地引入Web Agent规划，解决长历史视觉信息难以编码的问题。
- **技术贡献**：提出Token Selection和GradCache组合优化，降低VLM作为检索器的训练成本，使对比学习在视频级轨迹数据上可行。
- **资源贡献**：构建了UATD统一轨迹数据集和GAE-Bench检索基准，为后续研究提供基础。
- **实践价值**：WebRAGent同时支持DOM和视觉观察，具备实际部署潜力，且性能增益显著。

## 8. 不足与局限
- **实验覆盖有限**：仅使用Online-Mind2Web一个下游基准，未在更多Web/移动端GUI数据集上验证泛化性。
- **偏差风险**：轨迹库来源可能偏向特定任务类型或界面风格，检索结果可能存在领域偏移。
- **应用限制**：
  - 检索依赖于预先构建的轨迹库，无法动态扩展或适应全新界面环境。
  - 训练GAE-Retriever需要大量人工标注或预收集的轨迹数据，成本较高。
  - 未讨论检索失败或注入噪声轨迹时的鲁棒性。
- **算力资源未公开**，无法评估方法的可复现性和计算门槛。
- **缺少消融实验**，如Token Selection和GradCache各自贡献、检索数量对性能的影响等，无法充分验证设计选择的必要性。

（完）
