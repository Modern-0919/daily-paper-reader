---
title: Failure Localization in Multi-Agent Code Generation via Knowledge-Guided and Transferable Reasoning
title_zh: 基于知识引导和可迁移推理的多智能体代码生成故障定位
authors: "Mingyang Geng, Shanzhi Gu, Zhipeng Liu, Chuanfu Xu, Zhaoyang Qu, Haotian Wang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/36993/40955"
tags: ["query:agent-errors"]
score: 8.0
evidence: 多智能体代码生成中考虑智能体间依赖的故障定位
tldr: 多智能体LLM代码生成中故障定位因智能体间依赖和路径多样性而困难。本文提出FLKR，一个自监督框架，结合行为编码、知识策略对齐和一致性评分，实现与解路径无关的故障定位。同时引入COFL基准进行评估。实验证明FLKR在多种场景下优于现有方法。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36993/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 850, \"height\": 657, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36993/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1613, \"height\": 948, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36993/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 841, \"height\": 506, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36993/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1797, \"height\": 531, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36993/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 807, \"height\": 463, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-36993/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 887, \"height\": 539, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-36993/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 885, \"height\": 552, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-36993/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 790, \"height\": 260, \"label\": \"Table\"}]"
motivation: 多智能体代码生成中智能体间依赖导致故障定位困难，现有方法对语义有效但非标准策略脆弱。
method: 提出FLKR框架，结合行为编码、知识策略对齐和一致性评分，实现解路径不变的故障定位。
result: 在COFL基准上，FLKR相比于提示方法显著提升故障定位准确率。
conclusion: 知识引导的推理可有效定位多智能体系统中的故障，提升代码生成可靠性。
---

## Abstract
Recent advances in multi-agent Large Language Model-based code generation enable collaborative software development through role-specialized agents. However, failure localization of code generation remains challenging due to inter-agent dependencies and solution-path multiplicity. Consequently, existing prompting-based localization methods exhibit vulnerability towards semantically valid but non-canonical strategies. To address this, we propose FLKR (Failure Localization via Knowledge-guided Reasoning), an self-supervised framework that combines behavior encoding, knowledge-strategy alignment, and consistency scoring for solution-path invariant localization. To evaluate, we also introduce COFL (Code Oriented Failure Localization), the first expert-annotated benchmark for fine-grained failure localization. Experiments show FLKR outperforms state-of-the-art prompting-based baselines by up to 14 points in Fault Localization Accuracy and 45 points in Top-1 accuracy, with strong performance in divergent, real-world, and refinement-critical cases. Such results demonstrate that our proposed FLKR generalizes well to real-world software development scenarios and opens up a new direction for failure-aware refinement recommendation by providing precise and interpretable responsibility signals.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：近年来，基于多智能体大语言模型的代码生成系统（如MetaGPT）通过角色专业化代理（产品经理、架构师、工程师、QA等）协作完成复杂软件开发。然而，随着协作复杂度增加，系统级故障频繁出现，源于代理间的错误沟通或有缺陷的依赖关系。准确地对故障进行**代理级定位**（即判断哪个代理导致失败及其责任比例）是构建可信、可调试系统的关键，但现有研究主要基于自然语言对话的提示方法（如全上下文输入、逐步提示、二分搜索）存在以下局限：
  - **表面文本对齐**：仅依赖文本表面相似性，无法应对代码数据与文本数据的结构性差异。
  - **解路径多重性**：编程任务存在多种正确算法策略（如贪心 vs. 动态规划），失败可能源于一条路径上的概念误解，而表面代码与参考代码差异很大，提示方法难以适应。
  - **代理间依赖**：上游设计缺陷会级联影响下游，隔离责任困难。
- **整体含义**：本文系统性地提出并研究了**多智能体代码生成中的故障定位**这一新问题，旨在实现不依赖人工标注的自监督代理级责任定位，为后续故障感知的细化修复推荐提供可解释的信号。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
提出**FLKR（Failure Localization via Knowledge-guided Reasoning）**，一个无监督框架，通过**行为编码**、**知识策略对齐**和**一致性评分**实现**解路径不变**的故障定位。即无论代理采用何种正确策略，都能鲁棒地定位故障。

### 关键技术细节（流程文字说明）

1. **语义编码器（Semantic Encoder）**  
   使用冻结的CodeBERT作为编码器，将每个代理的决策单元（角色+内容）编码为稠密向量 \( z_i \)。
