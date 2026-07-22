---
title: "From Detection to Diagnosis: Advancing Hallucination Analysis with Automated Data Synthesis"
title_zh: 从检测到诊断：通过自动化数据合成推进幻觉分析
authors: "Yanyi Liu, Qingwen Yang, Tiezheng Guo, Feiyu Qu, Jun Liu, Yingyou Wen"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40495/44456"
tags: ["query:agent-output"]
score: 8.0
evidence: 针对智能体输出的幻觉检测与诊断
tldr: 针对LLM幻觉检测仅能二元判断的局限，提出幻觉诊断任务（HDT），要求模型定位错误位置、解释原因并修正内容。通过自动化数据合成构建诊断数据集，实验表明该方法能提供更可解释和可操作的反馈，推动幻觉研究从检测走向诊断。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40495/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 709, \"height\": 431, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40495/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 870, \"height\": 1393, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40495/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1853, \"height\": 495, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40495/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 850, \"height\": 421, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40495/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 862, \"height\": 476, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40495/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 816, \"height\": 280, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40495/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1720, \"height\": 789, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40495/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 881, \"height\": 419, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40495/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 766, \"height\": 591, \"label\": \"Table\"}]"
motivation: 当前幻觉检测方法缺乏可解释性和可操作反馈，限制了实际应用中的模型改进。
method: 定义幻觉诊断任务并自动化合成包含错误定位、因果解释和修正的数据集。
result: 提出的诊断方法在多个指标上优于纯检测基线，并能提供细粒度的错误分析。
conclusion: 将幻觉研究从检测推进到诊断，有助于构建更可靠的LLM系统。
---

## Abstract
Hallucinations in Large Language Models (LLMs), defined as the generation of content inconsistent with facts or context, represent a core obstacle to their reliable deployment in critical domains. Current research primarily focuses on binary "detection" approaches that, while capable of identifying hallucinations, fail to provide interpretable and actionable feedback for model improvement, thus limiting practical utility. To address this limitation, a new research paradigm is proposed, shifting from "detection" to "diagnosis". The Hallucination Diagnosis Task is introduced, a task which requires models to not only detect hallucinations, but also perform error localization, causal explanation, and content correction.
We develop the Hallucination Diagnosis Generator (HDG), an automated pipeline that systematically generates high-quality training samples with rich diagnostic metadata from raw corpora through multi-dimensional augmentation strategies including controlled fact fabrication and reasoning chain perturbation. Using HDG-generated data, we train HDM-4B-RL, a 4-billion-parameter hallucination diagnosis model, employing Group Relative Policy Optimization (GRPO) with a comprehensive reward function incorporating structural, accuracy, and localization signals.
Experimental results demonstrate that our model surpasses previous state-of-the-art detection models on the HaluEval benchmark while achieving comparable performance to advanced general-purpose models. In comprehensive diagnosis tasks, HDM-4B-RL matches the capabilities of larger general models while maintaining a smaller size. This work validates the feasibility and value of hallucination diagnosis, providing an effective methodology for building more trustworthy and reliable generative AI systems.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：大型语言模型（LLMs）生成的“幻觉”内容（与事实或上下文不一致）阻碍了其在关键领域（如医疗、法律）的可靠部署。现有研究主要集中于二元“检测”（判断是否幻觉），但这种方法缺乏可解释性和可操作的反馈，无法有效指导模型改进或人工修正，限制了实际应用价值。
- **研究动机**：提出新的研究范式——从“检测”升级到“诊断”。类比医疗诊断，不仅判断是否有病，还要定位病灶、解释病因并提出治疗方案。
- **整体含义**：本文旨在通过自动化数据合成和强化学习训练，构建一个能够同时完成检测、定位、解释和修正的幻觉诊断模型，为构建更可信赖的生成式AI系统提供方法论。

## 2. 方法论：核心思想、关键技术细节
### 核心思想
- 定义**幻觉诊断任务（Hallucination Diagnosis Task）**，要求模型具备四项核心能力：
  - **检测**：准确判断生成内容是否与源上下文一致。
  - **定位**：指出输出中具体的幻觉片段（句子级别）。
  - **解释**：用自然语言解释为何内容不一致。
  - **修正**：基于定位和解释，生成与源一致的修正内容。
