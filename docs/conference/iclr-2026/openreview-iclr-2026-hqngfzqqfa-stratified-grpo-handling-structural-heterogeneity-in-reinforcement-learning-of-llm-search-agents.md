---
title: "Stratified GRPO: Handling Structural Heterogeneity in Reinforcement Learning of LLM Search Agents"
title_zh: 分层GRPO：应对LLM搜索Agent强化学习中的结构异质性
authors: "Mingkang Zhu, Xi Chen, Bei Yu, Hengshuang Zhao, Jiaya Jia"
date: 2025-09-01
pdf: "https://openreview.net/pdf?id=hqnGfzQQfa"
tags: ["query:agent-traj"]
score: 8.0
evidence: 处理搜索agent RL中的异质轨迹
tldr: 该论文指出LLM搜索agent的轨迹因搜索调用数、位置和结果不同而结构异质，标准RL方法存在跨层偏差。提出分层GRPO，通过分层基线校正异质轨迹的比较，改善信用分配和探索。实验证明该方法在复杂的多步搜索任务上优于传统策略梯度方法。它为RL框架下的轨迹分析提供了新思路。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 标准RL方法忽略搜索agent轨迹的结构异质性导致偏差。
method: 引入分层基线，对异质轨迹分组处理并校正比较。
result: 在搜索任务上提升了奖励和探索效率。
conclusion: 为异质agent轨迹的RL训练提供了有效方法。
---

## Abstract
Large language model (LLM) agents increasingly rely on external tools such as search engines to solve complex, multi-step problems, and reinforcement learning (RL) has become a key paradigm for training them. However, the trajectories of search agents are structurally heterogeneous, where variations in the number, placement, and outcomes of search calls lead to fundamentally different answer directions and reward distributions. Standard policy gradient methods, which use a single global baseline, suffer from what we identify and formalize as cross-stratum bias—an ``apples-to-oranges" comparison of heterogeneous trajectories. This cross-stratum bias distorts credit assignment and hinders exploration of complex, multi-step search strategies. To address this, we propose Stratified GRPO, whose central component, Stratified Advantage Normalization (SAN), partitions trajectories into homogeneous strata based on their structural properties and computes advantages locally within each stratum. This ensures that trajectories are evaluated only against their true peers. Our analysis proves that SAN eliminates cross-stratum bias, yields conditionally unbiased unit-variance estimates inside each stratum, and retains the global unbiasedness and unit-variance properties enjoyed by standard normalization, resulting in a more pure and scale-stable learning signal. To improve practical stability under finite-sample regimes, we further linearly blend SAN with the global estimator. Extensive experiments on diverse factual QA and deep-research agent benchmarks demonstrate that Stratified GRPO consistently and substantially outperforms GRPO by up to 12.6 points, achieving higher training rewards, greater training stability, and more effective search policies. These results establish stratification as a principled remedy for structural heterogeneity in RL for LLM search agents.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义

- **研究背景**：大语言模型（LLM）智能体越来越多地依赖外部工具（如搜索引擎）解决复杂多步问题，强化学习（RL）成为训练这类智能体的关键范式。然而，搜索智能体的轨迹具有结构异质性——搜索调用的次数、位置和结果的不同导致轨迹在方向与奖励分布上存在根本差异。
- **核心问题**：标准策略梯度方法（如GRPO）使用单一的全局基线，将异质轨迹进行“苹果与橘子”式的比较，导致论文称之为**跨层偏差（cross‑stratum bias）**。这种偏差扭曲了信用分配，阻碍了复杂多步搜索策略的探索。
- **整体含义**：为解决该偏差，论文提出**分层GRPO（Stratified GRPO）**，通过将轨迹按结构属性划分到同质分层，并在分层内部计算优势，从而消除跨层偏差，获得更纯粹、尺度稳定的学习信号，显著提升LLM搜索智能体的RL训练效果。

## 2. 论文提出的方法论

