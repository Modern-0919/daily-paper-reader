---
title: "Seeing Is Believing: Rich-Context Hallucination Detection for MLLMs via Backward Visual Grounding"
title_zh: 眼见为实：通过反向视觉接地进行多模态大模型的丰富上下文幻觉检测
authors: "Pinxue Guo, Chongruo Wu, Xinyu Zhou, Lingyi Hong, Zhaoyu Chen, Jinglun Li, Kaixun Jiang, Sen-Ching Samson Cheung, Wei Zhang, Wenqiang Zhang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40345/44306"
tags: ["query:agent-output"]
score: 6.0
evidence: 多模态大模型的幻觉检测
tldr: 多模态大语言模型易产生幻觉，本文提出VBackChecker参考免费幻觉检测框架，通过像素级接地LLM验证生成响应与视觉输入的一致性，有效处理丰富上下文场景并提供可解释性。该方法无需外部参考，为MLLM幻觉检测提供了可靠手段。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40345/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 836, \"height\": 769, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40345/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 969, \"height\": 318, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40345/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 788, \"height\": 548, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40345/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1637, \"height\": 528, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40345/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1803, \"height\": 521, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40345/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1799, \"height\": 519, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40345/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1802, \"height\": 415, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40345/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1699, \"height\": 378, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40345/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1170, \"height\": 334, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40345/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 854, \"height\": 270, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40345/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1815, \"height\": 486, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40345/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 577, \"height\": 337, \"label\": \"Table\"}]"
motivation: 多模态大模型存在严重幻觉，需要准确的参考免费检测方法确保可靠性。
method: 提出像素级接地LLM，利用推理和指代分割验证响应与视觉输入的一致性。
result: 有效处理丰富上下文场景，提供可解释的幻觉检测结果。
conclusion: VBackChecker为多模态大模型幻觉检测提供了可靠且可解释的参考免费方案。
---

## Abstract
Multimodal Large Language Models (MLLMs) have unlocked powerful cross-modal capabilities, but still significantly suffer from hallucinations. As such, accurate detection of hallucinations in MLLMs is imperative for ensuring their reliability in practical applications. To this end, guided by the principle of “Seeing is Believing”, we introduce VBackChecker, a novel reference-free hallucination detection framework that verifies the consistency of MLLM-generated responses with visual inputs, by leveraging a pixel-level Grounding LLM equipped with reasoning and referring segmentation capabilities. This referencefree framework not only effectively handles rich-context scenarios, but also offers interpretability. To facilitate this, an innovative pipeline is accordingly designed for generating instruction-tuning data (R-Instruct), featuring richcontext descriptions, grounding masks, and hard negative samples. We further establish R 2 -HalBench, a new hallucination benchmark for MLLMs, which, unlike previous benchmarks, encompasses real-world, rich-context descriptions from 18 MLLMs with high-quality annotations, spanning diverse object-, attribute-, and relationship-level details. VBackChecker outperforms prior complex frameworks and achieves state-of-the-art performance on R^2 -HalBench, even rivaling GPT-4o’s capabilities in hallucination detection. It also surpasses prior methods in the pixel-level grounding task, achieving over a 10% improvement.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：多模态大语言模型（MLLMs）在生成丰富上下文描述时频繁出现**视觉幻觉**（hallucination），即生成内容与输入图像不符，严重威胁模型在真实场景中的可靠性与部署。
- **研究动机**：现有幻觉检测方法存在两大局限：
  - **基于参考的方法**依赖人工标注或外部专家模型（如目标检测器、密集描述模型），可扩展性差且受限于专家能力。
  - **无参考方法**（如 FaithScore）使用二元问答范式，缺乏可解释性，且需要LLM将响应分解为原子单元，计算开销大且错误传播，难以处理长文本。
- **整体含义**：亟需一种**无参考、无需外部专家、支持丰富上下文、具备可解释性**的幻觉检测框架，以提升MLLMs的实际部署可靠性。

## 2. 论文提出的方法论
### 2.1 核心思想
- **“Seeing is Believing”（眼见为实）**：如果描述的内容能在图像中**被像素级定位**（grounding），则无幻觉；否则存在幻觉。
- 将幻觉检测转化为**二元分类任务**：对MLLM生成的每个句子 \( r_i \)，判断其是否与图像 \( I \) 一致（\( y_i = 0 \) 正常，\( y_i = 1 \) 幻觉）。

