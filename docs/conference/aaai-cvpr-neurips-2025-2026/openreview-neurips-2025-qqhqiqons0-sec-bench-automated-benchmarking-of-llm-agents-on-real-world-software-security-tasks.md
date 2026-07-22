---
title: "SEC-bench: Automated Benchmarking of LLM Agents on Real-World Software Security Tasks"
title_zh: SEC-bench：LLM智能体在真实软件安全任务上的自动化基准
authors: "Hwiwon Lee, Ziqi Zhang, Hanxiao Lu, LINGMING ZHANG"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=QQhQIqons0"
tags: ["query:agent-traj"]
score: 7.0
evidence: 用于评估LLM智能体在安全任务中轨迹的基准
tldr: 针对现有安全基准依赖合成或简化漏洞的问题，提出SEC-bench自动化基准框架，通过多智能体脚手架构建真实代码仓库并复现漏洞，可系统评估智能体在安全任务中的完整轨迹，为安全部署提供可靠评价。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有安全基准过于简化，无法反映真实安全任务的复杂性。
method: 设计多智能体脚手架自动构建含漏洞的代码仓，并生成金标准补丁。
result: 在多个LLM上展示了基准的有效性，可区分不同智能体的安全能力。
conclusion: SEC-bench为评估LLM智能体在真实安全任务中的表现提供了标准化平台。
---

## Abstract
Rigorous security-focused evaluation of large language model (LLM) agents is imperative for establishing trust in their safe deployment throughout the software development lifecycle. However, existing benchmarks largely rely on synthetic challenges or simplified vulnerability datasets that fail to capture the complexity and ambiguity encountered by security engineers in practice. We introduce SEC-bench, the first fully automated benchmarking framework for evaluating LLM agents on authentic security engineering tasks. SEC-bench employs a novel multi-agent scaffold that automatically constructs code repositories with harnesses, reproduces vulnerabilities in isolated environments, and generates gold patches for reliable evaluation. Our framework automatically creates high-quality software vulnerability datasets with reproducible artifacts at a cost of only $0.87 per instance. Using SEC-bench, we implement two critical software security tasks to rigorously evaluate LLM agents' capabilities: proof-of-concept (PoC) generation and vulnerability patching. A comprehensive evaluation of state-of-the-art LLM code agents reveals significant performance gaps, achieving at most 18.0% success in PoC generation and 34.0% in vulnerability patching on our complete dataset. These results highlight the crucial steps needed toward developing LLM agents that are more practical, intelligent, and autonomous for security engineering.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有针对大语言模型（LLM）智能体的安全评测基准大多基于合成或过度简化的漏洞数据集，无法反映真实世界中安全工程师所面临的复杂性和歧义性，导致评估结果难以指导实际安全部署。
- **研究动机**：为了建立对LLM智能体在软件开发生命周期中安全部署的信任，亟需一种能够真实反映真实安全工程任务难度的自动化评测框架。
- **整体含义**：论文提出了SEC-bench，首个全自动基准框架，专注于评估LLM智能体在**真实**软件安全任务（如概念验证生成、漏洞修补）中的表现，从而填补现有基准与现实需求之间的鸿沟。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：通过多智能体协作脚手架，自动构建带有测试夹具（harnesses）的代码仓库，在隔离环境中复现漏洞，并生成黄金标准补丁（gold patches），以此作为可靠评估的依据。
- **关键技术细节**：
  - **多智能体脚手架（multi-agent scaffold）**：自动完成以下步骤：
    1. 从真实软件项目中提取漏洞相关信息，自动构建包含漏洞的可复现代码仓库。
    2. 在沙盒环境中重现漏洞，确保评估环境一致且可控。
    3. 自动生成正确修复的黄金补丁，作为评判智能体漏洞修补能力的参考标准。
  - **任务设计**：实现两个关键安全任务：
    - **概念验证生成（PoC generation）**：要求智能体生成能触发漏洞的输入或步骤。
    - **漏洞修补（vulnerability patching）**：要求智能体生成修复代码。
  - **评估自动化**：整个流程无需人工干预，每个实例的构建成本仅约0.87美元。

