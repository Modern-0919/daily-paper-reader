---
title: "RCAFlow: A Workflow-Informed Hierarchical Planning Multi-Agent System for Root Cause Analysis"
title_zh: RCAFlow：面向根因分析的工作流感知层级规划多智能体系统
authors: "Yufei Gao, Zhengong Cai, Bowei Yang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/36991/40953"
tags: ["query:agent-errors"]
score: 8.0
evidence: 利用工作流感知的分层规划进行根因分析，实现故障归因
tldr: RCAFlow提出了一个多智能体系统，通过集成结构化工作流知识和分层规划来执行根因分析，克服了现有方法搜索空间大、工作流复杂的问题。该工作直接针对故障归因和错误传播，适用于多步LLM智能体工作流中的失败分析。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36991/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1832, \"height\": 1073, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-36991/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 835, \"height\": 835, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-36991/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1558, \"height\": 635, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-36991/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 806, \"height\": 359, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-36991/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 883, \"height\": 181, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-36991/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 682, \"height\": 182, \"label\": \"Table\"}]"
motivation: 微服务架构下根因分析面临灵活性和可解释性不足。
method: 结合工作流知识和分层规划的多智能体框架。
result: 提升了诊断透明度与推理能力。
conclusion: 为智能体工作流中的错误归因提供了有效框架。
---

## Abstract
As microservice architectures become increasingly complex and system events become more frequent, Root Cause Analysis (RCA) has emerged as a critical task to ensure system reliability. However, existing deep learning-based methods often struggle with limited flexibility and a lack of interpretability when addressing complex system failures. Recent efforts to integrate large language models (LLMs) have shown promise in enhancing diagnostic transparency and reasoning capability. However, expansive search spaces, intricate workflows, and entangled constraints constrain practical adoption. We propose RCAFlow, a multi-agent framework that integrates structured workflow knowledge with hierarchical planning to address these challenges. RCAFlow transforms semi-structured documents into behavior tree-style workflows to support interpretable plan generation, employs a Git-inspired branching mechanism for modular and hierarchical task execution with path isolation, and leverages state-aware task execution with semantic analysis to improve result understanding and feedback. We evaluate RCAFlow on three benchmark datasets provided by OpenRCA. Experimental results demonstrate that RCAFlow consistently outperforms existing methods across all datasets. Further ablation studies confirm the effectiveness of each core module, highlighting the reliability, extensibility, and interpretability of RCAFlow to support complex RCA tasks within intelligent IT operations.

---

## 论文详细总结（自动生成）

### 论文总结：RCAFlow

#### 1. 核心问题与整体含义（研究动机和背景）
- **问题**：微服务架构日益复杂，系统事件频繁，根因分析（RCA）成为保障可靠性的关键。现有基于深度学习的方法（如因果发现、图神经网络）灵活性和可解释性不足，难以适应系统演变和新故障模式。基于大语言模型（LLM）的方法虽有前景，但面临搜索空间过大、工作流复杂、约束交错等实际挑战。
- **含义**：亟需一种能结合结构化领域知识（如故障排查指南TSG、标准操作流程SOP）与LLM自主规划能力的框架，以提升RCA的准确性、可解释性和适应性。

#### 2. 方法论：核心思想、关键技术细节
- **核心思想**：RCAFlow是一个多智能体框架，核心是**将结构化工作流知识**（行为树形式）与**分层规划**相结合，通过Git式分支管理实现模块化、解耦的推理执行。
- **关键技术细节**：
  - **工作流知识库（K）**：由三部分组成：
    - 操作流程（Kp）：行为树控制节点（Sequence、Selector、Parallel）组织任务步骤。
    - 条件/规则（Kr）：定义前置条件和异常处理。
    - 工具/数据（Kt）：封装原子操作API。
  - **自动化工作流生成**：利用LLM从半结构化文档（SOP/TSG）提取任务元素、控制逻辑和工具元数据，生成JSON格式行为树。
  - **分层规划与执行**：
    - `Planner`：基于当前任务和语义检索，生成行为树样式的计划路径π。
    - `Coordinator`：解析计划，评估步骤复杂度，将复杂步骤派发给`Brancher`创建子分支，简单步骤直接分配给`Executor`。
    - `Brancher`：Git式分支管理，每个子任务独立分支（隔离执行、共享工作空间），完成后合并回父分支。
    - `Executor`：调用预定义工具或动态生成代码执行原子任务。
    - `State-Aware Analyzer`：对执行结果进行多维评估（上下文关联、完整性指标如异常数量百分比），反馈给`Coordinator`用于路径调整。
  - **自我反思**：任务完成后对推理轨迹进行事后分析，将经验存入内存Mr。
