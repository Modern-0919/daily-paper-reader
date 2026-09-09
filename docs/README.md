<div class="dpr-home-notice-card">
  <h3 class="dpr-home-notice-title">🚀 Start Here</h3>
  <ul class="dpr-home-notice-list">
    <li><a href="#/tutorial/README">使用教程</a></li>
  </ul>
</div>

## 每次日报
- 最新运行日期：2026-09-09
- 运行时间：2026-09-09 22:17:38 UTC
- 运行状态：成功
- 本次总论文数：40
- 精读区：27
- 速读区：13

### 今日简报（AI）
今日推荐40篇，精读27篇，重点锁定幻觉检测与长程智能体压力状态两大主题。
最高分9.0的两篇均聚焦模型可靠性：Enoki提出高效多级幻觉检测，另一篇则识别工具智能体晚期压力状态，值得优先深读。
建议再关注速读中关于参考轨迹自我进化与实时轨迹建模的8分论文，可补充可靠性提升的全局视角。
- 详情：[/202609/09/README](/202609/09/README)

### 精读区论文标签
1. [Enoki: Efficient Multi-Level Hallucination Detection](/202609/09/2609.00581v1-enoki-efficient-multi-level-hallucination-detection)  
   标签：评分：9.0/10、query:agent-output
   evidence：开放信息抽取框架，支持声明级验证与跨层级的幻觉定位
2. [Polished but Unresolved: Identifying Late-Stage Pressure States in Long-Horizon Tool-Use Agents](/202609/09/2609.00823v1-polished-but-unresolved-identifying-late-stage-pressure-states-in-long-horizon-tool-use-agents)  
   标签：评分：9.0/10、query:agent-errors
   evidence：识别会导致提前提交未解决答案的智能体压力状态，并利用激活干预改变行为
3. [Dense Process Supervision for Search Agents via Fact Utility Estimation](/202609/09/2609.00833v1-dense-process-supervision-for-search-agents-via-fact-utility-estimation)  
   标签：评分：9.0/10、query:agent-traj
   evidence：面向搜索智能体RL提出稠密过程监督，通过将推理建模为事实累积并沿轨迹估计事实效用。
4. [Reveree: Diagnosing LLM Reverse-Engineering Agents](/202609/09/2609.01185v1-reveree-diagnosing-llm-reverse-engineering-agents)  
   标签：评分：9.0/10、query:agent-traj
   evidence：从求解率、里程碑进展和行为画像三个层面对LLM智能体轨迹进行评分
5. [What Does an Agentic Software Engineering Benchmark Measure? Profiling Task Demands and Agent Behaviour Beyond What Category Labels Reveal](/202609/09/2609.01271v1-what-does-an-agentic-software-engineering-benchmark-measure-profiling-task-demands-and-agent-behaviour-beyond-what-category-labels-reveal)  
   标签：评分：9.0/10、query:agent-traj
   evidence：面向智能体软件工程基准，用SNC画像分析任务负载与轨迹行为
6. [EDGE: Error Dependency Graph-Guided Multi-Error Attribution in Multi-Agent LLM Systems](/202609/09/2609.01360v1-edge-error-dependency-graph-guided-multi-error-attribution-in-multi-agent-llm-systems)  
   标签：评分：9.0/10、query:agent-errors
   evidence：通过错误依赖图和反事实回滚对多智能体LLM工作流中多个关联错误进行归因，对应失败归因与错误传播需求。
7. [Efficient SWE Agent Benchmarking via Trajectory-Aware Evaluation](/202609/09/2609.01603v1-efficient-swe-agent-benchmarking-via-trajectory-aware-evaluation)  
   标签：评分：9.0/10、query:agent-traj
   evidence：利用过程与结果信号的软件工程智能体轨迹感知评估
8. [Agent Flight Recorder: Tamper-Evident Audit Trails with On-Chain Anchoring for Long-Horizon Tool-Using Agents](/202609/09/2609.01931v1-agent-flight-recorder-tamper-evident-audit-trails-with-on-chain-anchoring-for-long-horizon-tool-using-agents)  
   标签：评分：9.0/10、query:agent-output
   evidence：将工具型智能体每个动作记录为防篡改审计事件，支持输出监控与验证
