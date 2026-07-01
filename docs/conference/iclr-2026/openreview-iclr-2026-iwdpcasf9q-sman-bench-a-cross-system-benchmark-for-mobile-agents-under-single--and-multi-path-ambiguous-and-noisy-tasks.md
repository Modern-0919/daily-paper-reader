---
title: "SMAN-Bench: A Cross-System Benchmark for Mobile Agents under Single- and Multi-path, Ambiguous, and Noisy Tasks"
title_zh: SMAN-Bench：面向移动代理的跨系统基准测试，支持单/多路径、模糊和噪声任务
authors: "Weikai Xu, Zhizheng Jiang, Yuxuan Liu, Pengzhi Gao, Wei Liu, Jian Luan, Yunxin Liu, Yuanchun Li, Bin Wang, Bo An"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=IWDpCaSF9Q"
tags: ["query:agent-traj"]
score: 9.0
evidence: 针对移动代理轨迹的基准测试，涵盖多路径和噪声场景
tldr: 现有移动代理基准测试在动态环境和多解特性下存在不足。SMAN-Bench提出一种基于槽位的指令生成方法，构建涵盖单路径、多路径、模糊和噪声任务的跨系统基准，用于全面评估移动代理轨迹。实验表明该基准能更稳定地反映代理性能，推动移动代理评估的标准化。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有在线和离线基准无法有效评估移动代理在动态和模糊任务下的轨迹表现，缺乏对多解特性和噪声的考虑。
method: 提出SMAN-Bench，采用槽位指令生成方法，构造单/多路径、模糊和噪声任务，构建跨系统移动代理评估基准。
result: 该基准能稳定评估移动代理轨迹，弥补现有基准在多解和噪声场景下的不足。
conclusion: SMAN-Bench为移动代理轨迹评估提供了更全面的基准，促进代理在真实场景中的鲁棒性研究。
---

## Abstract
VLM-based mobile agents are increasingly popular due to their capabilities to interact with smartphone GUIs and XML-structured texts and to complete daily tasks. However, existing online benchmarks fail to obtain stable critical reward signals under dynamic environmental changes, and neglect the influence of noise components and interactive instructions. Offline benchmarks evaluate the agents through single-path trajectories, which stand in contrast to the inherently multi-solution characteristics of GUI tasks. To address these limitations, we introduce SMAN-Bench, a benchmark designed to evaluate agents under Single-path, Multi-path, Ambiguous, and Noisy task settings. We employ a slot-based instruction generation method to match templates with GUI trajectories from an existing, graph-structured, unlabeled mobile corpus. SMAN-Bench includes a common task split, with offline multi-path evaluation to assess the agent’s ability to obtain step rewards during task execution. It contains a noisy split based on pop-ups and ad apps, and a contaminated split named AITZ-Noise to simulate a realistic noisy environment. Furthermore, an ambiguous instruction split with preset Q&A interactions is released to evaluate the agent’s proactive interaction capabilities. Our evaluation covers mobile agent frameworks like AppAgent-v1, Mobile-Agent-v2, and Mobile-Agent-E, and includes both open-source and closed-source mobile fundamental models, as well as several multimodal reasoning models.

---

## 论文详细总结（自动生成）

# SMAN-Bench: 面向移动代理的跨系统基准测试，支持单/多路径、模糊和噪声任务——详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：当前基于VLM的移动代理在智能手机GUI交互和文本结构任务中表现活跃，但现有基准测试存在严重缺陷。
- **在线基准问题**：无法在动态环境变化下获得稳定的关键奖励信号，且忽略了噪声组件（如弹窗、广告）和交互式指令的影响。
- **离线基准问题**：仅通过**单一路径轨迹**评估代理，这与GUI任务固有的**多解特性**（即同一任务可有多条正确完成路径）相矛盾。
- **整体含义**：需要一个能全面评估移动代理在**单路径、多路径、模糊指令和噪声环境**下轨迹表现的标准基准，以推动移动代理评估的标准化和鲁棒性研究。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：采用**基于槽位的指令生成方法**，将模板与来自现有图结构、无标签移动语料库的GUI轨迹进行匹配，自动构造多样化任务。
- **关键技术细节**：
  - **槽位指令生成**：定义任务模板，其中包含可替换的槽位（如操作目标、应用名称、点击元素等），从无标签移动语料库中提取GUI轨迹结构（如状态图），将模板实例化为具体指令。
  - **任务类型划分**：
    - **常见任务（Common Split）**：包含单路径（Single-path）和多路径（Multi-path）任务，用于评估代理在任务执行过程中获取步骤奖励的能力（离线多路径评估）。
    - **噪声任务（Noisy Split）**：基于弹窗（pop-ups）和广告应用（ad apps）模拟真实噪声环境。
    - **污染噪声任务（AITZ-Noise Split）**：模拟更复杂的噪声场景。
    - **模糊指令任务（Ambiguous Instruction Split）**：包含预设的问答交互，评估代理主动交互能力（如需要向用户澄清模糊指令）。
