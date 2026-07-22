---
title: "CoFact: Dynamic Coordination of Attention Heads for Improving Factual Consistency in LLMs"
title_zh: CoFact：动态协调注意力头以提升LLM的事实一致性
authors: "Shike Li, Xiaokai Wang, Xiaofeng Liu, Xin Tong, Hu Zhang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40438/44399"
tags: ["query:agent-errors"]
score: 6.0
evidence: 通过动态协调注意力头提升事实一致性以减轻幻觉
tldr: 针对LLM推理时事实不一致问题，提出CoFact机制，借鉴合作博弈理论动态协调多头注意力的行为，引导模型生成更忠实于事实的内容。实验在多个幻觉基准上验证了有效性和通用性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40438/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 745, \"height\": 863, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40438/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1637, \"height\": 782, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40438/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 666, \"height\": 506, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40438/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 878, \"height\": 412, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40438/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 879, \"height\": 464, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40438/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1825, \"height\": 1784, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40438/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 869, \"height\": 439, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40438/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 866, \"height\": 263, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40438/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 879, \"height\": 832, \"label\": \"Table\"}]"
motivation: 现有推理时干预方法独立处理注意力头，忽略了多头之间的协作结构。
method: 基于合作博弈论设计注意力头协调机制，在推理时动态调整头的行为。
result: 在多个事实一致性基准上优于先前方法，且与现有解码策略兼容。
conclusion: 利用注意力头协调可有效提升LLM的事实一致性，减少幻觉。
---

## Abstract
Large language models (LLMs) frequently generate fluent yet factually inaccurate content, a phenomenon known as hallucination. Recent inference-time approaches aim to improve truthfulness by steering model activations toward semantically meaningful directions. While effective to some extent, these methods typically process activations independently, neglecting the internal coordination structure of multi-head attention (MHA), where attention heads interact to form semantic representations. In this work, we propose CoFact, an adaptive inference-time mechanism that improves factual consistency by dynamically coordinating attention head behaviors. Inspired by cooperative game theory, CoFact conceptualizes attention heads as collaborative agents. It models the semantic utility and redundancy of each head and adaptively modulates their contributions to the final attention output. Notably, rather than directly altering intermediate representations, CoFact performs token-level coordination to encourage diverse and complementary attention patterns across heads. CoFact is plug-and-play compatible with mainstream LLM architectures and requires no additional supervision or model retraining. Experimental results across multiple standard factuality benchmarks demonstrate that CoFact consistently enhances factual accuracy while maintaining generation fluency.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：大型语言模型（LLM）在生成流畅文本时经常出现事实性错误（幻觉），尤其是在高风险领域（医疗、法律、教育）需要严格的事实一致性。
- **现有方法的不足**：已有的推理时干预方法（如ITI、DoLa）通常独立地处理每个注意力头的激活，忽略了多头注意力（MHA）模块内部头与头之间的协作结构。实验观察发现，注意力头经常关注重叠区域，导致冗余信息流和语义多样性下降，从而增加幻觉风险。
- **整体含义**：本文提出**CoFact**，利用合作博弈论思想动态协调注意力头的行为，通过在推理时评估每个头的语义效用和冗余度，自适应地重新分配注意力权重，以促进互补和多样化的注意力模式，从而提升事实一致性。

## 2. 论文提出的方法论：核心思想、关键技术细节与流程

- **核心思想**：将注意力头视为合作博弈中的代理，每个头对当前令牌输出的贡献（效用）和与其他头之间的冗余度共同决定其激活权重。
- **关键技术细节**：
  - **语义估值器**：计算每个头对层输出的边际贡献 \( U_{con}(h_i) = \| O - O_{(-i)} \|_2 \)（排除该头后输出的变化量），作为该头独特信息量的代理。
  - **冲突仲裁器**：计算每个头与其他头注意力矩阵之间的平均余弦相似度 \( U_{red}(h_i) = \frac{1}{|H|-1} \sum_{j\neq i} C_{i,j} \)，衡量冗余度。
  - **联盟协调器**：组合效用 \( U(h_i) = \alpha U_{con}(h_i) - \beta U_{red}(h_i) \)，通过softmax归一化得到动态权重 \( \tilde{\alpha}_i = \frac{\exp(\lambda U(h_i))}{\sum_j \exp(\lambda U(h_j))} \)，最后加权各头输出得到CoFact输出。
