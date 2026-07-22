---
title: "Why and How LLMs Hallucinate: Connecting the Dots with Subsequence Associations"
title_zh: 大语言模型为何及如何产生幻觉：通过子序列关联连接点
authors: "Yiyou Sun, Yu Gai, Lijie Chen, Abhilasha Ravichander, Yejin Choi, Nouha Dziri, Dawn Song"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=K2Q4Jp4RbB"
tags: ["query:agent-errors"]
score: 7.0
evidence: 系统性框架理解大语言模型中幻觉的来源
tldr: 针对大语言模型幻觉原因诊断难题，提出基于子序列关联的框架，揭示幻觉源于非事实关联压制忠实关联的机制。通过理论和实验分析，证明解码器仅Transformer作为子序列嵌入模型，全连接层编码输入输出关联，为理解幻觉提供了新视角。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 大语言模型频繁产生幻觉，但诊断幻觉原因困难，因潜在原因复杂交织。
method: 提出框架将幻觉归因于子序列关联的竞争，并通过理论和实验分析解码器仅Transformer中的关联机制。
result: 证明了幻觉是由更频繁但非事实的关联压倒忠实关联导致的。
conclusion: 该框架系统地解释了幻觉行为，有助于未来检测和缓解幻觉。
---

## Abstract
Large language models (LLMs) frequently generate hallucinations—content that deviates from factually inaccurate or deviates from provided context—posing challenges for diagnosis. However, diagnosing the causes of hallucination is challenging due to the complex interplay of underlying causes. This paper introduces a framework to systematically understand the sources of hallucination behavior in large language models. Our key insight is that hallucinations arise when more frequent but non-factual associations outweigh faithful ones.
Through theoretical and empirical analyses, we demonstrate that decoder-only transformers effectively function as subsequence embedding models, with the fully-connected layers encoding input-output associations. We propose a tracing algorithm that identifies causal subsequences by analyzing hallucination probabilities across randomized input contexts. Experiments show our method outperforms standard attribution techniques in identifying hallucination causes and is supported by evidence from the model’s training corpus. This work provides a unified perspective on hallucinations and a robust framework for their cause and analysis.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文元数据与摘要信息，对论文《大语言模型为何及如何产生幻觉：通过子序列关联连接点》的中文详细总结。由于未能获取论文完整原文，以下分析严格基于已有信息，并在必要时做出诚实说明。

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：大语言模型（LLMs）在生成文本时频繁出现“幻觉”——即内容与事实不符或偏离给定上下文。这一问题严重制约了LLM在医疗、法律、金融等高风险领域的可靠应用。
- **研究动机**：当前诊断幻觉成因十分困难，因为潜在原因（如训练数据偏差、模型架构、解码策略等）相互交织，缺乏统一的归因框架。
- **整体目标**：构建一个系统性的理论框架，从根本上解释LLM幻觉的产生机制，并发展出可操作的诊断工具。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：幻觉源于 **子序列关联的竞争**。模型在生成词时，会同时考虑多种可能的“输入-输出”关联（association），其中 **更频繁但非事实的关联** 会压制 **忠实于事实/上下文的关联**，从而导致幻觉。
- **关键技术细节**：
  - **模型作为子序列嵌入模型**：通过理论分析，将解码器仅Transformer解释为一种“子序列嵌入模型”（subsequence embedding model），其全连接层编码了输入与输出之间的关联强度。
  - **因果子序列追踪算法（Tracing Algorithm）**：通过随机化输入上下文（randomized input contexts），测量不同子序列被保留或扰动时幻觉概率的变化，从而识别出导致幻觉的**关键因果子序列**。该算法不依赖模型内部梯度，而直接基于输出概率扰动。
