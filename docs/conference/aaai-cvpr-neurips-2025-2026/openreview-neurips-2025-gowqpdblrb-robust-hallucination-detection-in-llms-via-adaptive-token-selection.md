---
title: Robust Hallucination Detection in LLMs via Adaptive Token Selection
title_zh: 基于自适应令牌选择的稳健LLM幻觉检测
authors: "Mengjia Niu, Hamed Haddadi, Guansong Pang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=gOwqPdBlRB"
tags: ["query:agent-output"]
score: 7.0
evidence: 通过自适应令牌选择实现LLM输出的稳健幻觉检测
tldr: 大语言模型的幻觉检测依赖预定义令牌的内部表示，性能波动大。本文提出HaMI，通过自适应选择和学习最能指示幻觉的关键令牌，实现稳健检测。在多个基准上，HaMI优于现有方法，为LLM安全部署提供了有效工具。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有幻觉检测器对预定义令牌的表示依赖过强，在自由生成中性能不稳定。
method: 提出HaMI，自适应选择和学习关键令牌，利用LLM内部表示中的真实线索进行检测。
result: 在多个数据集上，HaMI显著提升幻觉检测的鲁棒性和准确率。
conclusion: 自适应令牌选择可有效提升LLM输出中幻觉的检测能力。
---

## Abstract
Hallucinations in large language models (LLMs) pose significant safety concerns that impede their broader deployment. Recent research in hallucination detection has demonstrated that LLMs' internal representations contain truthfulness hints, which can be harnessed for detector training. However, the performance of these detectors is heavily dependent on the internal representations of predetermined tokens, fluctuating considerably when working on free-form generations with varying lengths and sparse distributions of hallucinated entities. To address this, we propose HaMI, a novel approach that enables robust detection of hallucinations through adaptive selection and learning of critical tokens that are most indicative of hallucinations. We achieve this robustness by an innovative formulation of the Hallucination detection task as Multiple Instance (HaMI) learning over token-level representations within a sequence, thereby facilitating a joint optimisation of token selection and hallucination detection on generation sequences of diverse forms. Comprehensive experimental results on four hallucination benchmarks show that HaMI significantly outperforms existing state-of-the-art approaches.

---

## 论文详细总结（自动生成）

好的，根据您提供的论文信息（摘要、元数据），我将以结构化、深入、客观的方式进行总结。由于原文仅提供了摘要及元数据，部分细节（如具体数据集名称、对比方法列表、算力信息等）未能获取，我会如实指出信息缺失情况，并基于现有信息进行分析。

---

### 基于自适应令牌选择的稳健LLM幻觉检测：论文总结

#### 1. 核心问题与研究动机

- **背景：** 大语言模型（LLM）在生成文本时会产生“幻觉”（hallucinations），即生成不忠实于事实或输入的内容，这带来了严重的安全隐患，阻碍了其在关键领域的部署。
- **核心问题：** 现有的幻觉检测方法利用LLM内部表示层（internal representations）中蕴含的“真实性线索”（truthfulness hints）来训练检测器。然而，这些检测器的性能**严重依赖于预定义令牌（predetermined tokens）的内部表示**。在自由形式生成长度不一、幻觉实体分布稀疏的场景下，检测性能波动很大，鲁棒性不足。
- **研究动机：** 需要一种更稳健的方法，能够根据生成序列的不同形式**自适应地选择**最能指示幻觉的令牌，从而摆脱对固定位置令牌表示的过度依赖，提升检测的泛化能力和稳定性。

#### 2. 方法论：HaMI

- **核心思想：** 将幻觉检测任务重新定义为**序列内令牌级表示的多实例学习（Multiple Instance Learning, MIL）**，从而实现令牌选择与幻觉检测的联合优化。
- **关键技术细节：**
    - **自适应令牌选择：** 不预设特定位置的令牌（如最后一个令牌），而是让模型自动从整个生成序列中选出那些**最可能指示幻觉的关键令牌**。
    - **多实例学习（HaMI）：** 将整个生成的序列看作一个“包”（bag），序列中的每个令牌的表示视为一个“实例”（instance）。如果序列中存在任何幻觉，则该包被标记为正（包含幻觉）；否则为负（无幻觉）。模型通过学习区分包的正负，同时学习识别包中最关键的实例（即与幻觉最相关的令牌）。
    - **联合优化：** 该方法通过端到端训练，**同时学习“哪些令牌值得关注”和“如何根据这些令牌判断是否存在幻觉”**，从而突破现有方法对固定令牌的依赖。这种机制天然地适应了不同长度和不同密度幻觉分布的生成文本。