### 2.2 关键技术细节
#### 2.2.1 VBackChecker 框架
- 基于**像素级接地LLM（Grounding LLM）**，融合推理与指代分割能力。
- 模型输出：当无幻觉时输出 **[SEG] token**，其嵌入被送入**掩码解码器**（类似LISA）生成物体掩码 \( m \in \mathbb{R}^{H \times W} \)；当有幻觉时输出 **[REJ] token**，同时生成自然语言解释。
- 特点：无参考、端到端、单模型、同时提供视觉与文本可解释性。

#### 2.2.2 R-Instruct 数据生成管道
- **自动四步流程**：
  1. **目标提议**：使用RAM + Grounded SAM获取候选物体及其边界框与掩码。
  2. **丰富上下文描述**：用Qwen2-VL-72B为每个物体生成详细描述（类别、属性、空间关系等）。
  3. **质量控制**：通过CLIP和SBERT进行三轮过滤：
     - 前景/背景一致性检查（前景CLIP分数 > 背景分数）
     - 视觉-文本一致性阈值（>0.5）
     - 多模态语义NMS（去重）
  4. **幻觉注入**：用Qwen2-VL对合法描述进行扰动，生成带错误属性/物体的负样本，并标注幻觉类型与解释。
- **两套数据**：
  - **R-Instruct-A**：含掩码的单个物体描述（100k正样本 + 负样本）。
  - **R-Instruct-B**：不含掩码的多物体整体描述（200k正样本 + 负样本）。
- 总计：30k图像，300k正样本，500k负样本。

#### 2.2.3 指令微调损失函数
- 总损失：\( L = L_L(t, t') + L_G(m, m') \)
  - \( L_L \)：语言损失（交叉熵，仅对响应部分计算）
  - \( L_G \)：接地损失（像素级BCE + DICE）
- **对[SEG]和[REJ] token的交叉熵赋予更高权重** \( \lambda > 1 \)（公式(3)），使模型更关注关键决策。

### 2.3 算法流程（文字描述）
1. 输入图像 \( I \) 和MLLM生成的句子 \( r_i \)。
2. 将 \( r_i \) 构建为查询：“Can you segment [描述]?”。
3. VBackChecker 输出 token——若为 [SEG]，则解码掩码，判定无幻觉；若为 [REJ]，则输出反向解释并判定存在幻觉。
4. 对每个句子重复步骤2-3，得到整段响应的幻觉检测结果。

## 3. 实验设计
### 3.1 使用数据集/场景
- **像素级接地任务**：
  - **gRefCOCO**（简单物体指代）
  - **R-Instruct-A-Val**（本文构建的丰富上下文验证集）
- **幻觉检测任务**：
  - **R²-HalBench**（本文构建的真实响应、丰富上下文基准）
    - 3000条对象描述，来自18种MLLMs（0.5B~78B，含闭源），真实MLLM输出，人工标注（三类：物体级、属性级、关系级）
    - 平均长度16.4词，最长110词，超越POPE、AMBER、MHaluBench等现有基准
  - **POPE**（简单物体查询，作为额外验证）

### 3.2 对比方法
- **像素级接地**：ReLA、LISA-7B/13B、SAM4MLLM、GSVA-7B/13B
- **幻觉检测**：
  - 闭源MLLM：GPT-4o（直接推理 / 加解释链）
  - 开源MLLM：LLaVA-OneVision-7B、Qwen2VL-7B/72B、GSVA-7B/13B
  - 度量标准：准确率（Acc）、负样本准确率（Neg-Acc）、正样本准确率（Pos-Acc），并按幻觉类型细分。

### 3.3 Benchmark特点
| 项目 | R²-HalBench 优势 |
|------|-----------------|
| 来源MLLMs | 18种（远多于MHaluBench的5种） |
| 是否真实分布 | 是（真实MLLM输出） |
| 描述长度 | 平均16.4词，最长110词 |
| 幻觉类型 | 物体、属性、关系三类全覆盖 |