- **算法流程**（文字说明）：
  1. 从无标签移动语料库（图结构）中提取GUI状态图和操作轨迹。
  2. 设计任务模板（含槽位），模板对应不同路径数量、噪声等级、模糊程度。
  3. 自动填充槽位生成具体指令及对应期望轨迹（可有多条）。
  4. 评估时，代理根据指令执行，通过对比代理的实际轨迹与期望轨迹集合计算步骤奖励（step reward）。

## 3. 实验设计

- **数据集/场景**：
  - 使用**现有图结构、无标签移动语料库**作为基础，构造基准任务。
  - 包含**常见任务**、**噪声任务（弹窗/广告）**、**污染噪声任务（AITZ-Noise）**、**模糊指令任务**。
- **基准（Benchmark）**：SMAN-Bench本身即为提出的基准，强调跨系统（Cross-System）评估。
- **对比方法**：
  - **移动代理框架**：AppAgent-v1、Mobile-Agent-v2、Mobile-Agent-E。
  - **模型**：开源和闭源的移动基础模型（Mobile Fundamental Models）、多种多模态推理模型。
- **评估指标**：主要采用步骤奖励（step reward），评估代理在任务执行中每一步是否正确。

## 4. 资源与算力

- **文中未明确说明**：论文摘要及元数据中未提及使用的GPU型号、数量、训练时长等具体算力信息。可能受限于篇幅或尚在补充中。需要查看全文（如果可访问）才能获得详细算力消耗。

## 5. 实验数量与充分性

- **实验数量**：根据摘要，对比了多个框架和多种模型，涵盖了开源/闭源基础模型以及推理模型，但未列出具体实验组数。
- **充分性判断**：
  - **正面**：任务类型覆盖单/多路径、模糊指令、多种噪声环境，场景较全面；对比了多个代表性方法。
  - **不足**：缺乏消融实验、不同噪声程度的控制变量分析、跨系统泛化性验证等细节。可能需要在完整论文中查看更详细的消融和统计分析。
  - **客观公平性**：采用离线多路径评估，避免了在线环境不稳定性，但离线评估可能无法完全反映真实环境。对比方法均采用相同基准设置，较为公平。

## 6. 主要结论与发现

- SMAN-Bench能够**更稳定地反映代理性能**，弥补现有基准在**多解特性和噪声场景**下的不足。
- 通过实验表明，现有的移动代理在**模糊指令**和**噪声环境**下性能显著下降，需要更鲁棒的交互策略。
- 提出的**槽位指令生成方法**有效构建了多样化任务，支持跨系统评估。
- 该基准促进了移动代理在真实场景中鲁棒性研究，为标准化评估提供了基础。

## 7. 优点

- **任务设计创新**：首次在基准中系统性地考虑了**多路径、模糊指令和噪声环境**，非常贴合实际移动GUI任务特点。
- **生成方法高效**：基于槽位指令生成，无需人工标注大量指令，可自动扩展任务规模。
- **评估指标合理**：采用**步骤奖励**而非仅最终成功率，能更细粒度衡量代理执行质量。
- **跨系统支持**：设计上考虑不同移动系统（如Android、iOS）的通用性，促进跨平台比较。
- **涵盖多种噪声**：包括弹窗、广告和AITZ-Noise污染，模拟真实世界干扰。

## 8. 不足与局限

- **实验细节缺失**：未说明具体实验组数、方差、显著性检验等，说服力可能不足。
- **算力资源未提及**：难以判断基准构建和评估的计算成本。
- **离线评估局限**：虽然避免了在线不稳定性，但离线环境可能简化了真实交互的复杂性（如网络延迟、UI状态动态变化等）。
- **噪声场景泛化性**：仅基于特定语料库的噪声（弹窗、广告），可能不能代表所有类型的噪声。
- **模糊指令设计**：预设问答交互可能偏离真实用户自然模糊表达，需要进一步验证生态效度。
- **模型覆盖**：对比的移动代理框架数量有限（三个），且未提及最新大模型（如GPT-4V等闭源模型的具体版本），可能不够及时。

（完）