- **公式**：系统表示为ΣA = (A, K, O, M, W)。计划生成：ŝij = Planner(Kp, sij, Wi)。工具执行：aij = Executor(Kt, sij, Wij)。

#### 3. 实验设计
- **数据集**：来自OpenRCA的**三个真实企业系统数据集**：Bank（银行系统）、Market（在线零售系统）、Telecom（电信数据库系统）。包含大规模多模态观测数据（日志、指标、链路追踪，总容量超68GB）。
- **基准指标**：**正确率（Correct）**（所有根因要素完全匹配）、**部分正确率（Partial）**（至少一个要素正确）。
- **对比方法**：
  - 基于LLM的基线：CoT、ReAct、RCA-agent、mABC、Flow-of-Action。
  - 骨干模型：GPT-4o、DeepSeek-V3。
  - 所有方法均使用相同的故障规则知识库以保证公平。

#### 4. 资源与算力
- 论文**未明确说明**所使用的GPU型号、数量、训练时长等算力资源。仅提及实验环境，未提供具体硬件配置。

#### 5. 实验数量与充分性
- **主实验**：在三个数据集上分别以两种骨干模型进行全指标对比（共6组主结果）。
- **消融实验**：五种变体（w/o K、w/o Planner、w/o Coordinator、w/o Analyzer、w/o Reflection）在三个数据集上测试正确率（共15组）。
- **阈值θ分析**：在Bank数据集上测试不同θ对正确率和平均路径长度的影响。
- **工作流敏感性分析**：初始工作流增强实验（两个数据集）、LLM生成工作流的稳定性实验（3次重复）。
- **充分性评价**：实验设计全面，从多个维度验证各模块必要性，基线覆盖主流方法，骨干模型多样性增加泛化性证据。但缺少与更多最新RCA智能体方法（如AgentFM）的对比，且仅在三个数据集上评估，可能存在场景局限性。

#### 6. 论文的主要结论与发现
- RCAFlow在**所有数据集和两种骨干模型上均取得最高正确率和部分正确率**，显著优于现有方法（例如Bank数据集上正确率41.91% vs. 最佳基线27.94%）。
- 消融实验表明，**工作流知识库和分支协调机制**是关键贡献模块（移除后性能下降最大），而反思和分析器贡献虽小但不可或缺。
- 阈值θ在0.90附近平衡效率与准确；工作流质量提升可直接带来性能增益；LLM生成工作流程具有低方差、稳定性良好。

#### 7. 优点
- **创新性整合**：首次将行为树式结构化工作流与分层规划引入LLM-based RCA，兼顾专家知识引导与自主规划。
- **Git式分支管理**：实现推理路径隔离与模块化执行，避免状态污染，支持可追溯和可回滚。
- **状态感知执行**：通过Analyzer实现执行结果的多维评估，动态调整策略，减少冗余和遗漏。
- **强可解释性**：生成完整推理轨迹，且工作流结构本身具有可读性。
- **通用性强**：两种骨干模型均表现优异，且可扩展至不同领域。

#### 8. 不足与局限
- **算力成本未量化**：未报告训练/推理所需资源，难以评估部署可行性。
- **工作流生成依赖LLM**：对复杂SOP/TSG的自动提取可能不准确，论文未深入分析该限制。
- **阈值θ需要手动调优**：系统对阈值较敏感（实验显示过高/过低均导致性能下降），缺乏自适应机制。
- **实验覆盖有限**：仅三个数据集，且均为开源基准，未在真实生产环境大规模部署验证；未与AgentFM等近期多智能体方法对比。
- **推理质量分析缺失**：未深入评估推理路径的因果合理性或错误步数，仅有正确率指标。

（完）
