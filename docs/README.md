<div class="dpr-home-notice-card">
  <h3 class="dpr-home-notice-title">🚀 Start Here</h3>
  <ul class="dpr-home-notice-list">
    <li><a href="#/tutorial/README">使用教程</a></li>
  </ul>
</div>

## 每次日报
- 最新运行日期：2026-07-29
- 运行时间：2026-07-29 21:47:11 UTC
- 运行状态：成功
- 本次总论文数：32
- 精读区：19
- 速读区：13

### 今日简报（AI）
今日聚焦LLM智能体安全验证与调试工具，两篇高分精读分别提出白盒XSS发现中抗奖励黑客的确定性验证方案（RECEIPT）和开源智能体失败可观测性工具包（AgentDebugX）。最值得关注的是智能体可靠性验证与可观测性方向，后者提供从失败归因到恢复的完整工具链。建议优先阅读这两篇精读，并关注速读中的噪声环境验证-修复循环停止策略。
- 详情：[/202607/29/README](/202607/29/README)

### 精读区论文标签
1. [RECEIPT: Deterministic, Reward-Hacking-Resistant Verification for White-Box Agentic XSS Discovery](/202607/29/2607.18575v1-receipt-deterministic-reward-hacking-resistant-verification-for-white-box-agentic-xss-discovery)  
   标签：评分：9.0/10、query:agent-output
   evidence：验证agent生成的XSS发现
2. [AgentDebugX: An Open-Source Toolkit for Failure Observability, Attribution, and Recovery in LLM Agents](/202607/29/2607.18754v1-agentdebugx-an-open-source-toolkit-for-failure-observability-attribution-and-recovery-in-llm-agents)  
   标签：评分：9.0/10、query:agent-errors
   evidence：LLM agent轨迹中的故障归因与调试
3. [AgentTrails: Towards Trust and Reuse for Agentic Tasks](/202607/29/2607.18816v1-agenttrails-towards-trust-and-reuse-for-agentic-tasks)  
   标签：评分：9.0/10、query:agent-traj
   evidence：将原始轨迹转换为结构化溯源图用于评估
4. [Guardrails as Scapegoats: Auditing Unfaithful Safety Refusals in Tool-Augmented LLM Agents](/202607/29/2607.19449v1-guardrails-as-scapegoats-auditing-unfaithful-safety-refusals-in-tool-augmented-llm-agents)  
   标签：评分：9.0/10、query:agent-output
   evidence：审计智能体对工具沉默故障的不忠实安全拒绝和幻觉性编造行为
5. [JANUS: Foreseeing Latent Risk for Long-Horizon Agent Safety](/202607/29/2607.19913v1-janus-foreseeing-latent-risk-for-long-horizon-agent-safety)  
   标签：评分：9.0/10、query:agent-traj
   evidence：训练守卫从部分轨迹中预判延迟风险
6. [GuardianAgentBench: Where Agents Fail and How to Guard Them](/202607/29/2607.20982v1-guardianagentbench-where-agents-fail-and-how-to-guard-them)  
   标签：评分：9.0/10、query:agent-errors
   evidence：揭示工具调用错误（如少调用和误选）的基准
7. [Workflow-Localized Mechanism Learning: Attribution-Guided Repair and Knowledge Reuse for Structured Agent Skills](/202607/29/2607.20999v1-workflow-localized-mechanism-learning-attribution-guided-repair-and-knowledge-reuse-for-structured-agent-skills)  
   标签：评分：9.0/10、query:agent-errors
   evidence：工作流本地化机制学习，用于智能体技能中的失败归因和修复
8. [HalluScope: Fine-grained Hallucination Diagnosis for Multimodal Large Language Models](/202607/29/2607.21105v1-halluscope-fine-grained-hallucination-diagnosis-for-multimodal-large-language-models)  
   标签：评分：9.0/10、query:agent-output
   evidence：多模态大模型细粒度幻觉诊断
9. [Toward User-Conditioned Evaluation of Personal LLM Agents under Temporal Interventions](/202607/29/2607.21635v1-toward-user-conditioned-evaluation-of-personal-llm-agents-under-temporal-interventions)  
   标签：评分：9.0/10、query:agent-errors
   evidence：关注时间干预下agent组分间的失败传播评估