- 自动化数据合成管道**HDG**，从大规模预训练语料中生成带有丰富诊断元数据的训练样本。
- 使用**GRPO（Group Relative Policy Optimization）** 算法，结合结构化输出奖励、检测准确率奖励和定位奖励，训练4B参数的诊断模型**HDM-4B-RL**。

### 关键技术细节
1. **HDG数据合成管道（四阶段）**：
   - **阶段1：种子样本生成**。从Wikipedia（20231101.en）中随机采样文档，通过启发式过滤和递归分割得到短文本；生成涵盖摘要、逻辑推理、数学等任务的指令；利用LLM（Qwen3-32B）生成带CoT推理链的回答。
   - **阶段2：样本增强**（核心创新）：
     - **上下文增强**：程序化增删上下文信息（添加干扰项、删除关键细节）。
     - **幻觉注入**：对“正确”回答直接注入（替换实体）或通过推理链扰动注入逻辑幻觉。
     - **模糊信息替换**：将精确数字替换为模糊表述（仅用于摘要任务）。
   - **阶段3：质量验证**：使用LLM评估样本质量和标签正确性；集成多个检测模型（Lynx、MiniCheck-7B、Qwen3-32B、GPT-4.1）通过置信度集成来提高标签准确性。
   - **阶段4：元数据丰富**：添加句子级标注（定位幻觉位置）、任务难度标签（基于推理链长度）、推理轨迹（解释判断依据）。
2. **HDM-4B-RL训练**：
   - 基座模型：Qwen3-4B（启用推理模式）。
   - 算法：GRPO（组相对策略优化），通过比较一组生成样本的相对质量来更新策略，避免SFT破坏模型原有的推理能力。
   - **奖励函数**（加权和）：
     - **结构化输出奖励**\(R_{struct}\)：奖励输出为合规JSON（包含conclusion, diagnosis, hallucinations, corrected answer字段）。
     - **检测准确率奖励**\(R_{acc}\)：检测结论与真实标签一致时得1，否则0。
     - **定位奖励**\(R_{loc}\)：基于预测幻觉句子与真实句子集的命中率及长度比计算得分。
     - 总奖励 \(R = \alpha_1 R_{struct} + \alpha_2 R_{acc} + \alpha_3 R_{loc}\)，\(\alpha_1=1.0, \alpha_2=0.5, \alpha_3=0.5\)。

## 3. 实验设计：数据集、基准、对比方法
### 评估数据集
- **检测任务**：使用 **HaluBench** 基准，包含多个子集：
  - QA任务：HaluEval (10000)、RAGTruth (1000)、FinanceBench (1000)、DROP (1000)、CovidQA (1000)、PubMedQA (1000)
  - 摘要任务：SummEval (1600)
- **诊断任务**：自己构建的诊断基准，从SummEval中筛选出所有含幻觉的摘要（285个样本），额外标注了句子级幻觉位置和修正答案。
- **训练数据**：HDG从Wikipedia合成的18453个样本（含约60%幻觉样本），分布见Table 2。

### 对比方法
- **专业幻觉检测模型**：HHEM (110M)、Alignscore-Large (355M)、FactCG (435M)、MiniCheck (783M/7B)、Lynx-8B。
- **通用LLM（提示法）**：Qwen3-4B（推理/非推理模式）、Qwen3-32B、GPT-4.1、o4-mini。
- 对于诊断任务，还对比了**管道方法**（分步执行检测、定位、修正）与**单次提示法**。

### 评估指标
- **检测**：Macro F1（主指标），同时报告Precision、Recall、Accuracy。
- **诊断**：检测（Accuracy）、定位（Hit Rate + Span Validity）、修正（AlignScore）。

## 4. 资源与算力
- 训练使用 **2 × NVIDIA A100 80GB GPU**。一个GPU用于策略模型rollout，另一个用于奖励计算和参数更新。
- 训练超参数：AdamW优化器，学习率1e-6，weight decay 0.01，batch size 64，共训练 **2306步**。
- 评估时，开源模型部署在单张A100 80GB上使用vLLM框架；大模型（如Qwen3-32B）使用FP8量化；闭源模型通过API调用。
- 论文未明确说明训练总时长（小时数）。

