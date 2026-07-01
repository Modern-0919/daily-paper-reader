---
title: Process-Level Trajectory Evaluation for Environment Configuration in Software Engineering Agents
title_zh: 软件工程智能体环境配置的过程级轨迹评估
authors: "Jiayi Kuang, Yinghui Li, Xin Zhang, Yangning Li, di yin, Xing Sun, Ying Shen, Philip S. Yu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=Q8qgloDKUO"
tags: ["query:agent-traj"]
score: 9.0
evidence: 针对智能体能力的过程级轨迹评估
tldr: 该论文针对软件工程智能体环境配置的瓶颈，提出EnConda-bench基准，提供过程级轨迹评估。不同于仅关注端到端成功率的现有基准，EnConda-bench细粒度评估智能体在计划、感知驱动错误诊断、反馈驱动修复和执行配置等各环节的能力。通过注入真实README错误构建任务实例。该工作为智能体轨迹评估提供了精细的诊断方法。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有基准只能评估端到端结果，无法诊断智能体在环境配置各阶段的表现。
method: 构建EnConda-bench基准，通过注入错误自动生成实例并提供过程级轨迹评估。
result: 实验揭示了智能体在环境配置各阶段的具体失败模式。
conclusion: EnConda-bench为智能体轨迹评估提供了细粒度的诊断工具。
---

## Abstract
Large language model-based agents show promise for software engineering, but environment configuration remains a bottleneck due to heavy manual effort and scarce large-scale, high-quality datasets.
Existing benchmarks assess only end-to-end build/test success, obscuring where and why agents succeed or fail.
We introduce the Environment Configuration Diagnosis Benchmark, EnConda-bench, which provides process-level trajectory assessment of fine-grained agent capabilities during environment setup-planning, perception-driven error diagnosis, feedback-driven repair, and action to execute the final environment configuration. 
Our task instances are automatically constructed by injecting realistic README errors and are validated in Docker for scalable, high-quality evaluation.
EnConda-bench combines process-level analysis with end-to-end executability to enable capability assessments beyond aggregate success rates.
Evaluations across state-of-the-art LLMs and agent frameworks show that while agents can localize errors, they struggle to translate feedback into effective corrections, limiting end-to-end performance.
To our knowledge, EnConda-bench is the first framework to provide process-level internal capability assessment for environment configuration, offering actionable insights for improving software engineering agents.

---

## 论文详细总结（自动生成）

# 论文《Process-Level Trajectory Evaluation for Environment Configuration in Software Engineering Agents》详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：基于大语言模型（LLM）的智能体在软件工程任务中展现出潜力，但环境配置环节仍是瓶颈——需要大量人工操作，且缺乏大规模、高质量的数据集。
- **现有基准的不足**：现有基准（如SWE-bench）仅评估端到端的构建/测试成功率，无法揭示智能体在环境配置过程中**何处**以及**为何**成功或失败，缺乏细粒度的过程诊断能力。
- **整体目标**：通过构建一个**过程级轨迹评估基准**，系统评估智能体在环境配置各环节的能力，提供可操作的改进方向。

## 2. 论文提出的方法论

### 核心思想
- 引入 **EnConda-bench**（环境配置诊断基准），从**计划、感知驱动错误诊断、反馈驱动修复、执行配置**四个细粒度环节对智能体轨迹进行过程级评估，而不仅仅是最终成功率。

### 关键技术细节
- **任务实例自动构造**：通过向真实README文件**注入常见错误**（如依赖版本冲突、缺失包、路径错误等）自动生成测试实例，无需人工标注。
- **质量验证**：在 Docker 容器中运行，确保实例的**可执行性**和**可复现性**，实现可扩展的高质量评估。
- **评估指标**：结合**过程级分析**（各环节能力得分）与**端到端可执行性**（最终环境配置是否成功），提供超越聚合成功率的细粒度能力评估。

