---
title: "Rethinking the Reliability of Multi-agent System: A Perspective from Byzantine Fault Tolerance"
title_zh: 重新思考多智能体系统的可靠性：拜占庭容错视角
authors: "Lifan Zheng, Jiawei Chen, Qinghong Yin, Jingyuan Zhang, Xinyi Zeng, Yu Tian"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40806/44767"
tags: ["query:agent-errors"]
score: 5.0
evidence: 从拜占庭容错角度研究LLM智能体的可靠性
tldr: 本文从拜占庭容错视角研究LLM智能体系统的可靠性，通过实验发现LLM智能体表现出更强的怀疑机制，能够更好地识别故障代理。该研究为理解LLM智能体在故障场景下的行为提供了理论基础，但与具体的错误检测或修复方法关联较弱。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40806/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 878, \"height\": 502, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40806/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1837, \"height\": 278, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40806/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1759, \"height\": 1004, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40806/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1741, \"height\": 1006, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40806/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1837, \"height\": 278, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40806/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1833, \"height\": 439, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40806/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1724, \"height\": 782, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40806/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 866, \"height\": 585, \"label\": \"Table\"}]"
motivation: LLM智能体取代传统代理后可靠性影响尚不明确。
method: 采用拜占庭容错理论量化LLM智能体可靠性。
result: 发现LLM智能体表现出更强的怀疑机制。
conclusion: 为设计更可靠的LLM多智能体系统提供见解。
---

## Abstract
Ensuring the reliability of agent architectures and effectively identifying problematic agents when failures occur are crucial challenges in multi-agent systems (MAS). Advances in large language models (LLMs) have established LLM-based agents as a major branch of MAS, enabling major breakthroughs in complex problem solving and world modeling. However, the reliability implications of this shift remain largely unexplored. i.e., whether substituting traditional agents with LLM-based agents can effectively enhance the reliability of MAS. In this work, we investigate and quantify the reliability of LLM-based agents from the perspective of Byzantine fault tolerance. We observe that LLM-based agents demonstrate stronger skepticism when processing erroneous message flows, a characteristic that enables them to outperform traditional agents across different topological structures. Motivated by the results of the pilot experiment, we design CP-WBFT, a confidence probe-based weighted Byzantine Fault Tolerant consensus mechanism to enhance the stability of MAS with different topologies. It capitalizes on the intrinsic reflective and discriminative capabilities of LLMs by employing a probe-based, weighted information flow transmission method to improve the reliability of LLM-based agents. Extensive experiments demonstrate that CP-WBFT achieves superior performance across diverse network topologies under extreme Byzantine conditions (85.7 % fault rate). Notably, our approach surpasses traditional methods by attaining remarkable accuracy on various topologies and maintaining strong reliability in both mathematical reasoning and safety assessment tasks.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：多智能体系统（MAS）的可靠性至关重要，但传统代理在面临故障（尤其是拜占庭故障）时容易崩溃。大语言模型（LLM）驱动的代理已成为MAS主流，但LLM代理替换传统代理后是否能提升系统可靠性尚不清楚。
- **整体含义**：本文从拜占庭容错（Byzantine Fault Tolerance, BFT）视角，首次系统性地量化了LLM代理在不同网络拓扑下的可靠性，并基于此设计新的容错机制，推动更可靠的LLM-MAS构建。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：利用LLM内在的反思与判别能力，通过置信度探针评估代理的可信度，并设计加权拜占庭共识协议，使高置信度的代理在信息聚合中具有更大权重，从而在极端故障率下仍能达成一致正确结果。
- **关键技术细节**：
  - **两阶段置信度探针**：
    - **Prompt-Level Confidence Probe (PCP)**：通过结构化提示（如“请提供答案和置信度[0,1]”）从LLM输出中直接提取置信度分数。
    - **Hidden-Level Confidence Probe (HCP)**：从LLM隐藏层（如LLaMA3-8B-Instruct的第16层）提取隐藏状态，经PCA降维后训练线性逻辑回归分类器，输出置信度。
  - **加权拜占庭共识协议（CP-WBFT）**：
    - Step 1：每个代理根据从邻居收到的更高置信度答案，更新自身决策（若邻居置信度高于自身则采纳）。
    - Step 2：聚合所有代理的最终决策，选择具有最高平均置信度的答案作为共识结果，置信度相同时以支持者数量为决胜项。
  - **公式化描述**：式(4) —— \( R = \arg\max_r \left( \frac{1}{|A_r|} \sum_{i \in A_r} C_i^{\text{final}}(x), |A_r| \right) \)。

## 3. 实验设计