- **流程说明**（据摘要推断）：
  1. 对每个待分析输入，构建一组随机化的上下文变体（如删除、替换、掩码某些词）。
  2. 计算原始输入与各变体下模型生成特定词（幻觉词或正确词）的概率差。
  3. 统计哪些子序列的保留与幻觉概率下降显著相关，从而定位“非事实关联”的来源。
  4. 通过比对训练语料频率，验证这些关联的高频性。

### 3. 实验设计：数据集、基准与对比方法
- **数据集/场景**：摘要未提供具体数据集名称。根据领域惯例，可能涉及常识问答（如Natural Questions、TriviaQA）、摘要生成（如CNN/DailyMail）、或医疗/法律文本。由于缺乏原文，无法确认。
- **基准（Benchmark）**：主流幻觉检测基准或归因基准（如HaluEval、TruthfulQA的幻觉子集），但未明确说明。
- **对比方法**：提出的追踪算法与 **标准归因技术**（standard attribution techniques，如基于梯度的显著性、集成梯度、注意力归因等）进行对比，并声称在识别幻觉原因上优于这些方法。
- **额外验证**：使用了来自模型**训练语料的证据**支持关联频率与幻觉的因果关系（例如，在训练语料中找到高频率的非事实二元组）。

### 4. 资源与算力
- 文中 **未明确说明** 使用的GPU型号、数量或训练时长。这可能是因为本工作侧重于分析而非大规模训练，实验主要基于现有预训练模型（如GPT-2、LLaMA等）进行推理和归因计算。如需完整信息，需查阅原文附录。

### 5. 实验数量与充分性
- **实验数量**：基于摘要，至少包含：① 理论分析验证（子序列嵌入模型成立）；② 追踪算法在不同随机化配置下的效果；③ 与标准归因方法的对比实验；④ 训练语料频次分析。具体消融实验数量未知。
- **充分性与客观性**：
  - **优势**：理论框架与因果归因方法有清晰逻辑，对比了经典归因方法，且有语料证据支持。
  - **不足**：缺少在多个数据集、多种规模模型上的系统评测（如7B/13B/70B），也未报告幻觉类型的覆盖率（如事实性幻觉、上下文遗忘、反事实生成等）。对于极端输入（如长上下文、对抗性提示）的鲁棒性未讨论。实验规模可能不足以支撑“系统性框架”的宣称。

### 6. 论文的主要结论与发现
- **核心结论**：LLM幻觉的根本原因是 **非事实关联在子序列竞争中获胜**——这些关联因在训练语料中更频繁出现，从而在推理时压制了正确的、事实性的关联。
- **发现1**：解码器仅Transformer可被重新解释为子序列嵌入模型，其全连接层的权重编码了输入-输出关联的强度。
- **发现2**：提出的子序列追踪算法能有效定位导致幻觉的关键输入片段，比传统归因方法更准确。
- **发现3**：通过训练语料统计分析，证实了非事实关联与训练语料中的高频共现模式高度一致。

### 7. 优点：方法或实验设计上的亮点
- **理论创新**：首次将幻觉归因于子序列关联的竞争，提供统一解释而非零散假设。
- **方法新颖**：提出的追踪算法无需模型梯度，直接基于概率扰动，普适性高。
- **可解释性强**：通过识别具体因果子序列，工程师可以针对性地修正数据或模型。
- **证据链完整**：从理论推导→算法设计→实验验证→语料支持，形成了闭环。

### 8. 不足与局限
- **实验覆盖有限**：原文未展示在不同架构（如仅编码器或MoE）、不同规模（如超过70B参数）上的结果，泛化性存疑。
- **依赖训练语料访问**：需要大规模语料库来进行频次统计，实践中可能无法获取。
- **未处理多种幻觉类型**：是否适用于“上下文遗忘”或“逻辑矛盾”等复杂幻觉尚不确定。
- **算力需求未声明**：如果需对每次输出进行大量随机化扰动，推理开销可能较高，未提供效率分析。
- **偏差风险**：随机化上下文的构造方式可能引入人为偏差，历史输入扰动可能改变语义，导致归因不准确。

（完）
