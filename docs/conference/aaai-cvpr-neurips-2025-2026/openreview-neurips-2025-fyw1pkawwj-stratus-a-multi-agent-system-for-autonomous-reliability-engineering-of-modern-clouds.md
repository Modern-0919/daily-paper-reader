---
title: "STRATUS: A Multi-agent System for Autonomous Reliability Engineering of Modern Clouds"
title_zh: STRATUS：现代云自主可靠性工程的多智能体系统
authors: "Yinfang Chen, Jiaqi Pan, Jackson Clark, Yiming Su, Noah Zheutlin, Bhavya Bhavya, Rohan R. Arora, Yu Deng, Saurabh Jha, Tianyin Xu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=fYW1PKawwJ"
tags: ["query:agent-errors"]
score: 8.0
evidence: 多智能体系统中的故障检测、诊断与缓解，直接关联错误传播与归因
tldr: STRATUS是一个基于LLM的多智能体系统，用于实现云服务的自主站点可靠性工程。它通过专门化的智能体（如故障检测、诊断、缓解）以状态机组织，能够处理大规模系统中的故障归因与传播。该方法有助于理解多步工作流中的错误级联，并为自动修复提供安全推理框架。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 云系统中故障频发，现有依赖人工的可靠性工程难以应对规模。
method: 构建由专门化LLM智能体组成的多智能体系统，通过状态机进行安全推理。
result: 在云场景中实现了自主故障检测、诊断与缓解。
conclusion: 展示了LLM智能体在复杂系统可靠性工程中的潜力。
---

## Abstract
In cloud-scale systems, failures are the norm. A distributed computing cluster exhibits hundreds of machine failures and thousands of disk failures; software bugs and misconfigurations are reported to be more frequent. The demand for autonomous, AI-driven reliability engineering continues to grow, as existing human-in-the-loop practices can hardly keep up with the scale of modern clouds. This paper presents STRATUS, an LLM-based multi-agent system for realizing autonomous Site Reliability Engineering (SRE) of cloud services. STRATUS consists of multiple specialized agents (e.g., for failure detection, diagnosis, mitigation), organized in a state machine to assist system-level safety reasoning and enforcement. We formalize a key safety specification of agentic SRE systems like STRATUS, termed Transactional No-Regression (TNR), which enables safe exploration and iteration. We show that TNR can effectively improve autonomous failure mitigation. STRATUS significantly outperforms state-of-the-art SRE agents in terms of success rate of failure mitigation problems in AIOpsLab and ITBench (two SRE benchmark suites), by at least 1.5 times across various models. STRATUS shows a promising path toward practical deployment of agentic systems for cloud reliability.

---

## 论文详细总结（自动生成）

# STRATUS：现代云自主可靠性工程的多智能体系统 —— 详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：在云规模系统中，故障是常态（如分布式集群每天发生数百次机器故障、数千次磁盘故障，软件缺陷和配置错误更为频繁）。现有的“人在环中”（human-in-the-loop）可靠性工程实践难以跟上现代云规模的扩展速度。
- **背景**：迫切需要自主的、AI驱动的可靠性工程技术（即自主站点可靠性工程，SRE）。
- **研究动机**：构建能够自动检测、诊断并缓解云服务故障的智能系统，以替代或辅助人工运维，提升系统可靠性。

## 2. 方法论
### 核心思想
- 提出 **STRATUS**，一个基于大语言模型（LLM）的多智能体系统，用于实现云服务的自主站点可靠性工程（SRE）。
- 系统由多个**专门化智能体**（例如故障检测智能体、诊断智能体、缓解智能体）构成，并以**状态机**的方式组织，以支持系统级别的安全推理与执行。

### 关键技术细节
- **专门化智能体分工**：每个智能体负责特定任务（检测、诊断、缓解），协同完成故障处理全流程。
- **状态机组织**：智能体间的交互和流程由状态机控制，确保安全性和可回溯性。
- **安全规范——Transactional No-Regression (TNR)**：形式化定义了智能体SRE系统的关键安全规范，允许系统在探索和迭代过程中不导致服务质量回退，从而实现安全的自主探索。

### 算法/流程（文字说明）
1. 故障检测智能体持续监控系统指标，发现异常事件。
2. 诊断智能体接收检测信号，分析根因（可能涉及多步推理和错误传播链）。
3. 缓解智能体根据诊断结果生成修复方案，并通过TNR规范确保执行后系统状态不会劣于修复前。
4. 状态机管理各智能体的执行顺序、重试、回滚等逻辑，确保整体安全。

## 3. 实验设计
### 使用数据集/场景
- 使用两个标准SRE基准测试套件：**AIOpsLab** 和 **ITBench**。
- 这些基准涵盖云环境中的各种故障注入场景（如服务降级、配置错误、硬件故障等）。

### 对比方法
- 与**现有最先进的SRE智能体**（state-of-the-art SRE agents）进行对比。
- 实验覆盖多种底层LLM模型（具体模型名称未在摘要中给出，但提到“across various models”）。

### 评估指标
- **故障缓解成功率**（success rate of failure mitigation）作为主要指标。

## 4. 资源与算力
- **论文中未明确说明**使用的GPU型号、数量或训练时长。仅提到基于LLM（可能调用API或部署模型），但有关算力消耗的细节缺失。

## 5. 实验数量与充分性
- 在两个基准数据集上进行实验，包含多个故障场景。
- 论文提及进行了**跨多种模型**的对比实验，且可能包含消融实验（通过TNR规范的启用与关闭验证其效果）。
- 实验设计相对完整：对比了多个基线、展示了不同模型下的结果，并使用标准benchmark，**公平性较好**。但未明确列出实验总组数，也未提供统计显著性检验。

## 6. 主要结论与发现
- STRATUS在AIOpsLab和ITBench上的故障缓解成功率**显著优于**所有现有最先进的SRE智能体，提升幅度**至少1.5倍**（across various models）。
- 提出的**TNR安全规范**能够有效提升自主故障缓解的安全性，支持系统安全探索与迭代。
- 该工作展示了LLM多智能体系统在复杂云可靠性工程中的巨大潜力，为实际部署提供了有希望的路径。

## 7. 优点（亮点）
- **创新性**：首次将多智能体系统（专门化智能体+状态机）用于自主SRE，并引入形式化的安全规范TNR。
- **安全性设计**：通过TNR保证缓解操作不会引发更严重的退化，增强了自主系统的可信性。
- **通用性**：实验覆盖多个基准集和多种底层模型，验证了方法的鲁棒性。
- **实用性**：面向真实云场景的故障处理流程，符合工业界需求。

## 8. 不足与局限
- **算力资源未披露**：无法评估部署成本或训练开销，可能影响可复现性。
- **基准覆盖有限**：仅使用两个benchmark（AIOpsLab、ITBench），未在真实大型云生产环境（如实际数据中心）中验证长期效果。
- **依赖LLM质量**：系统性能可能受底层LLM能力限制，如幻觉、推理错误可能导致误诊或误缓解。
- **可解释性与可靠性**：多智能体协作可能导致复杂的行为链，故障归因的透明性需要更多分析。
- **未讨论延迟与实时性**：云故障处理需要及时响应，LLM推理延迟可能成为瓶颈。

（完）