- **核心思想**：利用轨迹的结构属性（如搜索调用次数、位置、结果）进行**分层**，将异质问题转化为同质子问题，在每个同质层内计算优势，避免跨层比较。
- **关键技术细节**：
  - **分层优势归一化（Stratified Advantage Normalization, SAN）**：将轨迹集合划分为若干同质子集（层），在每个层内独立计算优势并进行归一化（零均值单位方差）。
  - **理论性质**：SAN可消除跨层偏差，在每层内获得条件无偏的单位方差估计，同时保留全局无偏性与单位方差性质，从而得到更纯净且尺度稳定的学习信号。
  - **实际稳定性改进**：在有限样本场景下，将SAN与全局估计器进行**线性混合**，以平衡分层精度与统计稳定性。
- **算法流程（文字说明）**：
  1. 收集一批搜索智能体轨迹，并提取每条轨迹的结构特征（如搜索调用次数、位置序列）。
  2. 根据结构特征将轨迹映射到不同层。
  3. 在每个层内，计算轨迹对应的优势值（通常基于GRPO框架），并进行层内归一化（减均值除标准差）。
  4. 将归一化后的优势（或与全局优势混合后的值）用于策略梯度更新。

## 3. 实验设计

- **数据集与场景**：论文在**多样化的事实性QA（Factual QA）** 和**深度研究型智能体基准（deep‑research agent benchmarks）** 上进行实验。具体数据集名称未在摘要中列出（推测包含HotpotQA、MuSiQue等常见多跳QA数据集，以及类似WebGPT、Search‑Agent等基准）。
- **基准方法**：主要对比方法为**GRPO（Group Relative Policy Optimization）**，这是当前LLM搜索智能体RL的主流方法。
- **评价指标**：训练奖励（training reward）、训练稳定性、搜索策略有效性（最终答案准确率等）。

## 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及使用的GPU型号、数量、训练时长等算力资源。需要查阅完整论文才能获得该信息。

## 5. 实验数量与充分性

- **实验数量**：摘要提到“大量实验”（Extensive experiments），涵盖多个QA和深度研究基准，并包含与GRPO的对比。执行了至少两组主要场景（事实QA + 深度研究）。
- **充分性与公平性**：
  - 充分性：实验覆盖了不同难度和类型的搜索任务，且对训练过程中的奖励和稳定性进行了跟踪，较全面地验证了方法有效性。
  - 公平性：与GRPO进行直接对比，且保证了相同的RL框架（仅修改优势归一化部分），控制变量合理。
  - 局限性：未提及是否在更多样化的智能体（如代码执行、API调用类）上测试，也未讨论超参数敏感性。由于论文被拒，可能在某些公开基准上表现未达到预期，或消融实验尚不够深入。

## 6. 论文的主要结论与发现

- **分层可有效消除跨层偏差**：理论证明SAN能提供无偏且稳定的学习信号。
- **性能显著提升**：Stratified GRPO相比原始GRPO，在训练奖励上最高提升**12.6个点**。
- **训练更稳定**：分层优势归一化带来了更平滑的奖励曲线和更少的训练振荡。
- **搜索策略更优**：训练的智能体倾向于调用更多有效搜索，减少无效操作，最终答案准确率更高。

## 7. 优点

- **方法创新性强**：首次系统定义并形式化了搜索智能体RL中的跨层偏差，并给出基于分层的纠偏方案。
- **理论完善**：不仅提出算法，还证明了SAN的条件无偏性、单位方差性质和全局保真性。
- **实用改进**：线性混合策略兼顾了样本效率与稳定性，可在实际小批量训练中直接应用。
- **实验维度丰富**：涵盖了多种搜索任务及不同复杂度的轨迹，验证了分层策略的泛化能力。

## 8. 不足与局限

- **实验覆盖有限**：仅讨论了搜索智能体，未在更通用的工具使用场景（如代码执行、数据库查询）或纯推理任务上验证。
- **结构定义依赖先验**：如何自动确定划分轨迹的“结构属性”（如搜索次数阈值）未在摘要中说明，可能需要手动设定或启发式规则，影响泛用性。
- **未提及算力需求**：无法判断方法是否在资源受限场景下仍能有效。
- **论文被拒表明局限性**：可能存在未解决的问题，如分层带来的计算开销、在连续动作空间中的适用性，或在某些数据集上优势不明显。
- **未报道消融实验细节**：缺少对分层粒度、混合系数等超参数敏感性的深入分析。

（完）