## 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅提到基础模型采用LISA（LLaVa-vicuna-v1.1 + SAM），分两阶段训练：第一阶段使用LISA混合数据+gRefCOCO，第二阶段使用R-Instruct数据。未提供训练超参数与硬件配置。

## 5. 实验数量与充分性
- **实验组数**：
  - 像素级接地：2个数据集（gRefCOCO的验证/测试A/测试B + R-Instruct-A-Val）共4组结果，对比5种方法。
  - 幻觉检测：R²-HalBench主表（含16种方法/变体，分有无解释链） + POPE（5种设置），并额外按幻觉类型、模型来源、文本长度进行详细分析。
  - 消融实验：6组（移除接地损失、移除[SEG]/[REJ]权重、移除全部R-Instruct、移除R-Instruct-A/B等），覆盖所有核心模块。
  - 附加分析：按18种MLLM来源分别报告准确率，按文本长度分组报告。
- **充分性评价**：实验设计较为**充分且客观**：
  - 对比基线覆盖了闭源/开源、多种规模、有无外部专家的方法。
  - 消融实验验证了各核心组件（接地损失、token权重、丰富上下文数据）的必要性。
  - 额外分析了跨模型、跨长度的泛化能力，增强了结论可信度。
  - 但**缺乏与其他最新（2025-2026）幻觉检测方法的直接对比**（如UniHD、GAVIE等虽在表1中被引用，但未实现对比），可能存在一定选择偏差。

## 6. 主要结论与发现
1. **VBackChecker在幻觉检测上达到SOTA**：在R²-HalBench上整体准确率达62.5%，超过所有开源方法（包括FaithScore 59.3%），接近GPT-4o（68.3%带解释链），且在POPE上表现优异。
2. **像素级接地大幅改善**：在gRefCOCO上IoU达80.2%，拒绝准确率81.3%，分割准确率95.2%，超越GSVA等先前方法10%以上。
3. **可解释性强**：同时提供视觉掩码与自然语言解释，优于只能输出二元标签的方法。
4. **泛化能力强**：在18种不同MLLM来源、1~110词不同长度下性能稳定，证明其实用性。

## 7. 优点
1. **方法论创新**：首次将“反向视觉接地”思想用于幻觉检测，思路直观且有效。
2. **无参考/无外部专家**：完全端到端，无需人工标注或调用第三方模型（如搜索、目标检测），可扩展性强。
3. **显式可解释性**：失败时输出[REJ] token+文本解释，成功时输出分割掩码，双模态解释。
4. **数据生成管道巧妙**：自动构造了大规模、高质量、带硬负样本的丰富上下文指令数据（R-Instruct），包含多种质量控制机制（CLIP+SBERT+NMS）。
5. **新基准构建严谨**：R²-HalBench由18种真实MLLM输出+人工标注（三人投票）组成，覆盖多种幻觉类型和长度，弥补了现有基准偏简单/偏短的不足。
6. **消融实验全面**：证实了接地损失、特殊token权重、完整R-Instruct数据（含A和B）均不可或缺。

## 8. 不足与局限
1. **算力信息缺失**：未报告训练硬件、GPU数量、训练时间，不利于复现与成本估计。
2. **实验对比不够广泛**：表1列举了HaELM、AMBER、UniHD、GAVIE等方法，但主实验中仅与FaithScore和GSVA等有限对比，未实现与其他最新参考免费方法的公平对比，可能存在“挑软柿子捏”的偏差。
3. **基准本身局限性**：R²-HalBench仅基于静态图像（SA1B），未涵盖视频、多轮对话等更复杂场景；标注仅检查单个对象描述，未评估整段响应的全局连贯性幻觉。
4. **任务转化假设**：将每个句子独立评估，忽略了句子之间的上下文依赖性（例如：两个句子描述同一物体可能冲突）。
5. **对长文本性能波动**：在31-50词区间准确率63.7%，但在51-110词区间下降至59.1%，说明模型在处理极长描述时仍存在挑战。
6. **方法论迁移性**：依赖于一个可区分[SEG]/[REJ]的Grounding LLM，未探讨在缺乏指代分割能力的MLLM上如何泛化。
7. **公开范围**：代码与数据在GitHub开源（https://github.com/PinxueGuo/VBackChecker），但具体许可协议未提及。

（完）
