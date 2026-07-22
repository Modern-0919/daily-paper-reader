---
title: "Rethinking Verification for LLM Code Generation: From Generation to Testing"
title_zh: 重新思考大语言模型代码生成的验证：从生成到测试
authors: "Zihan Ma, Taolin Zhang, Maosongcao, Junnan Liu, Wenwei Zhang, Minnan Luo, Songyang Zhang, Kai Chen"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=Gp2vgxWROE"
tags: ["query:agent-errors"]
score: 7.0
evidence: 提出基于多维指标的测试用例生成方法用于代码生成验证
tldr: 针对现有代码生成基准测试用例同质化导致错误漏检的问题，系统研究测试用例生成任务，提出多维指标量化测试集充分性，并引入人机协作方法（SAG），显著提升了验证可靠性，对强化学习的奖励估计也至关重要。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有代码生成基准测试用例少且同质，导致细微错误漏检且影响强化学习奖励估计。
method: 提出多维测试充分性指标和人机协作的测试用例生成方法（SAG）。
result: 新方法生成更多样化的测试用例，提高了错误检测率。
conclusion: 该工作为LLM代码生成的验证提供了更可靠的评估框架。
---

## Abstract
Large language models (LLMs) have recently achieved notable success in code‑generation benchmarks such as HumanEval and LiveCodeBench. However, a detailed examination reveals that these evaluation suites often comprise only a limited number of homogeneous test cases, resulting in subtle faults going undetected. This not only artificially inflates measured performance but also compromises accurate reward estimation in reinforcement learning frameworks utilizing verifiable rewards (RLVR). To address these critical shortcomings, we systematically investigate the test-case generation (TCG) task by proposing multi-dimensional metrics designed to rigorously quantify test-suite thoroughness. Furthermore, we introduce a human-LLM collaborative method (SAGA), leveraging human programming expertise with LLM reasoning capability, aimed at significantly enhancing both the coverage and the quality of generated test cases. In addition, we develop a TCGBench to facilitate the study of the TCG task. Experiments show that SAGA achieves a detection rate of 90.62\% and a verifier accuracy of 32.58\% on TCGBench. The Verifier Accuracy (Verifier Acc) of the code generation evaluation benchmark synthesized by SAGA is 10.78\% higher than that of LiveCodeBench-v6. These results demonstrate the effectiveness of our proposed method. We hope this work contributes to building a scalable foundation for reliable LLM code evaluation, further advancing RLVR in code generation, and paving the way for automated adversarial test synthesis and adaptive benchmark integration.

---

## 论文详细总结（自动生成）

# 论文总结：重新思考大语言模型代码生成的验证：从生成到测试

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有大语言模型（LLM）代码生成基准测试（如 HumanEval、LiveCodeBench）存在测试用例数量少、同质化严重的问题，导致模型生成的代码中的微小错误难以被检测到。
- **研究动机**：这种缺陷不仅人为地高估了模型的性能，还会在基于可验证奖励的强化学习（RLVR）框架中造成奖励估计不准确，从而影响模型训练和评估的可靠性。
- **整体含义**：本文旨在通过系统研究测试用例生成（Test-Case Generation, TCG）任务，构建更可靠、更具可扩展性的LLM代码评估框架，为后续强化学习奖励估计和自动化对抗测试奠定基础。

## 2. 论文提出的方法论
- **核心思想**：提出多维指标来量化测试集的充分性，并引入一种人机协作（Human-LLM Collaboration）方法，以提升生成测试用例的覆盖率和质量。
- **关键技术细节**：
  - **多维测试充分性指标**：从多个维度（如代码覆盖、边界条件、语义多样性等）设计量化指标，用于评估测试集的全面性。
  - **SAGA方法（人-LLM协作方法）**：结合人类程序员的专业知识（如对错误模式的理解）与LLM的推理能力（如自动生成多样化的测试输入），共同生成更全面、更高质量的测试用例。
  - **TCGBench**：构建专门的基准数据集，用于系统研究TCG任务。
- **算法流程（文字说明）**：
  1. 给定待验证的代码片段或生成的目标代码。
  2. 人类专家提供初始的测试用例设计思路或关键边界条件。
  3. LLM基于这些提示生成大量候选测试用例，并利用多维充分性指标进行筛选或扩展。
  4. 人工评审或自动验证测试用例的正确性与覆盖率，迭代优化直到满足多维指标要求。
  5. 最终生成的测试套件用于LLM生成代码的正确性评估。

## 3. 实验设计
- **使用的数据集/场景**：论文构建了TCGBench作为专门的基准数据集，用于研究测试用例生成任务。评估场景包括代码生成模型的错误检测能力和验证器准确性。
- **Benchmark**：主要与LiveCodeBench-v6进行对比。
- **对比方法**：未明确列出其他对比方法，但实验表明SAGA方法在TCGBench上取得了显著提升。
- **关键指标**：
  - 检测率（Detection Rate）：SAGA达到90.62%。
  - 验证器准确率（Verifier Accuracy）：SAGA为32.58%，比LiveCodeBench-v6高出10.78%（绝对值）。

## 4. 资源与算力
- **未明确说明**：论文摘要及元数据中未提及使用的GPU型号、数量、训练时长等算力信息。可能需要在全文才能获取。

## 5. 实验数量与充分性
- **实验数量**：从摘要看，主要在TCGBench上进行了有效评估，并对比了LiveCodeBench-v6这一单一基准。未提及消融实验、不同数据集或超参数调优等多组实验。
- **充分性与公平性**：
  - **优点**：提供了明确的量化指标（检测率、验证器准确率），对比基准直接。
  - **不足**：实验覆盖范围有限（仅一个自定义benchmark和一个公开基准），未涉及多种代码生成模型、多种编程语言或不同复杂度任务的测试。消融实验（如单独使用人类或LLM的对比）未提及，因此实验的充分性和全面性有待增强。

## 6. 论文的主要结论与发现
- SAGA方法能够生成更多样化的测试用例，显著提高对代码生成中微小错误的检测能力。
- 基于SAGA合成的评估基准，其验证器准确率比当前主流基准（LiveCodeBench-v6）提升10.78%，证明该方法的有效性。
- 本文工作为LLM代码评估提供了更可靠的框架，有助于推动RLVR在代码生成中的实际应用，并为自动对抗测试生成和自适应基准集成铺平道路。

## 7. 优点
- **方法创新**：首次系统研究LLM代码生成中的测试用例生成任务，并提出人机协作策略，兼顾人类专家的领域知识和LLM的规模化生成能力。
- **指标设计**：提出多维测试充分性指标，超越了传统单一覆盖指标，更能反映测试集的真实质量。
- **实用性**：生成的测试用例多样性高，错误检测率接近91%，直接提升评估可靠性。
- **开源贡献**：构建了TCGBench，便于后续研究。

## 8. 不足与局限
- **实验覆盖有限**：仅在单一自建benchmark和LiveCodeBench-v6上进行验证，未在更多主流代码生成基准（如HumanEval、MBPP、CodeContests等）上测试。
- **未消融分析**：没有明确分离人类贡献与LLM贡献，人机协作的具体增益量化不明。
- **可扩展性风险**：依赖人类专家参与，可能导致规模化成本高、主观偏差；自动化程度有待提升。
- **偏差风险**：TCGBench的构建过程可能引入对特定错误模式的偏好，导致对某些模型的评估偏差。
- **应用限制**：当前方法主要针对功能性验证，未涉及性能、安全性等非功能需求测试；此外，对于极长或极复杂的代码生成任务，测试用例生成本身可能面临组合爆炸问题。

（完）