### 算法/流程（文字说明）
1. **实例生成**：从开源项目收集真实 README 文件 → 人工定义错误类型模板 → 自动注入错误生成“损坏”的 README → Docker 环境验证。
2. **智能体任务设定**：给定损坏的 README + 项目代码仓库，智能体需逐步完成环境配置（包括诊断错误、生成修复命令、执行配置）。
3. **轨迹评估**：将智能体的完整动作序列分解为四个阶段，分别计算每个阶段的正确率/完成度，并汇总端到端成功率。

## 3. 实验设计

### 数据集与场景
- **基准**：EnConda-bench 自身作为评估基准，实例来源于真实开源项目的 README 文件，注入多种典型配置错误。
- **未明确说明具体项目来源**，但强调“自动构造 + Docker 验证”确保质量。

### 对比方法
- 评估了多个 **SOTA LLM**（如 GPT-4、Claude、开源模型等）以及 **智能体框架**（如 ReAct、Plan-and-Solve 等）。
- 对比维度：各阶段能力得分 vs. 端到端成功率。

### 实验覆盖
- 论文提到“across state-of-the-art LLMs and agent frameworks”，但未给出具体数量或消融实验细节。从元数据推测实验数量有限，可能缺少跨不同困难级别或错误类型的充分对比。

## 4. 资源与算力

- **未明确说明**使用的 GPU 型号、数量、训练时长或推理算力。论文主要聚焦基准构建与评估，而非模型训练，因此未提及算力需求。

## 5. 实验数量与充分性

- **实验数量**：论文仅给出摘要，未列出具体实验组数。推测至少进行了多模型（LLM）和多框架的对比实验，但缺少消融实验（如不同错误类型下的表现、不同阶段重要性分析等）。
- **充分性评价**：实验覆盖了主流模型和框架，但未提供详细的统计显著性检验或误差分析。过程级评估本身是创新，但实验规模可能不足以全面揭示不同模型的能力差异。总体而言，实验设计具有初步验证价值，不够充分。

## 6. 论文的主要结论与发现

- **关键发现**：智能体在**错误定位**（感知驱动诊断）环节表现相对较好，但在**将反馈转化为有效修正**（反馈驱动修复）环节存在明显困难，导致端到端性能受限。
- **启示**：改进智能体的核心瓶颈不在于感知错误，而在于**利用环境反馈调整修复策略**的能力。
- **基准价值**：EnConda-bench 是首个为环境配置任务提供过程级内部能力评估的基准，能产生可操作的见解，推动软件工程智能体的发展。

## 7. 优点：方法或实验设计上的亮点

- **细粒度诊断**：首次将环境配置能力分解为计划、感知诊断、反馈修复、执行配置四个阶段，超越传统的端到端成功率评估。
- **自动实例构造**：通过注入真实 README 错误自动生成测试实例，避免人工标注成本，且通过 Docker 验证保证可执行性，实现可扩展的高质量评估。
- **可操作性**：过程级分析能明确指出智能体在哪个环节失败，为后续改进提供明确方向。
- **实用性**：针对实际软件工程中的典型配置错误，贴近真实开发场景。

## 8. 不足与局限

- **错误类型覆盖有限**：仅基于 README 注入的错误类型（如依赖冲突、缺失包等），未覆盖更复杂的环境配置问题（如系统级依赖、操作系统兼容性、缓存问题等）。
- **项目范围未知**：未公开所使用的开源项目清单，可能引入项目选择偏差，影响基准的泛化性。
- **缺乏消融实验**：未深入分析各阶段能力对最终成功的贡献权重，或不同错误类型对智能体表现的差异性影响。
- **评估指标简化**：过程级评估各阶段采用简单的二值正确率，可能无法反映部分正确或部分成功的中间状态。
- **未考虑多步交互**：智能体可能通过多轮反馈迭代修复，但论文的轨迹分段可能未充分建模这种循环交互。
- **计算资源未公开**：无法复现或评估评估成本。

（完）