- **流程**：在每个解码步骤，对每个层动态计算上述权重，保留全部注意力头但重新分配影响，无需修改模型参数或重新训练。

## 3. 实验设计

- **数据集/场景**：
  - **TruthfulQA**：包含多项选择（MC）和开放生成两种格式，主要评估事实性和信息量。
  - **TriviaQA** 和 **Natural Questions (NQ)**：用于验证跨数据集泛化能力（每数据集子集3610个问题）。
- **基准（Baselines）**：
  - 基础模型（Base）、DoLa、AD、ITI、NL_ITI、TruthFlow。
- **对比模型**：
  - LLaMA2-7B-Chat、LLaMA2-13B-Chat、LLaMA3-8B-Instruct、Mistral-7B-Instruct-v0.2/v0.3、Gemma2-9B-it。
- **评估指标**：
  - 开放生成：BLEURT、True（真实性）、Info（信息量）、True*Info（主要复合指标）。
  - 多项选择：准确率（MC Acc.）。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量、训练时长或推理硬件配置。CoFact为推理时方法，不涉及重训练，因此计算开销相对较低。但具体算力需求未量化。

## 5. 实验数量与充分性

- **实验数量**：
  - 主表（Table 1）覆盖6个模型在TruthfulQA上的完整结果。
  - 跨数据集泛化表（Table 2）在TriviaQA和NQ上的结果。
  - 激活策略对比（Figure 3）比较了CoFact与均匀权重、Top-k选择。
  - Token级和层内分析（Figure 4、5）可视化注意力行为变化。
  - 鲁棒性实验（Table 3）在噪声前缀、随机替换、高斯干扰下测试。
  - 消融实验（Table 4）移除语义估值器或冲突仲裁器后的性能下降。
- **充分性与客观性**：
  - 实验涵盖了多种模型架构、参数规模、数据集和对比方法，对比全面。
  - 消融验证了核心组件的必要性，鲁棒性测试显示了稳定性。
  - 评估指标多维度（真实性、信息量、复合分数、BLEURT），结果客观。总体实验设计充分、公平。

## 6. 论文的主要结论与发现

- CoFact在TruthfulQA开放生成上相对Base模型**True*Info提升最高达18.54%**（Gemma2-9B-it），并在多模型上一致优于所有基线。
- 在TriviaQA和NQ上，CoFact也取得最高事实性分数，且优于TruthFlow等强基线，显示良好泛化能力。
- 相比静态或均匀激活策略，CoFact通过动态协调实现了更高的事实性与信息量的平衡。
- 消融实验表明：语义效用和冗余惩罚两个组件缺一不可，协同作用效果最佳。
- 鲁棒性实验显示CoFact对输入噪声和注意力扰动具有较好抵抗能力。

## 7. 优点：方法或实验设计上的亮点

- **创新性**：首次将合作博弈论引入多头注意力头的协调，从任务层面建模头间协作与冗余。
- **实用性**：即插即用，无需重训练或外部知识，适用于主流LLM架构。
- **细粒度动态调节**：在令牌级别自适应调整头权重，优于固定或均匀激活。
- **实验广泛性**：跨6种不同模型、3个标准事实性基准、多种基线的对比，以及鲁棒性和消融分析，证据充分。
- **可视化分析**：通过token级激活热图和注意力相似度矩阵，直观展示了CoFact如何促进头 specialization 和减少冗余。

## 8. 不足与局限

- **未提供计算资源细节**：缺乏对推理延迟和GPU需求的量化分析，难以评估实际部署成本。
- **超参数依赖**：α、β、λ三个超参数需针对不同模型和任务调优，论文未提供系统调参指南。
- **任务覆盖有限**：仅在问答类事实性基准上评估，未涉及摘要、对话、长文本生成等更复杂的幻觉场景。
- **理论分析仅附带在附录**：论文主文中未深入解释为何冗余会导致幻觉，理论支撑可进一步加强。
- **未与其他推理时方法（如对比解码变体）做更广泛的对比**：如对比于PPL-based或特征编辑方法。
- **潜在偏差风险**：TruthfulQA等基准本身可能包含偏见，且GPT-4o-Mini评估器可能存在不一致性。

（完）
