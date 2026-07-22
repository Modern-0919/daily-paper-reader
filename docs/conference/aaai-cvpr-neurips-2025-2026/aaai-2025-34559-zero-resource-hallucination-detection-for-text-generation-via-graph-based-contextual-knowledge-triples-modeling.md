---
title: Zero-resource Hallucination Detection for Text Generation via Graph-based Contextual Knowledge Triples Modeling
title_zh: 基于图上下文知识三元组建模的零资源文本生成幻觉检测
authors: "Xinyue Fang, Zhen Huang, Zhiliang Tian, Minghui Fang, Ziyi Pan, Quntian Fang, Zhihua Wen, Hengyue Pan, Dongsheng Li"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34559/36714"
tags: ["query:agent-output"]
score: 7.0
evidence: 基于图的知识三元组零资源幻觉检测方法
tldr: 本文面向开放文本生成场景，提出零资源幻觉检测方法：将生成文本拆分为事实，构建基于图的知识三元组模型进行一致性比较。无需外部知识库，可泛化到agent输出检测任务，用于判断agent生成内容是否忠实。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34559/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1808, \"height\": 882, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2025-accepted/aaai-2025-34559/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 811, \"height\": 737, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34559/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 723, \"height\": 313, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34559/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 726, \"height\": 345, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34559/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 570, \"height\": 157, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2025-accepted/aaai-2025-34559/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 369, \"height\": 284, \"label\": \"Table\"}]"
motivation: 现有幻觉检测依赖外部知识或短答案，难以处理开放式长文本生成。
method: 将长文本分割为事实对，通过图建模上下文知识三元组，比较一致性。
result: 在多个生成任务上超越基线，无需外部资源即可有效检测幻觉。
conclusion: 为无资源场景下的幻觉检测提供通用方法，可适配agent输出验证。
---

## Abstract
LLMs obtain remarkable performance but suffer from hallucinations. Most research on detecting hallucination focuses on questions with short and concrete correct answers that are easy to check faithfulness. Hallucination detections for text generation with open-ended answers are more hard. Some researchers use external knowledge to detect hallucinations in generated texts, but external resources for specific scenarios are hard to access. Recent studies on detecting hallucinations in long texts without external resources conduct consistency comparison among multiple sampled outputs. To handle long texts, researchers split long texts into multiple facts and individually compare the consistency of each pair of facts. However, these methods (1) hardly achieve alignment among multiple facts; (2) overlook dependencies between multiple contextual facts. In this paper, we propose a graph-based context-aware (GCA) hallucination detection method for text generations, which aligns facts and considers the dependencies between contextual facts in consistency comparison. Particularly, to align multiple facts, we conduct a triple-oriented response segmentation to extract multiple knowledge triples. To model dependencies among contextual triples (facts), we construct contextual triples into a graph and enhance triples’ interactions via message passing and aggregating via RGCN. To avoid the omission of knowledge triples in long texts, we conduct an LLM-based reverse verification by reconstructing the knowledge triples. Experiments show that our model enhances hallucination detection and excels all baselines.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：大型语言模型（LLMs）在文本生成中常出现“幻觉”（hallucination），即生成看似合理但事实错误或虚构的内容。现有幻觉检测研究主要集中于具有简短、具体正确答案的任务（如问答、算术），这些任务易于验证一致性。然而，**开放式的长文本生成**场景更具挑战性：输出没有唯一正确答案，且长文本包含多个事实，难以进行一致性比较。
- **背景与动机**：现有零资源（无外部知识）的黑盒检测方法分为两类：
  - **自检**（Self-checking）：利用模型自身能力（如链式思维）验证事实，但可能导致漏检（模型过度自信）。
  - **一致性比较**（Consistency comparison）：通过多次采样输出，比较事实间的一致性。但面对长文本时，存在两个关键缺陷：
    1. **事实对齐困难**：不同采样中同一事实的措辞不同，难以精准匹配。
    2. **忽略上下文依赖**：仅孤立地比较单个事实，未考虑事实之间的相互依赖关系（例如“爱因斯坦提出相对论并因此获得诺贝尔奖”中，两个事实的依赖关系可能是错误的）。