### 3. 实验设计：使用的数据集/场景、基准、对比方法

- **数据集/场景**：论文通过SEC-bench框架自动生成了包含真实软件漏洞的数据集（具体构成未详述，但强调每个实例都对应一个真实仓库中的漏洞）。数据集覆盖了多种漏洞类型和真实项目。
- **基准（Benchmark）**：SEC-bench本身即作为评估基准，提供了标准化的任务定义（PoC生成和漏洞修补）和自动化的评分机制（基于金标准补丁等）。
- **对比方法**：评估了当前最先进的多个LLM代码智能体（state-of-the-art LLM code agents），但摘要中未列出具体模型名称。实验通过对比这些智能体在相同任务上的成功率来衡量性能差异。

### 4. 资源与算力

- **文中说明**：论文摘要及元数据中**未明确提及**具体的GPU型号、数量、训练时长等算力信息。仅提到基准构建成本为每个实例0.87美元（可能包含算力开销），但未详细说明评估时消耗的算力资源。

### 5. 实验数量与充分性

- **实验数量**：摘要中仅给出了两个任务的总体成功率（PoC生成最高18%，漏洞修补最高34%），但未提供具体实验组数、消融实验或不同智能体表现的细分数据。推测实验涵盖了多个LLM智能体，但细节不足。
- **充分性与客观性**：
  - **优点**：框架全自动，避免人工偏差；使用真实漏洞，生态效度高。
  - **不足**：由于摘要篇幅限制，未展示实验的重复次数、统计显著性检验、不同参数影响等，无法判断实验的全面性。元数据中标记“evidence: 用于评估LLM智能体在安全任务中轨迹的基准”，表明论文可能还包含轨迹分析，但摘要未涉及。总体而言，实验设计合理但披露信息有限，需要阅读全文确认充分性。

### 6. 论文的主要结论与发现

- 当前最先进的LLM代码智能体在真实安全任务上表现**显著不足**：PoC生成成功率最高仅18.0%，漏洞修补成功率最高仅34.0%。
- 性能差距表明现有智能体在处理真实世界安全工程复杂性时仍有很大局限，需要发展更实用、更智能、更自主的LLM安全智能体。
- SEC-bench作为标准化平台，能够有效区分不同LLM智能体的安全能力，为后续研究提供了可靠的评测基础。

### 7. 优点：方法或实验设计上的亮点

- **全自动化**：从数据集构建到评估完全自动化，可规模化扩展，且成本低廉（$0.87/实例）。
- **真实性**：基于真实软件漏洞和代码仓库，避免了合成数据的不匹配风险。
- **可复现性**：在隔离环境中复现漏洞并生成金标准补丁，确保评估结果可重复。
- **双任务设计**：同时评估攻击（PoC生成）和防御（漏洞修补）两种关键安全能力，覆盖安全工程核心环节。
- **多智能体脚手架**：创新地利用多个LLM协作自动完成数据构建，减少人工标注需求。

### 8. 不足与局限

- **实验覆盖有限**：仅公开了两个任务的整体成功率，未展示不同漏洞类型、不同项目、不同智能体变量下的详细结果，也未进行消融实验（如脚手架设计的影响）。
- **偏差风险**：虽然使用真实漏洞，但自动构建过程可能引入选择偏差（如仅对易于自动复现的漏洞有效），且金标准补丁可能不唯一或与人工专家补丁有差异。
- **应用限制**：框架当前仅支持PoC生成和漏洞修补，未涵盖更广泛的安全任务（如安全需求分析、威胁建模等）。另外，评估可能忽略智能体在真实环境中的交互性能（如时间、资源消耗等）。
- **算力信息缺失**：未提供评估所需计算资源，影响可复现性和成本估算。
- **数据公开性**：未说明数据集是否公开，若仅用于内部实验，则限制了外部验证。

（完）