2. **知识投影模块（Knowledge Projection Module, KPM）**  
   - 从领域知识库中为问题P检索 \( m \) 个规范策略模板（代码骨架+推理链）。  
   - 将代理表示 \( z_i \) 和知识条目 \( K_j \) 通过可训练投影函数 \( \phi \) 映射到共享潜在空间，得到 \( \tilde{z}_i, \tilde{k}_j \)。  
   - 计算对齐分数 \( \alpha_{i,j} = \text{softmax}_j(\tilde{z}_i^\top \tilde{k}_j) \)，表示代理决策与规范策略的兼容性。
3. **一致性评分器（Consistency Scorer）**  
   计算两种无监督偏差信号：  
   - **知识偏差** \( d_i^{\text{knowledge}} = 1 - \max_j \alpha_{i,j} \)，反映决策偏离所有已知有效策略的程度。  
   - **上下文偏差** \( d_i^{\text{context}} = \| z_i - \frac{1}{|\mathcal{N}_i|} \sum_{j \in \mathcal{N}_i} z_j \| \)，衡量与相邻代理行为的差异。  
   最终线性组合：\( d_i = \lambda_1 d_i^{\text{knowledge}} + \lambda_2 d_i^{\text{context}} \)。
4. **无监督责任估计器（Unsupervised Responsibility Estimator）**  
   将语义表示 \( z_i \) 与偏差分数 \( d_i \) 拼接，输入MLP得到未归一化得分 \( s_i \)，再经softmax得到责任分数 \( r_i = \exp(s_i) / \sum_j \exp(s_j) \)。输出形式为 \( y = \{(A_i, r_i)\} \)。
5. **训练目标**  
   - 对比学习对齐损失（InfoNCE）：拉近代理表示与最佳匹配知识向量，推远与其他知识向量。  
   - 自蒸馏损失：以一致性分数 \( d_i \) 作为软伪标签，约束责任估计器的输出。  
   - 总损失：\( \mathcal{L} = \mathcal{L}_{\text{align}} + \lambda \cdot \mathcal{L}_{\text{distill}} \)。

### 算法流程（文字描述）

1. 编码每个代理的决策为语义向量，通过投影函数得到对齐表示。  
2. 检索问题P对应的Top-5策略模板，编码并投影。  
3. 计算每个代理与知识的对齐分数，得到知识偏差和上下文偏差，线性组合为一致性分数。  
4. 将语义表示与一致性分数输入MLP，归一化为责任分布。  
5. 端到端训练，优化对比对齐损失和自蒸馏损失。

## 3. 实验设计：数据集、基准与对比方法

### 数据集
- **训练集**：从Codeforces使用MetaGPT收集的2520个无标签失败案例（包含完整代理交互日志）。
- **评估基准COFL**：首个专家标注的细粒度故障定位基准，包含142个人工标注案例，覆盖多种算法领域。特别地，包含84个“解路径发散”子集（代理使用有效但非规范策略）。
- **泛化测试集**：从SoftwareDev数据集（真实世界多代理软件任务，如小游戏、数据可视化、工具应用等）中选取70个失败案例，手动标注责任向量。

### 对比方法
- 提示基线（Zhang et al., 2025）：
  - **All at once Prompting**：整段日志输入LLM直接预测。
  - **Step by Step Prompting**：逐步输入并询问每一步故障。
  - **Binary Search Prompting**：递归二分日志定位。
- 消融变体（对FLKR）：
  - w/o KPM：移除知识投影模块。
  - w/o context score：仅使用知识偏差。
  - w/o knowledge score：仅使用上下文偏差。
  - w/o \( \mathcal{L}_{\text{align}} \)：移除对比损失。
  - w/o \( \mathcal{L}_{\text{distill}} \)：移除自蒸馏损失。

### 评估指标
- **故障定位准确率（FLA）**：预测责任向量与真值向量的余弦相似度。
- **Top-1定位准确率（Top-1 LA）**：预测最高责任代理是否与真实责任代理匹配。

## 4. 资源与算力

论文在“Experimental Settings”表中明确说明：
- **硬件**：2× NVIDIA A100 GPU（40GB显存）
- **训练轮次**：20个epoch
- **批大小**：16个失败案例
- 未说明具体训练时长，但基于上述配置和2520个训练案例，属于中等规模训练。

## 5. 实验数量与充分性

论文进行了以下多组实验，覆盖不同维度：