- **研究目标**：在零资源、黑盒条件下，提出一种**基于图的上下文感知**幻觉检测方法，能够更好对齐事实并建模事实间的依赖关系。

## 2. 论文提出的方法论：核心思想与关键技术细节
**核心思想**：将长文本响应分解为知识三元组（头实体、关系、尾实体），构建图结构以捕获三元组间的上下文依赖，并通过图神经网络进行消息传递与聚合，同时设计反向验证任务减少遗漏。

### 整体框架（GCA）包含三个模块：

#### （1）三元组导向的响应分段（Triple-Oriented Response Segmentation）
- 使用LLM（GPT-4）从原始响应和采样响应中提取知识三元组（head, relation, tail）。
- 通过提示验证三元组与原文语义等价，若存在歧义则调整，确保事实表述结构化，便于后续对齐与比较。

#### （2）基于图的上下文一致性比较（GCCC, Graph-based Contextual Consistency Comparison）
- **图构建**：为原始响应和每个采样响应分别构建知识图，节点为实体（头/尾），边为关系。
- **表示初始化**：使用Sentence-BERT编码头/尾实体和关系得到初始嵌入。
- **消息传递与聚合**：采用RGCN（关系图卷积网络）逐层更新节点嵌入（公式1），整合邻居节点信息，使每个节点携带上下文依赖。
- **三元组一致性比较**：
  - 对齐原始响应与每个采样响应中的三元组（通过头实体相同且关系相似度 > 阈值θr）。
  - 计算尾实体嵌入相似度，若超过阈值θt则增加一致性分数ci,j，若有未对齐的三元组尾实体与原始尾实体相似度高则扣分（公式2）。
  - 最终一致性分数Ci为所有采样响应的总和。

#### （3）基于三元组重建的反向验证（RVF, Reverse Verification via Triple Reconstruction）
- 为解决GCCC中可能遗漏（某些事实未被采样响应覆盖）的问题，设计三个LLM驱动的重建任务，每个任务在减小正确答案空间的同时检验三元组的真实性：
  - **头实体问答任务（HEQA）**：基于部分高置信度事实作为约束，重建问题促使模型回答头实体，统计匹配比例Sh。
  - **关系重建任务（RR）**：遮盖三元组中的关系，给定头尾实体多次预测关系，统计匹配比例Sr。
  - **基于尾实体的事实三元组选择（FTSTE）**：替换尾实体为同类型候选，要求模型选择正确的三元组，统计选择原三元组的比例St。
- 最终每个三元组的综合分数F = w1·Sh + w2·Sr + w3·St + w4·Ci（公式3），用于判断是否为幻觉。

## 3. 实验设计：数据集、评估基准与对比方法
- **数据集**：
  - **PHD**（Yang et al. 2023）：300个样本，每个样本为ChatGPT生成的维基百科式短文，有全文级人工标注。
  - **WikiBio**（Manakul et al. 2023）：238个GPT3生成的段落，句子级标注，通过聚合得到段落级伪标签（95%为幻觉样本）。
  - **sub-WikiBio**：从WikiBio中平衡抽取（12个事实+48个幻觉），解决类别不均衡问题。
- **基准方法（6种）**：
  1. **RVQG**（Reverse Validation via QG）：基于重建问题反向验证。
  2. **SE**（Semantic Entropy）：基于语义熵的聚类不确定性。
  3. **SelfCk-BS**（SelfCheckGPT via BERTScore）：使用BERTScore比较一致性。
  4. **SelfCk-NLI**（SelfCheckGPT via NLI）：使用自然语言推理模型比较。
  5. **SC**（Self-contradiction）：基于提示的自相矛盾检测。
  6. **Focus**：白盒方法，利用模型内部关键token属性。