## 5. 实验数量与充分性
- **检测实验**：在7个数据集（6个QA + 1个摘要）上进行，对比了6个专业检测模型和7个通用模型配置（含多个大小/推理模式），主表为Table 3，进行了完整的F1比较。此外还分析了推理模式的影响（Table 4）和训练有效性（RL vs 基座）。
- **诊断实验**：在自建的285个样本集上，比较了5种方法（管道法+单次提示法），使用3个指标，见Table 5。
- **其他分析**：统计了合成数据集的推理步数分布（Figure 3）和上下文长度分布（Figure 4）。
- **充分性评价**：
  - **优点**：数据集覆盖多领域、多难度；对比方法全面（专业模型+通用模型）；评估指标多样。消融体现在比较了推理/非推理模式、RL vs 基座，以及管道法 vs 单次法。
  - **不足**：
    - 诊断数据集仅285个样本，规模较小，可能不足以充分评估诊断能力。
    - 未进行跨模型架构的消融（如换用其他基座模型）。
    - 未测试在更多真实场景（如开放域对话）中的泛化性。
  - **公平性**：论文在相同基准上使用官方或标准评估设置，对比公平；但训练数据与评测数据存在重叠风险（HaluBench部分子集可能包含类似Wikipedia内容），文中未明确说明数据污染检查。

## 6. 主要结论与发现
1. **检测性能超越SOTA专业模型**：HDM-4B-RL在HaluBench上平均F1达79.65，超过同任务的最佳专业模型Lynx-8B（77.25），且模型大小仅为后者一半。
2. **接近/超越部分通用大模型**：优于GPT-4.1（75.37）和Qwen3-32B非推理模式（74.13），接近Qwen3-32B推理模式（81.70），但运行时长仅为32B模型的约1/2.2。
3. **推理模式显著提升性能**：无论是基座还是微调模型，启用推理模式均带来F1大幅提升（Qwen3-4B +11.82，HDM-4B-RL +9.52），主要源于Recall提升。
4. **RL训练有效**：对比基座Qwen3-4B，HDM-4B-RL在推理模式下F1提升2.91（Recall +4.62, Precision +2.50），非推理模式下F1提升5.21（Recall +5.97）。
5. **诊断任务上轻量级单次模型逼近重模型管道**：HDM-4B-RL作为单次提示4B模型，在检测、定位、修正上均优于同等单次提示的GPT-4.1和o4-mini，接近32B管道法的质量，但延迟仅为32B管道的约1/5。

## 7. 优点
- **任务定义创新**：首次系统定义了“幻觉诊断”任务，超越了简单二元检测，提供了更丰富的可解释反馈。
- **自动化数据合成管道（HDG）**：提出多维增强策略（上下文增强、幻觉注入、模糊替换），能够生成多样化、可控制的训练数据，解决诊断数据稀缺问题。
- **轻量高效**：4B模型在检测任务上超越8B专业模型，在诊断任务上匹配32B通用模型的管道性能，印证了专用小模型+高质量数据的有效性。
- **奖励函数设计精细**：结合结构化、准确性和定位三种信号，使得RL训练能同时提升检测和诊断能力。
- **实验分析深入**：不仅报告宏观指标，还分析了推理模式影响、RL带来的Precision/Recall变化，以及延迟对比。

## 8. 不足与局限
- **数据来源单一**：仅使用Wikipedia，缺乏科学文献、法律文书等专业领域数据，可能限制模型在垂直领域的泛化性。
- **诊断基准规模小**：仅含285个样本，且来自单一摘要数据集（SummEval），可能不足以全面评估诊断能力，且存在标注偏差。
- **模型规模局限**：仅实验了4B参数，未验证在更大模型（如32B, 70B）上的效果，也未测试连续RL训练的影响。
- **未进行数据污染检查**：训练数据来自Wikipedia，而评估基准可能包含类似来源，存在数据泄漏风险。
- **定位和修正指标**：定位采用Hit Rate和Span Validity，修正采用AlignScore，尚缺少人工评估或更细致的自动评估（如修正是否保留原文语义、流畅性等）。
- **算力开销未详细报告**：总训练时间、合成数据成本等未给出，不利于可复现性评估。
- **应用限制**：目前仅限于“忠实度幻觉”（faithfulness hallucination），未涵盖其他类型（如事实性、上下文冲突等），且诊断任务可能增加延迟，影响实时应用。

（完）