- **数据集/场景**：
  - 数学推理：**GSM8K**（10道精心挑选的题，使强弱模型表现产生差距）。
  - 安全评估：**XSTest**（10道题，检测夸大安全行为）。
  - 额外实验：CommonSenseQA（附录）。
- **Benchmark与对比方法**：
  - 对比传统代理（硬编码/规则型）与LLM代理。
  - 对比LLM代理中使用不同的置信度提取方法：PCP vs HCP。
  - 对比不同网络拓扑结构（链状、星形、树形、完全图、随机图、分层图）。
  - 对比不同攻击位置（中心节点 vs 叶子节点）。
  - 对比初始精度（IAA）、最终精度（FAA）、拜占庭容错提升（BFTI）、轮次精度（RA）。
- **基线**：传统BFT（f < n/3约束）；无容错机制的LLM代理。

## 4. 资源与算力

- **资源说明**：文中未明确给出使用的GPU型号、数量、训练时长。仅提及模型：LLaMA3-8B-Instruct / LLaMA3.1-8B-Instruct（开源自部署），GPT-4o-mini / GPT-3.5-turbo（API调用）。对于HCP训练，使用线性逻辑回归（scikit-learn），计算开销较低。对于PCP，仅调用API。**论文未披露具体算力资源**。

## 5. 实验数量与充分性

- **实验数量**：
  - 主要实验：2个数据集 × 6种拓扑 × 2种探针（PCP/HCP） × 10个问题（每个问题一轮共识） = 240组运行（+变体）。
  - 消融实验：隐藏层选择（不同层）、表示类型（query/answer/pooled）、特征聚合策略等。
  - 额外实验：不同故障率（1-6个拜占庭节点）、攻击位置敏感性（中心/叶子）、更大网络规模（附录）。
  - 每个结果采用多次均值，但论文未明确说明重复次数。
- **充分性与公平性**：
  - 覆盖了多种拓扑结构和极端故障率（85.7%），超过了经典BFT理论极限。
  - 强弱模型配对合理（GPT-4o-mini vs GPT-3.5-turbo，LLaMA3.1 vs LLaMA3），客观模拟了代理水平差异。
  - 实验设计较为全面，但样本量较小（每拓扑仅10题），可能存在统计波动风险。

## 6. 主要结论与发现

1. **LLM代理比传统代理更可靠**：即使在6/7节点为拜占庭时，LLM代理仍能维持较高精度，而传统代理在2-3个恶意节点后即崩溃。
2. **HCP显著优于PCP**：HCP在所有拓扑上实现100%轮次精度（RA），而PCP在某些拓扑（如星形中心恶意）下效果差。
3. **拓扑影响极大**：完全图和星形（叶节点恶意）表现最好；链式和星形（中心恶意）表现最差。
4. **任务特性影响**：数学推理（GSM8K）对不同拓扑较鲁棒；安全评估（XSTest）对拓扑高度敏感，但HCP仍能克服。
5. **置信度加权共识有效**：CP-WBFT能在极端拜占庭条件下达到与完全诚实系统一样的准确率。

## 7. 优点

- **视角新颖**：首次将拜占庭容错理论与LLM代理可靠性结合，提供了量化评估框架。
- **方法设计巧妙**：提出两种置信度探针（PCP和HCP），并设计加权共识协议，充分利用LLM的反思能力。
- **实验系统全面**：覆盖6种拓扑、多种攻击场景、两种任务，且考虑了中心节点攻击等敏感情况。
- **结果显著**：HCP在完全图和星形叶节点下达到100%准确率，BFTI提升达85.71%。
- **代码开源**：便于复现和后续研究。

## 8. 不足与局限

- **实验规模较小**：每个数据集仅10个问题，统计显著性不足；网络节点固定为7，未充分探索大规模多代理系统。
- **缺乏真实部署验证**：仅在模拟环境中运行，未考虑通信延迟、网络分区、恶意重放等现实问题。
- **HCP依赖模型内部表示**：对不开源模型（如GPT系列）无法直接使用，适用范围受限。
- **PCP效果不稳定**：安全任务上PCP甚至出现负BFTI，说明提示级置信度提取不够鲁棒。
- **未对比其他BFT协议**：仅与传统无容错代理比较，未与PBFT、Raft等经典算法对比。
- **计算开销**：多轮交互学习需要邻居信息交换，实际系统可能产生高通信成本，但本文未量化。
- **仅考虑硬恶性攻击**：假设所有拜占庭节点故意提供错误答案，未考虑更微妙的策略（如偶尔正确以欺骗系统）。

（完）