10. [CARE: Pre-Execution Command Verification for Shell-Executing LLM Agents](/202607/29/2607.21642v1-care-pre-execution-command-verification-for-shell-executing-llm-agents)  
   标签：评分：9.0/10、query:agent-output
   evidence：对智能体生成的shell命令进行预执行验证
11. [Reasoning Denoiser: Denoising Reasoning Traces for Hallucination Detection in Large Reasoning Models](/202607/29/2607.22098v1-reasoning-denoiser-denoising-reasoning-traces-for-hallucination-detection-in-large-reasoning-models)  
   标签：评分：9.0/10、query:agent-output
   evidence：对大推理模型的推理痕迹去噪以检测幻觉
12. [Do Agent Benchmarks Measure Capability? Protocol Validity in the Age of Agentic AI](/202607/29/2607.22368v1-do-agent-benchmarks-measure-capability-protocol-validity-in-the-age-of-agentic-ai)  
   标签：评分：9.0/10、query:agent-traj
   evidence：提出协议有效性概念引入HackDetect检测作弊行为
13. [ConsistencyGate: Preventing Memory Contamination in LLM Agents via Self-Consistency Admission Control](/202607/29/2607.22962v1-consistencygate-preventing-memory-contamination-in-llm-agents-via-self-consistency-admission-control)  
   标签：评分：9.0/10、query:agent-output
   evidence：通过自一致性检查防止智能体轨迹中的记忆污染
14. [SeekJudge: A Practical Reward Framework for Reinforcement Learning in Computer-Use Agents](/202607/29/2607.23263v1-seekjudge-a-practical-reward-framework-for-reinforcement-learning-in-computer-use-agents)  
   标签：评分：9.0/10、query:agent-traj
   evidence：判断轨迹是否符合指令以进行评测
15. [Melo: A Production LLM-Powered Music Recommendation Agent](/202607/29/2607.23718v1-melo-a-production-llm-powered-music-recommendation-agent)  
   标签：评分：9.0/10、query:agent-output
   evidence：检测智能体输出中的实体幻觉和长尾退化
16. [E-Bench: Benchmarking Multi-Step Tool-Use Agents in Real-World Product Scenarios](/202607/29/2607.23722v1-e-bench-benchmarking-multi-step-tool-use-agents-in-real-world-product-scenarios)  
   标签：评分：9.0/10、query:agent-traj
   evidence：用于多步工具使用智能体轨迹评估的合成基准，包含状态改变任务
17. [Falsifiable Commitment Planning for Self-Correcting Web Agents](/202607/29/2607.24167v1-falsifiable-commitment-planning-for-self-correcting-web-agents)  
   标签：评分：9.0/10、query:agent-traj
   evidence：用于agent轨迹的混合承诺测试模块
18. [SearchArt: Training Long-Horizon Search Agent with Scalable Synthetic and Verified Task](/202607/29/2607.24850v1-searchart-training-long-horizon-search-agent-with-scalable-synthetic-and-verified-task)  
   标签：评分：9.0/10、query:agent-traj
   evidence：通过验证驱动的任务合成评估智能体搜索轨迹
19. [When Do Agent Loops Mistake Stagnation for Progress? Self-Evaluation Bias and Externally Grounded Verification in Long-Running Autonomous LLM Agent Loops](/202607/29/2607.25152v1-when-do-agent-loops-mistake-stagnation-for-progress-self-evaluation-bias-and-externally-grounded-verification-in-long-running-autonomous-llm-agent-loops)  
   标签：评分：9.0/10、query:agent-errors
   evidence：对LLM智能体执行轨迹的验证和事实检查

### 速读区论文标签
1. [Verify, Repair, Repeat, or Stop? Robust Stopping for Noisy Verify-Repair Loops in LLM Agents](/202607/29/2607.17641v1-verify-repair-repeat-or-stop-robust-stopping-for-noisy-verify-repair-loops-in-llm-agents)  
   标签：评分：8.0/10、query:agent-output
   evidence：通过为验证-修复循环提供鲁棒停止机制来验证代理响应
