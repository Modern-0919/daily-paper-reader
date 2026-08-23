<div class="dpr-home-notice-card">
  <h3 class="dpr-home-notice-title">🚀 Start Here</h3>
  <ul class="dpr-home-notice-list">
    <li><a href="#/tutorial/README">使用教程</a></li>
  </ul>
</div>

## 每次日报
- 最新运行日期：2026-08-23
- 运行时间：2026-08-23 19:49:10 UTC
- 运行状态：成功
- 本次总论文数：28
- 精读区：15
- 速读区：13

### 今日简报（AI）
今日精读聚焦长程智能体失败归因，重点推荐《LongRCA Bench》对责任角色与根因的高分诊断；速读另涉及循环策略进化、编码代理路径测试与机器人验证层。建议关注长程任务中智能体的可解释性评估，以提升复杂场景下的可靠性。
- 详情：[/202608/23/README](/202608/23/README)

### 精读区论文标签
1. [LongRCA Bench: Diagnosing Responsible Roles and Root Causes in Long-Horizon Agent Failures](/202608/23/2608.15242v2-longrca-bench-diagnosing-responsible-roles-and-root-causes-in-long-horizon-agent-failures)  
   标签：评分：10.0/10、query:agent-errors
   evidence：用于长时程智能体轨迹中故障归因与根因步骤定位的基准
2. [LongRCA Bench: Diagnosing Responsible Roles and Root Causes in Long-Horizon Agent Failures](/202608/23/2608.15242v1-longrca-bench-diagnosing-responsible-roles-and-root-causes-in-long-horizon-agent-failures)  
   标签：评分：9.0/10、query:agent-errors
   evidence：长时程智能体失败根因与责任角色诊断基准
3. [Agent Gym: A Framework for Continuous Evaluation and Evolution of LLM Agents Through Human-in-the-Loop Feedback](/202608/23/2608.15591v1-agent-gym-a-framework-for-continuous-evaluation-and-evolution-of-llm-agents-through-human-in-the-loop-feedback)  
   标签：评分：9.0/10、query:agent-traj
   evidence：对LLM代理进行持续评估与演化
4. [Hallucination Span Detection with Input-Side Evidence Alignment](/202608/23/2608.15804v1-hallucination-span-detection-with-input-side-evidence-alignment)  
   标签：评分：9.0/10、query:agent-output
   evidence：联合检测幻觉区间并对齐输出词元与输入证据，直接支持智能体输出中的幻觉检测
5. [From Sequence to Structure: Relational Uncertainty Propagation for LLM Agents](/202608/23/2608.16002v1-from-sequence-to-structure-relational-uncertainty-propagation-for-llm-agents)  
   标签：评分：9.0/10、query:agent-errors
   evidence：轨迹级不确定性传播，用于识别LLM智能体执行中由长程错误累积导致的失败
6. [From Sequence to Structure: Relational Uncertainty Propagation for LLM Agents](/202608/23/2608.16002v2-from-sequence-to-structure-relational-uncertainty-propagation-for-llm-agents)  
   标签：评分：9.0/10、query:agent-errors
   evidence：提出轨迹级不确定性传播框架，跨步骤检测智能体失败与错误累积
7. [HalluTracer: Hallucination Detection via Depth-Averaging Truth Signals](/202608/23/2608.16353v1-hallutracer-hallucination-detection-via-depth-averaging-truth-signals)  
   标签：评分：9.0/10、query:agent-output
   evidence：白盒幻觉检测，跨层聚合真值信号
8. [Towards Risk-free AI Agent Deployment](/202608/23/2608.16411v1-towards-risk-free-ai-agent-deployment)  
   标签：评分：9.0/10、query:agent-traj
   evidence：倡导以执行轨迹为基础的智能体测试与调试，讨论oracle问题与轨迹验证
9. [Auditing Self-Evolution in Financial Agents: Capability Gains, Security Drift, and Execution-Interface Mismatch](/202608/23/2608.17684v1-auditing-self-evolution-in-financial-agents-capability-gains-security-drift-and-execution-interface-mismatch)  
   标签：评分：9.0/10、query:agent-traj
   evidence：使用执行接地检查和独立状态重放对匹配轨迹进行智能体审计
10. [Mixture-of-Expert Blocks Contain Strong Hallucination Detection Signals](/202608/23/2608.17687v1-mixture-of-expert-blocks-contain-strong-hallucination-detection-signals)  
   标签：评分：9.0/10、query:agent-output
   evidence：利用MoE内部路由信号进行逐词元幻觉检测
11. [Beyond Suspicious Steps: Ontological Trust in Long-Horizon Agents](/202608/23/2608.17718v1-beyond-suspicious-steps-ontological-trust-in-long-horizon-agents)  
   标签：评分：9.0/10、query:agent-traj
   evidence：提出本体论信任作为轨迹前缀的任务条件属性，并实例化为在线监控器RGE检测轨迹漂移
12. [ComponentBench: Diagnosing Component-Level Failures in Computer-Use Agents](/202608/23/2608.18307v1-componentbench-diagnosing-component-level-failures-in-computer-use-agents)  
   标签：评分：9.0/10、query:agent-traj
   evidence：提供包含2910个任务和人工参考轨迹的基准，用于诊断计算机使用智能体的组件级故障
13. [LEDGER: Claim-to-Evidence Trace Graphs for Auditing LLM Agents](/202608/23/2608.18398v1-ledger-claim-to-evidence-trace-graphs-for-auditing-llm-agents)  
   标签：评分：9.0/10、query:agent-errors
   evidence：构建分层追踪图来核验LLM智能体输出与证据的一致性，确保正确可信