9. [Monitoring Web Agents Without Internal Signals: Observable Trajectories and Key-Step Supervision](/202609/09/2609.02057v1-monitoring-web-agents-without-internal-signals-observable-trajectories-and-key-step-supervision)  
   标签：评分：9.0/10、query:agent-errors
   evidence：通过可观察轨迹前缀进行风险预测，并标注首个未被纠正的关键错误，属于LLM智能体轨迹的步骤级错误检测。
10. [Diagnosing with Insights: Structured Analysis of Agent Failures via Behavioral Abstractions](/202609/09/2609.02371v1-diagnosing-with-insights-structured-analysis-of-agent-failures-via-behavioral-abstractions)  
   标签：评分：9.0/10、query:agent-errors
   evidence：提出AGENTSCOPE，一种基于轨迹行为抽象的神经符号智能体故障模式诊断方法
11. [EarlyEval: Cheaper Agent Evaluation via Early Outcome Prediction](/202609/09/2609.02783v1-earlyeval-cheaper-agent-evaluation-via-early-outcome-prediction)  
   标签：评分：9.0/10、query:agent-traj
   evidence：利用智能体中段轨迹预测最终结果以降低评估成本，是一种面向智能体轨迹的评测加速方法。
12. [You Can't Escape Your Own Activations : Evaluation Awareness and Multi-Agent Monitoring](/202609/09/2609.03035v1-you-cant-escape-your-own-activations--evaluation-awareness-and-multi-agent-monitoring)  
   标签：评分：9.0/10、query:agent-output
   evidence：针对输出监控易被混淆或隐写骗过的问题，用内部激活探针检测多智能体合谋输出，并研究被监控知晓的影响
13. [KC-Bench: A Dynamic Interactive Benchmark for Evaluating Knowledge Conflicts in LLM Agents](/202609/09/2609.03588v1-kc-bench-a-dynamic-interactive-benchmark-for-evaluating-knowledge-conflicts-in-llm-agents)  
   标签：评分：9.0/10、query:agent-traj
   evidence：KC-Bench提供受控多轮交互任务，内含状态工具、环境断言与人工轨迹校验，直接构成智能体轨迹测试基准。
14. [DCFA: Dual-view Causal-inspired Attribution for Failure Reasoning in LLM-based Multi-agent Systems](/202609/09/2609.04749v1-dcfa-dual-view-causal-inspired-attribution-for-failure-reasoning-in-llm-based-multi-agent-systems)  
   标签：评分：9.0/10、query:agent-errors
   evidence：使用双视角因果启发归因，定位LLM多智能体系统中导致失败的最早关键错误
15. [Constructing and Evaluating Clinical Reasoning Trajectories for Medical Agent](/202609/09/2609.05090v1-constructing-and-evaluating-clinical-reasoning-trajectories-for-medical-agent)  
   标签：评分：9.0/10、query:agent-traj
   evidence：构建临床推理轨迹并多维度评分以评估医学智能体推理质量
16. [How to Speculate about Uncertainty in Agentic Coding? A Draft-Model Gate Method](/202609/09/2609.05274v1-how-to-speculate-about-uncertainty-in-agentic-coding-a-draft-model-gate-method)  
   标签：评分：9.0/10、query:agent-errors
   evidence：使用小型草稿模型对智能体已生成的轨迹进行评分，以预测失败可能性
17. [Agents Trust Tools Too Much: Measuring Reliance on Unreliable Tools](/202609/09/2609.05587v1-agents-trust-tools-too-much-measuring-reliance-on-unreliable-tools)  
   标签：评分：9.0/10、query:agent-errors
   evidence：测量智能体采纳被污染工具返回的比例，揭示工具使用中的过度信任与幻觉
18. [Evaluating Deep-Search Agents under Hierarchical Web Evidence Poisoning](/202609/09/2609.06027v1-evaluating-deep-search-agents-under-hierarchical-web-evidence-poisoning)  
   标签：评分：9.0/10、query:agent-errors
   evidence：提出从证据暴露到恢复的轨迹级基准，检验智能体是否能验证可疑证据、修正主张并恢复
19. [DAREBench: Deployment-Aware and Reliable Evaluation of Models as Agents](/202609/09/2609.06059v1-darebench-deployment-aware-and-reliable-evaluation-of-models-as-agents)  
   标签：评分：9.0/10、query:agent-traj
   evidence：提供233个多步骤智能体任务与统一执行环境的大规模评估基准，强调部署感知与可靠性
20. [Shortcutting the Fix: Identifying and Categorizing Agentic Exploits in Software Engineering Benchmarks](/202609/09/2609.06780v1-shortcutting-the-fix-identifying-and-categorizing-agentic-exploits-in-software-engineering-benchmarks)  
   标签：评分：9.0/10、query:agent-traj
   evidence：在回合级对软件工程智能体执行轨迹进行系统审计，识别并分类基准中的漏洞利用行为
21. [AURA-Eval: Evaluation Framework for Acting Under Risk Awareness in LLM Agent Trajectories](/202609/09/2609.06783v1-aura-eval-evaluation-framework-for-acting-under-risk-awareness-in-llm-agent-trajectories)  
   标签：评分：9.0/10、query:agent-traj
   evidence：针对LLM智能体工具使用轨迹风险感知和行为的评估框架
22. [Skynet: Workflow-Level Anomaly Detection for Agentic AI via Semantic and Structural Modeling](/202609/09/2609.06835v1-skynet-workflow-level-anomaly-detection-for-agentic-ai-via-semantic-and-structural-modeling)  
   标签：评分：9.0/10、query:agent-errors
   evidence：面向智能体AI的工作流级异常检测，针对错误传播与级联步骤失败
23. [AgentDrift: A Step-Labeled Benchmark of Injection-Hijacked LLM Agent Trajectories](/202609/09/2609.06972v1-agentdrift-a-step-labeled-benchmark-of-injection-hijacked-llm-agent-trajectories)  
   标签：评分：9.0/10、query:agent-traj
   evidence：提供大规模逐步骤标注的工具调用轨迹基准，用于智能体轨迹安全测试
24. [APPSim-Bench: Bridging Real-world Apps and Reproducible Evaluation for Mobile GUI Agents](/202609/09/2609.07712v1-appsim-bench-bridging-real-world-apps-and-reproducible-evaluation-for-mobile-gui-agents)  
   标签：评分：9.0/10、query:agent-traj
   evidence：面向移动GUI智能体的可控模拟应用基准，支持确定性任务评测
25. [The Unreliable Progress Bar: Can LLM Agents Reliably Report Task Progress Throughout Execution?](/202609/09/2609.08589v1-the-unreliable-progress-bar-can-llm-agents-reliably-report-task-progress-throughout-execution)  
   标签：评分：9.0/10、query:agent-traj
   evidence：在基准和受控测试带上系统评估LLM智能体各轨迹阶段的自我进度报告可靠性
26. [Closing the Consistency Gap: Self-Evolving Agents That Learn to Stay on Course](/202609/09/2609.08832v1-closing-the-consistency-gap-self-evolving-agents-that-learn-to-stay-on-course)  
   标签：评分：9.0/10、query:agent-errors
   evidence：一致性分析器检测LLM智能体轨迹中的不稳定低一致性步骤并用于自纠正
27. [ExecCritic: Learn to Test, Test to Improve for Coding Agents](/202609/09/2609.09133v1-execcritic-learn-to-test-test-to-improve-for-coding-agents)  
   标签：评分：9.0/10、query:agent-errors
   evidence：通过test-verify-revise脚手架分离测试与修复，并利用执行反馈实现LLM智能体的自动纠错。

### 速读区论文标签
1. [HarnessEvolve: Learning from Reference Trajectories for Reliable Agent Self-Evolution](/202609/09/2609.00829v1-harnessevolve-learning-from-reference-trajectories-for-reliable-agent-self-evolution)  
   标签：评分：8.0/10、query:agent-errors
   evidence：用参考轨迹学习解决自进化智能体的信用分配失败与步骤级错误归因
2. [Parsing the Stream: A Live Trace Model for Long-Horizon Agents and Their Observers](/202609/09/2609.01466v1-parsing-the-stream-a-live-trace-model-for-long-horizon-agents-and-their-observers)  
   标签：评分：8.0/10、query:agent-traj
   evidence：将长时程智能体轨迹实时编译为可查询的运行状态与视窗，帮助人类观察者以更低成本和更高准确率监控运行
3. [Detecting Object Hallucinations in Large Vision-Language Models via Cross-Modal Attention Drifts and Mask-Based Verification](/202609/09/2609.02028v1-detecting-object-hallucinations-in-large-vision-language-models-via-cross-modal-attention-drifts-and-mask-based-verification)  
   标签：评分：8.0/10、query:agent-output
   evidence：基于跨模态注意力漂移与目标掩码验证检测物体级幻觉
4. [SafeEvolve: Harness-Policy Co-Evolution from Agent Experience for Safety Alignment](/202609/09/2609.02786v1-safeevolve-harness-policy-co-evolution-from-agent-experience-for-safety-alignment)  
   标签：评分：8.0/10、query:agent-output
   evidence：针对大模型智能体最终回复与多步执行轨迹的安全对齐
5. [Do GUI Agents Know When Not to Act? Enabling Conflict-Aware Termination for Multimodal GUI Agents](/202609/09/2609.03438v1-do-gui-agents-know-when-not-to-act-enabling-conflict-aware-termination-for-multimodal-gui-agents)  
   标签：评分：8.0/10、query:agent-traj
   evidence：提出冲突感知终止的基准与框架，测试GUI智能体在自主轨迹中的行为。
6. [Defense-as-Skill: Evolving Runtime Guard Skill for Skill-Augmented Agents](/202609/09/2609.01487v1-defense-as-skill-evolving-runtime-guard-skill-for-skill-augmented-agents)  
   标签：评分：7.0/10、query:agent-output
   evidence：运行时守卫技能按用户任务边界检查智能体敏感输出与动作以保障安全
7. [When Agents Implement Systems: A Case Study in Defects, Detection, and Evaluation Rigor](/202609/09/2609.01985v1-when-agents-implement-systems-a-case-study-in-defects-detection-and-evaluation-rigor)  
   标签：评分：7.0/10、query:agent-traj
   evidence：对LLM编程智能体的系统级实现进行案例研究，记录缺陷并分析检测与评估方法
8. [Towards Trustworthy Autonomous Robots: An Explainable AI-Based Decision Framework](/202609/09/2609.02861v1-towards-trustworthy-autonomous-robots-an-explainable-ai-based-decision-framework)  
   标签：评分：7.0/10、query:agent-traj
   evidence：通过四层可审计决策框架和因果链溯源，使自主机器人的每个动作都能追溯到传感证据，支持行为验证与审计
9. [Fresh Memory, Stale Plans: Dependency-Scoped Validation for Distributed LLM-Agent Memory](/202609/09/2609.03340v1-fresh-memory-stale-plans-dependency-scoped-validation-for-distributed-llm-agent-memory)  
   标签：评分：7.0/10、query:agent-errors
   evidence：通过校验LLM智能体决策所依赖的记录，防止执行过时计划
10. [SoK: When Safe Agents Fail Together: The Security of Multi Agent LLM Systems](/202609/09/2609.00595v1-sok-when-safe-agents-fail-together-the-security-of-multi-agent-llm-systems)  
   标签：评分：6.0/10、query:agent-errors
   evidence：以执行为中心系统化多智能体LLM失败、攻击传播、系统级风险与防御
11. [REVISE: Validity-Guided Recovery for Online Revisions in Agent Workflows](/202609/09/2609.00643v1-revise-validity-guided-recovery-for-online-revisions-in-agent-workflows)  
   标签：评分：6.0/10、query:agent-errors
   evidence：防止过时状态传播到智能体输出与工具效应的有效性引导恢复机制
12. [EmbodiedSkills: A Unified Framework for Orchestrating, Training, and Deploying VLA Agents](/202609/09/2609.01281v1-embodiedskills-a-unified-framework-for-orchestrating-training-and-deploying-vla-agents)  
   标签：评分：6.0/10、query:agent-traj
   evidence：对长期自主VLA智能体技能执行提案进行运行时前提检查与结果验证
13. [CAPTURE: Disentangling Preference Drift from Memory Poisoning in Personalized LLM Agents](/202609/09/2609.02265v1-capture-disentangling-preference-drift-from-memory-poisoning-in-personalized-llm-agents)  
   标签：评分：6.0/10、query:agent-output
   evidence：检测个性化LLM智能体中的异常记忆更新以保障输出安全


<div class="dpr-home-promo-card">
  <h3 class="dpr-home-promo-title">💬 社区与支持</h3>
  <ul class="dpr-home-promo-list">
    <li>欢迎 Star / Fork / Issue / PR</li>
    <li>QQ群：583867967（欢迎交流，已有：1151人）</li>
  </ul>
</div>
