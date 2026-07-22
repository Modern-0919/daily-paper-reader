---
title: "GitTaskBench: A Benchmark for Code Agents Solving Real-World Tasks Through Code Repository Leveraging"
title_zh: GitTaskBench：通过代码仓库利用解决真实世界任务的代码智能体基准
authors: "Ziyi Ni, Huacan Wang, Shuo Zhang, Shuo Lu, Ziyang He, WangYou, Zhenheng Tang, Sen Hu, Bo Li, Chen Hu, Binxing Jiao, Daxin Jiang, Yuntao Du, Pin Lyu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/40533/44494"
tags: ["query:agent-traj"]
score: 6.0
evidence: 评估智能体在实际代码任务中轨迹的基准
tldr: GitTaskBench是一个针对代码智能体的基准，通过54个真实仓库任务和自动化评估工具来测试智能体在真实工作流中的表现，并提出了衡量经济效益的alpha-value指标。该基准弥补了当前缺乏面向代码仓库实际任务的评估空白，可间接用于代理轨迹测试领域。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40533/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 879, \"height\": 756, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40533/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1750, \"height\": 992, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40533/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1830, \"height\": 439, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40533/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 878, \"height\": 346, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40533/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 881, \"height\": 525, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40533/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1757, \"height\": 422, \"label\": \"Figure\"}, {\"url\": \"assets/figures/aaai-2026-accepted/aaai-2026-40533/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 882, \"height\": 319, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40533/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 710, \"height\": 480, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40533/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1787, \"height\": 339, \"label\": \"Table\"}, {\"url\": \"assets/tables/aaai-2026-accepted/aaai-2026-40533/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1339, \"height\": 918, \"label\": \"Table\"}]"
motivation: 现有基准缺乏对代码智能体在真实仓库工作流中表现的评估。
method: 构建包含54个跨7模态和7领域任务的基准，配对仓库与自动化评估工具。
result: 提出alpha-value指标综合任务成功率、代币成本和并发性。
conclusion: 为代码代理轨迹评估提供了实用基准和量化指标。
---

## Abstract
Beyond scratch coding, exploiting large-scale code repositories (e.g., GitHub) for practical tasks is vital in real-world software development, yet current benchmarks rarely evaluate code agents in such authentic, workflow-driven scenarios. 
To bridge this gap, we introduce GitTaskBench, a benchmark designed to systematically assess this capability via 54 realistic tasks across 7 modalities and 7 domains. 
Each task pairs a relevant repository with an automated, human-curated evaluation harness specifying practical success criteria. Beyond measuring execution and task success, we also propose the alpha-value metric to quantify the economic benefit of agent performance, which integrates task success rates, token cost, and average developer salaries. Experiments across three state-of-the-art agent frameworks with multiple advanced LLMs show that leveraging code repositories for complex task solving remains challenging: even the best-performing system, OpenHands+Claude 3.7, solves only 48.15% of tasks. Error analysis attributes over half of failures to seemingly mundane yet critical steps like environment setup and dependency resolution, highlighting the need for more robust workflow management and increased timeout preparedness.
By releasing GitTaskBench, we aim to drive progress and attention toward repository-aware code reasoning, execution, and deployment---moving agents closer to solving complex, end-to-end real-world tasks.

---

## 论文详细总结（自动生成）

# GitTaskBench 论文总结

## 1. 核心问题与整体含义
- **研究动机**：现有代码基准（如 SWE-Bench、RepoBench、MLE-Bench）多聚焦于从零编写代码、修复 Bug 或完成独立编程题，忽略了真实软件开发中更常见的场景——**利用已有 GitHub 仓库**来解决实际、多步骤、端到端的任务（如图像上色、语音增强、文档处理等）。
- **背景**：GitHub 上有海量开源项目，程序员经常复用而非重造轮子，但现有评估未衡量智能体在自主理解仓库、搭建环境、修改代码并执行方面的综合能力。
- **核心问题**：如何系统评估代码智能体在真实仓库工作流中的表现，并量化其经济效益？

## 2. 论文提出的方法论
### 核心思想
- 设计一个包含 **54 个真实任务** 的基准（GitTaskBench），每个任务绑定一个完整的 GitHub 仓库，智能体需根据自然语言指令自主完成仓库理解、环境配置、代码生成/修改、执行并输出符合质量要求的结果。

### 关键技术细节
- **构建流程（四步）**：
  1. **任务与仓库选择**：通过文献和专家调研确定 7 个领域（图像/视频/语音/生理信号处理、隐私安全、网络爬虫、办公文档），筛选 18 个 Python 仓库（⭐ ≥ 50、近 5 年活跃、提供预训练权重和简单配置）。
  2. **完整性验证**：人类专家按仓库文档完整执行一次，确保 100% 成功，并补充缺失的依赖或外部链接。
  3. **执行框架设计**：智能体输入为 GitHub 仓库 + 任务描述，需自主完成仓库分析、代码生成/修改、依赖安装、代码执行，最终输出指定文件或文本。
  4. **评估框架设计**：定制测试脚本自动判断输出格式、内容、质量指标。