14. [Beyond LLM-Based Reasoning: Lightweight GNNs for Agent Failure Attribution](/202608/23/2608.18575v1-beyond-llm-based-reasoning-lightweight-gnns-for-agent-failure-attribution)  
   标签：评分：9.0/10、query:agent-errors
   evidence：提出轻量级GNN方法，对多智能体轨迹进行失败归因，识别故障智能体与错误类型
15. [One Success Isn't Reliability: Thinkingbox, a Sandbox and Benchmark for Agents in Stateful Business Workflows](/202608/23/2608.19741v1-one-success-isnt-reliability-thinkingbox-a-sandbox-and-benchmark-for-agents-in-stateful-business-workflows)  
   标签：评分：9.0/10、query:agent-traj
   evidence：提供完整执行轨迹与结果评估的智能体测试沙盒

### 速读区论文标签
1. [OpenLoopEvolve: A Verifiable Self-Evolution Framework for Loop Policies in Long-Horizon Complex Tasks](/202608/23/2608.09380v1-openloopevolve-a-verifiable-self-evolution-framework-for-loop-policies-in-long-horizon-complex-tasks)  
   标签：评分：8.0/10、query:agent-errors
   evidence：面向长时程任务的可验证自进化框架，包含验证与故障恢复
2. [SpecPath: Testing Coding Agents Across Contract-Equivalent Specification Histories](/202608/23/2608.09799v1-specpath-testing-coding-agents-across-contract-equivalent-specification-histories)  
   标签：评分：8.0/10、query:agent-traj
   evidence：在契约等价规范历史下对编码智能体进行诊断式测试
3. [Agentic Harnesses: LLM-Driven Verification Layers for Robot Autonomy](/202608/23/2608.09857v1-agentic-harnesses-llm-driven-verification-layers-for-robot-autonomy)  
   标签：评分：8.0/10、query:agent-errors
   evidence：提出LLM驱动的验证层，在执行前检查智能体规划动作的可行性与安全性
4. [UserToolBench: A User-Profile-Hidden Benchmark for Personalized Decision Making in Tool-Use LLMs](/202608/23/2608.10042v1-usertoolbench-a-user-profile-hidden-benchmark-for-personalized-decision-making-in-tool-use-llms)  
   标签：评分：8.0/10、query:agent-traj
   evidence：基准测试工具使用LLM中个性化决策的工具调用轨迹
5. [When Visual Signals Mislead: A Mechanistic Study of Attribute Hallucination in Vision-Language Models](/202608/23/2608.11024v1-when-visual-signals-mislead-a-mechanistic-study-of-attribute-hallucination-in-vision-language-models)  
   标签：评分：8.0/10、query:agent-output
   evidence：对视觉语言模型输出中属性幻觉的机制研究与诊断
6. [Hierarchical Agentic Incident Response with Digital-Twin-Validated Attack Inference](/202608/23/2608.15016v1-hierarchical-agentic-incident-response-with-digital-twin-validated-attack-inference)  
   标签：评分：7.0/10、query:agent-errors
   evidence：数字孪生验证缓解LLM智能体幻觉攻击与响应
7. [TRACE: Trajectory Aware Reasoning for Multi-Turn Adversarial Conversation Evaluation](/202608/23/2608.15594v1-trace-trajectory-aware-reasoning-for-multi-turn-adversarial-conversation-evaluation)  
   标签：评分：7.0/10、query:agent-traj
   evidence：利用轨迹感知的结构化推理评估多轮对抗对话，防御越狱攻击
8. [A Policy Algebra for Trust-Preserving Agentic AI Execution](/202608/23/2608.16402v1-a-policy-algebra-for-trust-preserving-agentic-ai-execution)  
   标签：评分：7.0/10、query:agent-errors
   evidence：用政策代数校验执行轨迹的合规性
9. [SkillEffect: Checked Lowering for Memory-Bounded Agent Tools](/202608/23/2608.17007v1-skilleffect-checked-lowering-for-memory-bounded-agent-tools)  
   标签：评分：7.0/10、query:agent-errors
   evidence：在执行前校验智能体工具程序的降级过程，避免工具调用的内存溢出错误
10. [JarvisBench: Always-on Intelligence Between Humans and Agents](/202608/23/2608.14870v1-jarvisbench-always-on-intelligence-between-humans-and-agents)  
   标签：评分：6.0/10、query:agent-traj
   evidence：评估长时程智能体与人机协作行为
11. [Harness the Memory: A Holistic Evaluation of Memory Substrates in Memory Agents](/202608/23/2608.15008v1-harness-the-memory-a-holistic-evaluation-of-memory-substrates-in-memory-agents)  
   标签：评分：6.0/10、query:agent-traj
   evidence：对记忆增强智能体的记忆基板进行整体评估
12. [When Tool-Backed Skill Retrieval Fails: Source-Style Collapse in Executable Capability Retrieval](/202608/23/2608.16502v1-when-tool-backed-skill-retrieval-fails-source-style-collapse-in-executable-capability-retrieval)  
   标签：评分：6.0/10、query:agent-errors
   evidence：智能体技能检索中的工具选择失败模式
13. [Zetta $ζ$: An Efficient Closed-Loop Embodied Harness for Self-Evolving Physical Intelligence](/202608/23/2608.16590v1-zetta--an-efficient-closed-loop-embodied-harness-for-self-evolving-physical-intelligence)  
   标签：评分：6.0/10、query:agent-errors
   evidence：运行时批评与恢复技能实现在线自纠错


<div class="dpr-home-promo-card">
  <h3 class="dpr-home-promo-title">💬 社区与支持</h3>
  <ul class="dpr-home-promo-list">
    <li>欢迎 Star / Fork / Issue / PR</li>
    <li>QQ群：583867967（欢迎交流，已有：1151人）</li>
  </ul>
</div>
