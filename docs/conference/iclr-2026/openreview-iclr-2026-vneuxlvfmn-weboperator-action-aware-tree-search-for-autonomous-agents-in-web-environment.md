---
title: "WebOperator: Action-Aware Tree Search for Autonomous Agents in Web Environment"
title_zh: WebOperator：面向Web环境中自主智能体的动作感知树搜索
authors: "Mahir Labib Dihan, Tanzima Hashem, Mohammed Eunus Ali, Md Rizwan Parvez"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=vnEuxLVFmN"
tags: ["query:agent-traj"]
score: 6.0
evidence: 通过动作感知树搜索测试Web智能体行为
tldr: WebOperator提出动作感知树搜索方法，解决LLM智能体在Web环境中因贪婪决策导致的脆弱性问题。该方法引入显式回溯机制，允许智能体系统性探索替代路径并纠正错误，从而提升长期规划能力。在多个Web任务上，该方法显著提高了任务成功率和鲁棒性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: LLM智能体在Web环境中缺乏前瞻性，易因一步错误导致失败。
method: 提出动作感知树搜索，集成安全回溯机制以探索替代动作路径。
result: 在Web任务上提升了成功率和鲁棒性。
conclusion: 树搜索方法能有效增强智能体在部分可观测环境中的决策。
---

## Abstract
LLM-based agents often operate in a greedy, step-by-step manner, selecting actions solely based on the current observation without considering long-term consequences or alternative paths. 
This lack of foresight is particularly problematic in web environments, which are only partially observable—limited to browser-visible content such as the current page’s DOM and UI elements—where a single misstep often requires complex and brittle navigation to undo.
Without an explicit backtracking mechanism, agents struggle to correct errors or systematically explore alternative paths. 
Tree-search methods provide a principled framework for such structured exploration, but existing approaches lack mechanisms for safe backtracking, making them prone to unintended side effects. They also assume that all actions are reversible, ignoring the presence of irreversible actions—limitations that reduce their effectiveness in realistic web tasks.
To address these challenges, we introduce WebOperator, a tree-search framework that enables reliable backtracking and strategic exploration. Our method incorporates a best-first search strategy that ranks actions by both reward estimates and safety considerations, along with a robust backtracking mechanism that verifies the feasibility of previously visited paths before replaying them, preventing unintended side effects. To further guide exploration, WebOperator generates action candidates from multiple, varied reasoning contexts to ensure diverse and robust exploration, and subsequently curates a high-quality action set by filtering out invalid actions pre-execution and merging semantically equivalent ones.
Experimental results on WebArena and WebVoyager demonstrate the effectiveness of WebOperator. Notably, on WebArena, WebOperator achieves state-of-the-art performance with gpt-4o, achieving a 54.56% success rate, underscoring the critical advantage of integrating strategic foresight with safe execution.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：基于大语言模型（LLM）的智能体在Web环境中通常采用贪婪的逐步决策方式，仅基于当前观察选择动作，缺乏对长期后果或替代路径的考量。由于Web环境是部分可观测的（仅能看到当前页面的DOM和UI元素），单步错误往往需要复杂且脆弱的导航才能撤销。
- **现有局限**：缺乏显式的回溯机制，导致智能体难以纠正错误或系统地探索替代路径。虽然树搜索方法被提出用于结构化探索，但现有方法缺乏安全回溯机制，容易产生意外副作用；同时它们假设所有动作都可逆，忽略了不可逆动作的存在，从而限制了在真实Web任务中的有效性。
- **动机**：开发一种能够可靠回溯、安全探索替代路径的树搜索框架，以提升LLM智能体在Web环境中的长期规划能力和鲁棒性。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：提出**WebOperator**，一个动作感知树搜索框架，结合最佳优先搜索策略、安全回溯机制以及多样化的动作候选生成与筛选。
- **关键技术细节**：
  - **最佳优先搜索**：按奖励估计和安全性考虑对动作排序，指导探索方向。
  - **安全回溯机制**：在回溯前验证之前访问过的路径的可行性，并重放时避免产生意外副作用。
  - **多样化动作候选生成**：从多个不同的推理上下文中生成动作候选，确保探索的多样性。
  - **高质量动作集筛选**：在执行前过滤无效动作，合并语义等价的动作，以精简搜索空间。
- **算法流程（文字描述）**：
  1. 初始化根节点（当前状态）。
  2. 使用多个推理上下文生成候选动作集。
  3. 过滤无效动作，合并等价动作，得到高质量动作集。
  4. 利用最佳优先策略（结合奖励和安全性）选择下一个动作执行。
  5. 执行动作后若遇到错误或死胡同，启动安全回溯：验证之前访问的节点是否可安全回退，然后回退到该节点并继续探索。
  6. 重复步骤2–5，直到任务完成或达到最大搜索深度。

## 3. 实验设计
- **数据集/场景**：WebArena（基准Web任务环境）和WebVoyager（另一个Web智能体评估平台）。
- **Benchmark**：在WebArena上使用gpt-4o作为基座模型进行评测。
- **对比方法**：未在摘要中明确列出对比的基线方法，但提及“现有树搜索方法缺乏安全回溯”“贪婪基线”等，推测对比了贪心策略、现有树搜索方法等。

## 4. 资源与算力
- **文中未明确说明**：摘要及元数据中未提及使用的GPU型号、数量或训练时长。可能由于该工作主要基于现有LLM（如gpt-4o）进行推理，而不是训练大型模型，因此算力需求主要体现在推理和搜索过程，但具体细节缺失。

## 5. 实验数量与充分性
- **实验数量**：仅在两个数据集（WebArena、WebVoyager）上进行了测试。在WebArena上用gpt-4o取得了54.56%的成功率，但未报告消融实验数量、不同LLM基座对比等。元数据中提到“evidence: 通过动作感知树搜索测试Web智能体行为”，说明进行了验证。
- **充分性判断**：实验覆盖有限，缺少在不同模型、不同任务难度、长尾场景的详细分析。未提供消融实验来量化每个组件（如安全回溯、多样化生成）的贡献。因此充分性一般，有待更全面的评估。

## 6. 主要结论与发现
- WebOperator通过将策略性前瞻与安全执行相结合，显著提升了Web智能体的任务成功率和鲁棒性。
- 在WebArena上使用gpt-4o达到了54.56%的成功率，是当时的最优结果（state-of-the-art）。
- 显式回溯和安全性验证机制有效避免了错误传播，并允许系统性地探索替代路径。

## 7. 优点
- **方法创新**：首次在Web智能体树搜索中集成安全回溯，解决了现有方法忽略不可逆动作和副作用的问题。
- **实用性强**：动作候选多样化生成与筛选机制提升了搜索效率和质量。
- **结果突出**：在标准基准上达到SOTA，证明方法的有效性。
- **可解释性**：树搜索框架提供了可追溯的决策路径，有利于分析和调试。

## 8. 不足与局限
- **实验规模有限**：仅在两个数据集上测试，且只使用了gpt-4o一种模型，缺少跨模型（如开源LLM）和跨领域的泛化验证。
- **缺乏消融实验**：未单独评估安全回溯、多样化生成等组件的贡献，难以判断各模块的实际增益。
- **算力/资源未报告**：无法评估方法的经济性或可复现性。
- **未讨论失败案例**：摘要未分析54.56%成功率的剩余失败情况，可能涉及不可逆动作的极端复杂场景。
- **应用限制**：依赖强LLM（如gpt-4o）进行推理，成本较高；且树搜索可能需要大量API调用，实时性受限。

（完）