2. [ProEvent: An Event-centric Benchmark for Proactive Agents](/202607/29/2607.17701v1-proevent-an-event-centric-benchmark-for-proactive-agents)  
   标签：评分：8.0/10、query:agent-traj
   evidence：面向主动智能体的事件中心基准，评估事件轨迹
3. [(Over)Reliance on Test Agents in AI-Assisted Software Testing](/202607/29/2607.17927v1-overreliance-on-test-agents-in-ai-assisted-software-testing)  
   标签：评分：8.0/10、query:agent-output
   evidence：分析对测试代理的过度信赖，强调检测代理输出有效性和可靠性的必要性
4. [ResearchArena: Evaluating Sabotage and Monitoring in Automated AI R&D](/202607/29/2607.19321v1-researcharena-evaluating-sabotage-and-monitoring-in-automated-ai-rd)  
   标签：评分：8.0/10、query:agent-output
   evidence：评估AI研发代理输出中的隐蔽破坏监控
5. [OpenSkillRisk: Benchmarking Agent Safety When Using Real-World Risky Third-Party Skills](/202607/29/2607.20121v1-openskillrisk-benchmarking-agent-safety-when-using-real-world-risky-third-party-skills)  
   标签：评分：8.0/10、query:agent-output
   evidence：智能体使用第三方技能时的安全基准测试
6. [HACO: Hedged Agent Computing for Reliable LLM Systems](/202607/29/2607.19215v1-haco-hedged-agent-computing-for-reliable-llm-systems)  
   标签：评分：7.0/10、query:agent-errors
   evidence：处理LLM代理工作流中的角色到实例绑定失败
7. [DocOps: A Verifiable Benchmark for Autonomous Agents in Complex Document Operations](/202607/29/2607.19865v1-docops-a-verifiable-benchmark-for-autonomous-agents-in-complex-document-operations)  
   标签：评分：7.0/10、query:agent-traj
   evidence：引入DocOps，一个用于复杂文档操作自主智能体的可验证基准
8. [NVIDIA-labs OO Agents: Native Python Object-Oriented Agents](/202607/29/2607.20709v1-nvidia-labs-oo-agents-native-python-object-oriented-agents)  
   标签：评分：7.0/10、query:agent-traj
   evidence：智能体行为可以像软件一样被测试、追踪和重构
9. [IssueTrojanBench: Benchmarking AI Coding Agents Against Malicious Issue Requests](/202607/29/2607.20759v1-issuetrojanbench-benchmarking-ai-coding-agents-against-malicious-issue-requests)  
   标签：评分：7.0/10、query:agent-output
   evidence：针对恶意issue请求的编码agent输出安全基准
10. [Stochastic Multi-Objective Kinodynamic Planning Against Adversaries](/202607/29/2607.19284v1-stochastic-multi-objective-kinodynamic-planning-against-adversaries)  
   标签：评分：6.0/10、query:agent-traj
   evidence：规划中的轨迹风险评估
11. [Agents in the Wild: Where Research Meets Deployment](/202607/29/2607.19336v1-agents-in-the-wild-where-research-meets-deployment)  
   标签：评分：6.0/10、query:agent-traj
   evidence：关于LLM智能体部署中推理、规划、多智能体协调和评估进展的教程
12. [ChainWatch: A Kill Chain-Aligned Sequential Detection Framework for Multi-Step Attacks in MCP-Based AI Agent Systems](/202607/29/2607.19432v1-chainwatch-a-kill-chain-aligned-sequential-detection-framework-for-multi-step-attacks-in-mcp-based-ai-agent-systems)  
   标签：评分：6.0/10、query:agent-output
   evidence：通过工具调用轨迹的序列分析检测多步攻击
13. [Twin Agent: Context Residual Compression for Privilege Separated Agents](/202607/29/2607.19595v1-twin-agent-context-residual-compression-for-privilege-separated-agents)  
   标签：评分：6.0/10、query:agent-output
   evidence：特权分离设计增强智能体输出安全性


<div class="dpr-home-promo-card">
  <h3 class="dpr-home-promo-title">💬 社区与支持</h3>
  <ul class="dpr-home-promo-list">
    <li>欢迎 Star / Fork / Issue / PR</li>
    <li>QQ群：583867967（欢迎交流，已有：1151人）</li>
  </ul>
</div>