| 实验类型 | 具体内容 | 说明 |
|----------|----------|------|
| **主实验（RQ1）** | 在COFL的142个常规案例上对比FLKR与所有基线 | 验证整体准确性 |
| **解路径发散实验（RQ2）** | 在84个发散案例上对比 | 检验解路径不变性 |
| **人类评估（RQ3）** | 80个COFL案例，由6名经验注释者打分（正确性、针对性、可操作性，1-5量表） | 评估细化建议质量 |
| **泛化实验（RQ4）** | 在70个SoftwareDev案例上测试 | 检验跨域泛化 |
| **敏感性分析（RQ5）** | 变换训练数据比例、蒸馏权重λ、对比温度τ | 检验超参数鲁棒性 |
| **分布分析（RQ6）** | 计算预测责任分布的信息熵 | 检验定位聚焦性 |

**充分性评价**：实验设计较为全面，覆盖了核心性能、消融分析、泛化能力、鲁棒性、可解释性等多个维度。消融实验拆解了每个关键模块的贡献。无论是数据集规模（2520训练+212测试）、对比方法数量（3个提示基线+5个消融变体）、还是评估指标（FLA+Top-1 LA+人类评分），都体现了较好的客观性和公平性。唯一不足是缺少对真实多代理系统（如AutoGen、ChatDev）的跨框架泛化测试。

## 6. 论文的主要结论与发现

1. **FLKR在COFL上显著优于所有提示基线**：FLA达到0.89（比最佳提示基线高14个百分点），Top-1 LA达到78%（比提示基线高45个百分点）。  
2. **在解路径发散案例上优势更突出**：FLA 0.80 vs 基线最高0.68，Top-1 LA 62% vs 25%，证明知识引导对齐对路径多样性鲁棒。  
3. **人类评估表明FLKR生成的细化建议质量更高**：正确性4.3、针对性4.1、可操作性4.2（5分制），均高于提示基线。  
4. **泛化到真实世界软件任务表现良好**：在SoftwareDev数据集上FLA 0.81、Top-1 LA 76%，而提示基线下降至0.65、30%。  
5. **敏感性分析显示FLKR对超参数和数据量鲁棒**：在训练数据占比达80%后性能稳定；最优蒸馏权重λ≈0.7，最优温度τ≈0.07-0.08。  
6. **责任分布熵更低**：FLKR的定位更聚焦、更可解释，便于追溯错误源。

## 7. 优点：方法或实验设计上的亮点

- **问题新颖性**：首次针对多智能体代码生成中的细粒度代理级故障定位，并构建了专家标注基准COFL。  
- **方法创新**：  
  - 提出知识引导的对齐机制，使定位与解路径无关，克服了提示方法对表面文本的依赖。  
  - 结合知识偏差和上下文偏差两个自监督信号，无需人工标注即可训练。  
  - 采用对比学习+自蒸馏的两阶段训练，有效利用无标签数据。  
- **评估全面性**：  
  - 专门设计了发散案例子集，验证核心创新点。  
  - 进行了人类评估，从实用角度证明细粒度定位能带来更好的修复建议。  
  - 跨域泛化测试（算法题→真实软件任务）验证了通用性。  
- **可复现性**：公开了代码和数据集（GitHub链接），有利于后续研究。

## 8. 不足与局限

- **数据集规模有限**：训练数据来自单一平台Codeforces，评估基准COFL仅142个案例，泛化测试仅70个案例，可能无法充分覆盖所有多代理协作模式。  
- **框架依赖性**：实验仅在MetaGPT上开展，未验证在AutoGen、ChatDev等其他多代理框架上的有效性（论文“未来工作”提及但未实现）。  
- **知识库构建依赖人工**：知识模板需从Codeforces标签和手工标注的算法模板构建，对不同编程语言或领域的迁移需重新构建。  
- **评估偏差风险**：人类评估中使用的“细化建议”由GPT-4基于FLKR的定位生成，可能存在LLM自身偏差；注释者数量较少（6人），一致性检验未报告。  
- **计算资源未详细说明**：训练时间、推理速度等实用信息缺失，不利于实际部署评估。  
- **消融实验对比不全面**：如未引入随机初始化基线或更简单的统计方法（如基于代码执行迹的覆盖率），可能忽略了更baseline的方法。  
- **责任估计的离散性**：输出为连续责任分数，但在实际调试中可能难以直接转换为二元判断（谁负责），需要额外阈值设置。

（完）