- **评估指标**：F1分数和准确率（Accuracy）。

## 4. 资源与算力
- **文中未明确说明使用的GPU型号、数量及训练时长**。
- 实验过程：使用ChatGPT（gpt-3.5-turbo）生成采样响应（温度1.0），使用GPT-4（gpt-4-1106-preview）提取三元组和进行反向验证（温度0.0确保可复现）。模型部分（RGCN、Sentence-BERT）需训练/推理，但未披露具体算力开销。

## 5. 实验数量与充分性
- **主实验**（表1）：在三个数据集上对比6种基线，报告F1和Accuracy。
- **消融实验**（表2）：共8种变体（移除RVF、移除GCCC、分别移除三个反向验证任务、移除关系信息、移除图网络），涵盖各组件贡献分析。
- **图上下文集成分析**（图2、表3）：
  - 通过t-SNE可视化比较有无RGCN的节点分布。
  - 定量计算节点表示余弦相似度（GCCC vs. 无RGCN）。
- **三元组依赖错误检测**（表4）：构造子数据集TripleCom（含依赖错误的样本），测试所有方法。
- **充分性评价**：
  - 实验覆盖了主任务、组件消融、可视化与量化分析、特定错误类型检测，设计较全面。
  - 但数据集仅三个（其中WikiBio存在严重类别不平衡），且任务仅限于维基百科式传记生成，通用性有限。
  - 对比方法覆盖了主流零资源黑盒及白盒方法，公平性较好（阈值设置统一）。

## 6. 论文的主要结论与发现
- **GCA在所有三个数据集上均优于所有基线**，包括白盒方法Focus，在PHD上F1提升约3-9个百分点，在WikiBio上F1达90.7%。
- **消融实验**证实：
  - 移除图上下文一致性比较（GCCC）或反向验证（RVF）均导致性能下降，两者互补。
  - HEQA任务效果最显著，关系信息（RGCN）和图结构均重要。
- **图上下文集成分析**：使用RGCN后，节点表示更紧凑，相似度更高，证明有效建模上下文依赖。
- **三元组依赖错误检测**：GCA显著优于其他方法（F1 92.7%），证明能捕获事实间依赖错误。
- **结论**：通过结构化三元组对齐、图网络建模依赖、多任务反向验证，可实现零资源、黑盒条件下对长文本幻觉的有效检测。

## 7. 优点
- **方法创新性**：
  - 首次利用图神经网络（RGCN）建模事实间的上下文依赖，突破一致性比较中孤立比较的局限。
  - 三元组分割与对齐有效克服词汇差异，提升比较准确性。
  - 反向验证的三个重建任务通过约束正确答案空间，减少漏检，增强检测鲁棒性。
- **零资源 & 黑盒假设**：不依赖外部知识库或模型内部状态，普适性强，可应用于GPT-4等封闭模型。
- **实验设计系统**：包含主线对比、多维度消融、可视化分析、专门错误类型测试，论证充分。

## 8. 不足与局限
- **数据集局限性**：仅使用维基百科式传记生成数据，未在问答、对话、摘要等更广泛的长文本任务上验证，泛化性存疑。
- **依赖LLM提取三元组**：三元组提取质量依赖GPT-4能力，可能引入提取错误或偏差，且成本较高。
- **未讨论计算开销**：RGCN推理和多次LLM调用（每个三元组需多次预测）的算力成本未量化，实际部署可能不高效。
- **阈值的敏感性**：一致性比较中θr、θt及权重w1~w4需手动设置，可能影响结果稳定性（论文未做灵敏度分析）。
- **类别不平衡风险**：WikiBio中95%为幻觉样本，模型可能偏向预测幻觉，sub-WikiBio虽平衡但样本量小。
- **依赖模型自检能力**：反向验证仍要求LLM在约束下正确重建，若LLM自身知识错误则可能失效。

（完）