- **评估指标**：
  - **ECR（执行完成率）**：智能体成功生成非空、格式正确的输出。
  - **TPR（任务通过率）**：输出满足领域标准（如 PESQ ≥ 2.0 等）。
  - **α 经济效益值**：  
    \[
    \alpha = \frac{1}{n}\sum_{i=1}^{n}[(T \times MV \times Q) - C]
    \]  
    - \(T\)：ECR 对应的成功执行标志（0/1）。  
    - \(MV\)：人类完成该任务的公开市场价值（如 Fiverr 定价）。  
    - \(Q\)：输出质量因子（人类专家与参考结果比较，分 5 级：0, 0.25, 0.5, 0.75, 1.0）。  
    - \(C\)：智能体运行成本（API 费用）。

## 3. 实验设计
- **场景与数据**：
  - 54 个任务来自 18 个真实 GitHub 仓库，覆盖 7 个模态（图像、视频、语音、生理信号、隐私、爬虫、文档）和 24 个子领域。
  - 每个任务配备手工编写的测试脚本和质量阈值（如 PESQ ≥ 1.5、FID ≤ 400、列准确率 100% 等）。
- **基准对比**：
  - 三个主流代码智能体框架：**Aider**、**OpenHands**、**SWE-Agent**。
  - 多种 LLM（闭源：GPT-4o、GPT-4.1、Claude 3.5/3.7、Gemini 2.5 Pro、o3-mini；开源：DeepSeek V3、Qwen3-8B/14B/32B、Llama3.3-70B）。
- **对比内容**：
  - 主要性能指标（ECR、TPR、输入/输出 Token 数、API 成本）。
  - 领域特异性分析（图4）。
  - 超参数敏感性（timeout、max_iteration）。
  - 经济效益分析（每个仓库的 α 值和帕累托曲线，图6）。
  - 错误分类统计（图7）。

## 4. 资源与算力
- 论文 **未明确说明** 使用的 GPU 型号、数量或训练时长。仅提到“自行部署的开源模型”（如 Qwen3、Llama3.3），但未给出具体推理硬件配置。
- 所有实验结果基于 API 调用（闭源模型）或本地部署推理，**不涉及模型训练**。

## 5. 实验数量与充分性
- **数量**：
  - 表3：13 种框架-LLM 组合 × 2 次独立运行（取平均），共约 26 组试验。
  - 超参数敏感性分析（表5）：2 个参数 × 3 水平，共 6 组。
  - 经济效益分析（图6）：3 个模型 × 18 仓库，共 54 组数据点。
  - 错误分类：基于所有失败案例的手工归类（图7 涉及 5 种错误类型）。
- **充分性**：
  - 覆盖多种主流框架和模型，包括闭源和开源，对比较全面。
  - 采用两次独立运行取平均，增强统计可靠性。
  - 消融视角：通过调整超参数观察影响，可视为部分消融。
  - **不足**：任务数量 54 相对有限；仅评估 Python 仓库；未进行多智能体或人机协作对比；未做更细粒度的消融（如移除某评估指标的影响）。

## 6. 论文的主要结论与发现
1. **仓库中心任务极具挑战**：最佳组合（OpenHands + Claude 3.7）仅达到 48.15% TPR，超半数任务失败。
2. **经济效益非单调**：并非所有用智能体取代人都划算——高价值任务（如视频分析）利润可观，低价值任务（简单图像处理）可能亏损。
3. **纯文本任务优于多模态**：办公文档处理（Excel/PDF）成功率远高于图像/语音类任务，后者需要更深的仓库理解和环境配置。
4. **环境配置是主要瓶颈**：错误分析显示 65% 的失败归因于环境设置和依赖解析，而非代码逻辑错误。
5. **超参数至关重要**：增大 timeout 和 max_iteration 显著提升性能，但增加 Token 成本。
6. **开源模型有潜力**：Qwen3-32B（思考模式）可达闭源模型的 60% 性能且 Token 更少；DeepSeek V3 成本最低。

## 7. 优点
- **任务设计贴近真实**：54 个任务涵盖 7 个模态，全部来自实际 GitHub 仓库，强调端到端利用而非从零编码。
- **自动化与人工结合**：测试脚本自动评估，同时引入“完整性验证”和“质量因子”人工评判，保证可靠性。
- **经济效益量化**：α 值独创地将成本、质量、市场价值统一，为智能体部署提供经济决策依据。
- **错误分类系统**：将失败细分为 5 类（环境、规划、理解、运行时、指令遵循），揭示关键瓶颈。
- **开源代码与数据**：代码和数据集公开（GitHub），便于复现和扩展。

## 8. 不足与局限
- **任务规模有限**：仅 54 个任务、18 个仓库，且全部为 Python，缺乏跨语言、跨平台覆盖。
- **仓库选择偏差**：仅选择活跃、文档较好的仓库，未覆盖文档缺失或配置复杂的项目。
- **评估环境简化**：在沙箱中运行，未考虑真实网络延迟、权限限制、多版本冲突等复杂因素。
- **成本估算简化**：α 值中 C 仅考虑 API 费用，未计入硬件折旧、电费、人工干预等；MV 取自公开定价，可能偏离实际企业开销。
- **质量因子主观性**：Q 由 5 位专家投票决定，存在个体差异和领域知识差异。
- **未验证泛化性**：未测试训练数据泄露风险（模型可能已见过这些仓库代码）。
- **缺乏更高级协作**：未评估多智能体分工或集成工具链的场景。

（完）