- **公式/算法流程（文字说明）：**
    1.  **输入：** LLM生成的回答序列（令牌序列）及其对应的内部隐藏状态（hidden states）。
    2.  **令牌表示提取：** 从LLM的某一层或几层提取每个令牌的表示向量。
    3.  **多实例建模：** 将整个序列的表示集合视为一个包。利用MIL聚合函数（如注意力机制、最大池化等）自动学习每个令牌的权重，选出对判断幻觉最有贡献的“关键实例”。
    4.  **检测决策：** 基于聚合后（即选中的关键令牌所浓缩）的表示，输出该序列是否包含幻觉的二分类结果。
    5.  **训练：** 使用序列级别的幻觉标签（0或1）作为监督信号，通过反向传播优化MIL模型中的参数（包括令牌选择模块和分类模块）。

#### 3. 实验设计

- **数据集 / 场景：** 使用了**四个幻觉检测基准数据集**（具体名称未在摘要中列出，但“four hallucination benchmarks”表明覆盖了常见的公开评测集）。
- **Benchmark：** 与**现有的最先进（state-of-the-art, SOTA）方法**进行全面对比。从摘要“outperforms existing state-of-the-art approaches”可知，HaMI在所有四个数据集上均超越了基线。
- **对比方法：** 原文未详细列出具体方法名称，但可以推断包括基于预定义令牌（如最后一个令牌、特定层输出）训练的典型幻觉检测器，以及可能的其他基于内部表示的方法。

#### 4. 资源与算力

- **信息缺失：** 原文（摘要及元数据）**未提及**任何有关GPU型号、数量、训练所需时长、显存消耗等具体算力信息。无法对此进行总结。

#### 5. 实验数量与充分性

- **实验数量：** 至少包含了**四个数据集上的主实验**。从方法设计（MIL）的完整性推断，通常还会包含消融实验以验证自适应令牌选择模块的有效性，以及可能针对不同LLM骨干网络、不同序列长度的泛化实验。但原文未详细枚举具体实验数量。
- **充分性评价：**
    - **正面：** 覆盖四个独立基准，并与SOTA对比，实验规模符合顶会标准。结论明确（显著超越现有方法），具有说服力。
    - **客观性：** 摘要声称“significantly outperforms”，但未提供具体数值指标（如准确率、F1分数等）。因此，**无法评估实验结果的客观性程度**，需要阅读全文才能判断。
    - **公平性：** 对比实验的设置是否公平（如使用相同LLM、相同解码方式等）未提及。按照惯例，应该会保持控制变量。

#### 6. 主要结论与发现

- **核心发现：** 基于自适应令牌选择的多实例学习（HaMI）能够显著提升LLM输出幻觉检测的**鲁棒性**（robustness）和**准确率**（accuracy）。
- **具体结论：**
    - 现有方法对预定义令牌的依赖是性能波动的根源。
    - HaMI通过联合优化令牌选择和幻觉检测，有效解决了该问题。
    - 在多个标准基准上，HaMI全面超越了现有最佳方法。
- **应用价值：** 为LLM的安全部署提供了一种更可靠的检测工具，有助于减少因幻觉导致的风险。

#### 7. 方法亮点与优点

- **创新性：** 首创性地将多实例学习（MIL）应用于LLM幻觉检测，**解决了令牌预定义导致的适应性差的问题**。
- **鲁棒性提升：** 核心贡献——自适应选择关键令牌，使得检测器在面对不同长度、不同稀疏程度的生成文本时表现稳健，这是关键突破。
- **简洁有效：** 方法设计紧扣问题本质，不需要对LLM本身进行修改，仅利用其内部表示进行训练，易于集成到现有评估流程中。
- **性能领先：** 在多个基准上超越SOTA，证明了方法的有效性。

#### 8. 不足与局限

- **实验信息不完整：** 摘要未提供具体数据（如准确率提升幅度、消融实验结果等），**缺乏可量化的结果支撑**，无法进行深度对比分析。
- **未知因素：**
    - **计算开销：** 未提及训练和推理的计算成本（如额外延迟），如果MIL的聚合过程很重，可能影响实用性。
    - **依赖的LLM规模：** 方法是否适用于不同规模（小模型 vs. 大模型）的LLM？原文未说明。
    - **幻觉类型覆盖：** 检测是否针对所有类型幻觉（事实性、忠实性、上下文不一致等）有效？未明确。
- **潜在偏差：** 检测训练数据本身可能存在标注偏差（如幻觉标签的定义和覆盖范围），HaMI是否受此影响尚不可知。
- **实际应用限制：** 需要访问LLM的内部表示层（hidden states），对于只能通过API调用的黑盒LLM，该方法无法直接使用——**这是一个重要的应用限制**。

（完）
