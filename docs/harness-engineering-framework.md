# Harness Engineering（驾驭工程）终极框架 v2.0

> **版本**：v2.0 | **来源**：Mitchell Hashimoto (2026.02) + OpenAI百万行代码实验 + Anthropic Agent实践 + LangChain Terminal Bench 2.0 + 5大前沿补充系统
> **核心公式**：`Agent = LLM + Harness`
> **核心哲学**：**Humans steer. Agents execute.**（人类掌舵，智能体执行）
> **终极愿景**：构建一个分层、可演进、可自愈的AI Agent操作系统

---

## 一、范式演进全景图

```
┌─────────────────────────────────────────────────────────────────┐
│                    Agentic Stack 终极分层                        │
├─────────────────────────────────────────────────────────────────┤
│ 第五层 │ Intention Engineering（意图工程）  ← 终极目标           │
│        │ 人类只需设定目标，AI自主完成全链路                        │
├─────────────────────────────────────────────────────────────────┤
│ 第四层 │ Coordination Engineering（协同工程） ← 多Agent协作      │
│        │ 海量Agent通信、分工、管控                                │
├─────────────────────────────────────────────────────────────────┤
│ 第三层 │ Harness Engineering（驾驭工程）← 我们当前层级           │
│        │ 约束/反馈回路/控制系统/熵管理                            │
├─────────────────────────────────────────────────────────────────┤
│ 第二层 │ Context Engineering（上下文工程）← 我们已集成            │
│        │ 5大上下文管理模式（见第二章）                           │
├─────────────────────────────────────────────────────────────────┤
│ 第一层 │ Prompt Engineering（提示词工程）← 基础层                │
│        │ 指令措辞/格式/示例                                       │
└─────────────────────────────────────────────────────────────────┘

范式         年代      核心问题           优化对象
─────────────────────────────────────────────────────────
Prompt       2022-2024 怎么对AI说       提示词措辞/格式/示例
Context      2025     给AI看什么        文档/代码/历史/工具定义
Harness      2026     怎么让AI可靠干活  约束/反馈/控制/熵管理
Coordination 2026+    多Agent如何协作   通信协议/分工/冲突仲裁
Intention    未来      人类只需定目标   意图理解/自主规划/自我进化
```

---

## 二、五大上下文管理模式（Context Engineering 补充）

> 来源：Anthropic Agent Skills规范 + Aurimas Griciūnas深度分析 + Manus生产实践

### 模式一：渐进式披露（Progressive Disclosure）

**核心原理**：传统方案将所有指令一次性加载，但对多域Agent而言是浪费。

**三层加载机制**：
```
Discovery层（预加载）   →  仅加载name和description，约80 tokens/技能
Activation层（按需）   →  判断相关时加载完整指令体
Execution层（执行时）   →  仅在任务执行期间加载脚本和参考资料
```

**对我们系统的意义**：
- 影视系统：7步流水线每步按需加载对应参考文件
- 全栈构建师：执行前端时预加载前端知识，后端按需激活
- 68人剧组：不预加载全部68人，按场景调度激活对应剧组成员

**Agent Identity Management（重要延伸）**：
Agent不再需要为每个域spin up独立sub-agent，而是single agent在需要时activate相应skill，adopting该skill的指令、constraints、tone和behavioral patterns。任务完成后返回base identity。这正是我们68人剧组的核心设计逻辑。

### 模式二：上下文压缩（Context Compression）

**核心原理**：每个tool call、每个observation、每个reasoning step都在累积context。当context window快满时，有效信息被稀释，模型precision下降。

**三种压缩方案**：
| 方案 | 实现 | 权衡 |
|------|------|------|
| 硬截断 | Keep only top N turns | 简单但丢失早期关键信息 |
| 滑动窗口 | 保持最近N轮raw，用LLM summarization压缩早期 | **主流方案** |
| 长期记忆 | 早期history移至durable storage，按需检索 | 适合超长任务 |

**Manus两个关键实践细节**：
1. **保留最近tool calls的raw format**：不要压缩最近tool call的原始输出，保留模型的"rhythm"和formatting style。失去rhythm会导致subtle degradation。
2. **保留error traces**：当tool call失败时，将error和stack trace保留在上下文中，帮助model避免重复相同的错误。

**对我们系统的意义**：
- 影视创作：长剧本项目保留最近5个镜头的原始提示词，压缩早期镜头
- 全栈项目：保留最近3次代码修改的完整diff，压缩早期历史
- 剧组调度：保留最近一次剧组输出的完整方案，压缩历史讨论

### 模式三：上下文路由（Context Routing）

**核心原理**：多域Agent通常有多个knowledge bases、tool sets和instruction sets。为每个query加载所有域的context是浪费。

**三种路由方法**：
| 方法 | 实现 | 权衡 |
|------|------|------|
| LLM-powered routing | 用模型本身对query分类 | 准确率高，但增加latency和cost |
| Hierarchical routing | lead agent triage到specialized sub-agents | 每个sub-agent有focused context window |
| Rule-based routing | keyword matching或pattern detection | 快速可预测，但rigid |

**对我们系统的意义**：
- 用户说"写代码" → 路由到全栈构建师context
- 用户说"影视系统" → 路由到创作助手context + 激活68人剧组
- 用户说"诺兰视角" → 路由到导演视角库+诺兰心智模型

### 模式四：演进检索（Evolved Retrieval）

**核心原理**：固定pipeline（query → vector search → inject → generate）无法处理复杂query。

**三种演进方向**：
| 方向 | 核心能力 | 权衡 |
|------|---------|------|
| Agentic RAG | Agent控制自己的search strategy，可迭代直到confident | 准确率最强，但latency/token cost高 |
| Graph RAG | 构建entity-relationship graphs，支持跨文档关系推理 | 适合主题分析等复杂场景 |
| Self-RAG | 模型自行决定何时检索，批判自己的输出 | 训练模型，当前实用性受限 |

**对我们系统的意义**：
- 风格检索：用户说"诺兰+王家卫混合风格" → Graph RAG跨风格库连接
- 剧本分析：跨多集提取主题 → Agentic RAG迭代检索
- 平台对比：跨10大平台提取最佳参数 → Self-RAG自我评估

### 模式五：工具管理（Tool Management）

**核心问题**：MCP解决了连接问题，但没有解决context cost问题。OpenAI推荐每个Agent少于20个tools，90+ tool definitions意味着超过50,000 tokens的schemas在model开始推理前就已消耗。

**四个未解决的工程问题**：
1. **Description quality**：大多数MCP server作者为人类编写tool descriptions，而非为模型
2. **Tool overlap**：不同server提供相似capabilities，模型arbitrary选择
3. **No versioning**：MCP server更新时，Agent无法知道，cache中的stale descriptions导致silent failures
4. **Security surface**：每个连接的MCP server都是attack surface

**对我们系统的意义**：
- 平台桥接器：每个平台工具必须有精准的description，避免选择混乱
- 68人剧组：每个剧组Skill的工具集必须独立且无重叠
- 平台适配器：版本更新时自动同步，避免silent failures

### 五模式分层组合

```
┌──────────────────────────────────────────────────────────────┐
│ Tool Management          │ 定义什么可以进入context window   │
│ (MCP token audit, schema quality)                           │
├──────────────────────────────────────────────────────────────┤
│ Progressive Disclosure  │ 定义什么可以进入context window   │
│ (Agent Skills, identity management)                         │
├──────────────────────────────────────────────────────────────┤
│ Context Routing          │ 管理执行期间保留什么              │
│ (query classification, domain routing)                       │
├──────────────────────────────────────────────────────────────┤
│ Context Compression       │ 管理执行期间保留什么              │
│ (sliding window + summarization)                            │
├──────────────────────────────────────────────────────────────┤
│ Evolved Retrieval         │ 按需从外部获取知识                │
│ (Agentic RAG + Graph RAG + Self-RAG)                       │
└──────────────────────────────────────────────────────────────┘
```

---

## 三、四大护栏（The Four Guardrails）

> 来自Mitchell Hashimoto + OpenAI + Anthropic实践

### 护栏一：上下文工程（Context Engineering）——新员工手册

- AGENTS.md / SKILL.md是AI进入工作环境看到的第一份指南
- **动态优于静态**：提供稳定小巧入口点，按需检索拉取更多上下文
- **上下文是稀缺资源**：巨大指令文件会挤掉任务空间，按需注入
- **文档是活的反馈循环**：每一行对应一个历史失败案例，不是静态制品

### 护栏二：架构约束（Architecture Constraints）——缰绳

- 分层依赖模型，下层不能反向依赖上层
- 架构规则编码为Linter/CI，违反即阻断
- **Linter错误信息本身也是上下文**：不只说违反了什么，还解释为什么存在、正确做法是什么
- Agent读到错误后能自我理解并修正，无需人类介入

### 护栏三：反馈循环（Feedback Loop）——智能体审智能体

- Agent-to-Agent Review：AI审核自身更改，循环直到通过
- 失败时带着错误信息循环回到模型
- 如果AI写的测试通过了有Bug的代码，Harness判定测试无效，强迫重新思考
- **验证即约束**：有效验证可让输出质量提升2-3倍

### 护栏四：熵管理（Entropy Management）——垃圾回收

- 技术债务像高息贷款，持续小额偿还优于集中处理
- 定期扫描偏差、更新质量等级、发起针对性重构
- 文档园丁（Doc-gardening Agent）：自动扫描文档与代码不一致，发现过时内容自动修复
- **Agent为Agent维护文档**

---

## 四、六大行业共识

| # | 共识 | 核心观点 |
|---|------|---------|
| 1 | 瓶颈在基础设施，不在模型智能 | 仅改变Harness工具格式，模型得分从6.7%跳到68.3% |
| 2 | 文档必须是活的反馈循环 | 静态文档是坟场，动态文档才有价值 |
| 3 | 思考与执行分离 | 复杂任务需Orchestrator+Worker分层，状态持久化 |
| 4 | 上下文不是越多越好 | 巨大指令文件挤掉任务空间，应按需检索动态注入 |
| 5 | 约束必须自动化 | 人工Review是瓶颈，护栏编码为Linter/CI/类型系统 |
| 6 | 工程师角色在转变 | 从代码编写者→环境建筑师 |

---

## 五、Agent常见失败模式与Harness对策

| 失败模式 | 表现 | Harness对策 |
|---------|------|------------|
| 试图一步到位 | 上下文耗尽，留下一堆半成品 | 强制分步执行，每步有验证门 |
| 过早宣布胜利 | 看到部分进展就标记完成 | 完成度检查清单，端到端验证 |
| 过早标记功能完成 | 写完代码就标记完成，不做测试 | 强制测试+验证循环 |
| 模式复制放大 | 忠实复制坏模式和架构漂移 | Linter约束+架构规则自动阻断 |
| 无限循环 | 陷入重复操作，累积高额账单 | 最大迭代次数+成本监控+自动终止 |
| 上下文腐烂 | 长任务执行到一半忘记初始约束 | 上下文压缩+状态持久化+定期回检 |
| Lost-in-the-middle | 上下文太长，模型错过中间关键信息 | 渐进式披露+保留最近raw+路由优化 |
| 工具选择混乱 | 90+工具，模型arbitrary选择 | 工具子集原则+精准description+路由 |

---

## 六、实战数据

| 案例 | 改变 | 效果 |
|------|------|------|
| Nate B Jones | 同一模型仅优化Harness | 编程成功率 42%→78% |
| LangChain | 加入防死循环+自我验证 | Terminal Bench 52.8%→66.5%，Top30→Top5 |
| Vercel | 移除80%复杂工具，保留基础终端 | 准确率100%，Token成本降40%，速度提升3.5x |
| Pi Research | 一个下午修改Harness | 同时提升15个不同LLM的编程能力 |
| 行业实测 | 注入Harness | 医疗领域通过率暴增51.9个百分点 |

---

## 七、实施原则

### 7.1 Start Simple. Build to Delete.
- 保持轻量化、模块化
- 随时准备迭代重构
- 模型能力越强，Harness越"薄"——但永远存在

### 7.2 护栏悖论
- 车速越快，护栏越重要
- 模型能力越强，越需要精心设计的约束系统
- **增加信任需要的不是更多自由，而是更多限制**

### 7.3 错误复盘驱动
```
每次Agent犯错 → 记录错误 → 转化为约束规则 → 工程化落地 → 永不再犯同类错误
```

### 7.4 渐进式披露原则
```
Discovery层（预加载80 tokens） → Activation层（按需加载完整指令） → Execution层（仅执行时加载脚本）
```

### 7.5 上下文压缩原则
```
保持最近N轮raw → LLM summarization压缩早期 → 保留error traces → 定期持久化到外部存储
```

---

## 八、各系统终极适配

### 8.1 全栈构建师 Harness

- **上下文工程**：references/按需加载，不一次性塞入所有知识
- **渐进式披露**：Discovery仅预加载name+description，Activation加载完整指令，Execution加载脚本
- **上下文压缩**：保留最近3次代码修改raw diff，压缩早期历史，保留error traces
- **上下文路由**：query分类导向（前端context/后端context/架构context）
- **架构约束**：分层依赖(Types→Config→Repo→Service→Runtime→UI)，CI门禁
- **反馈循环**：代码审查→测试→验证→修正循环，最大迭代15次
- **熵管理**：定期清理过时文档，更新技术栈推荐
- **演进检索**：复杂问题用Agentic RAG，主题分析用Graph RAG

### 8.2 影视创作系统 Harness

- **上下文工程**：7步流水线每步自动加载对应参考文件，不预加载全部
- **渐进式披露**：Stage 0仅预加载风格库name+description，Stage 1激活角色资产指令，Stage 3激活分镜脚本
- **上下文压缩**：保留最近5个镜头的原始提示词raw，压缩早期镜头，保留失败案例的error traces
- **上下文路由**：用户说"古风"导向古风风格context，"科幻"导向科幻context
- **架构约束**：分镜16列标准不可省略，输出格式强制验证，平台适配器自动转换
- **反馈循环**：链式验证(CoV)，关键输出自我检查，一致性校验四重保障
- **熵管理**：风格库持续更新，防崩词库持续积累，平台蒸馏持续迭代
- **演进检索**：跨风格库连接用Graph RAG，角色一致性校验用Self-RAG

### 8.3 导演Pro Harness

- **上下文工程**：联网检索优先，本地模板仅作格式参考，按需检索不预加载
- **渐进式披露**：预加载检索策略name+description，按需激活完整技法库
- **上下文压缩**：保留最近5次检索的raw结果，压缩早期检索历史
- **架构约束**：强制联网检索流程(6步)，适用性拦截，检索纪律(禁止来源+优先来源)
- **反馈循环**：适用性校验(情绪冲突检测)，异常自愈(联网不可用→本地降级)
- **熵管理**：知识库持续铸造(联网→本地)，文档园丁自动清理过时参数
- **演进检索**：技法对比用Agentic RAG，跨导演风格连接用Graph RAG

### 8.4 平台桥 Harness

- **上下文工程**：平台能力按需加载，不一次性暴露全部工具
- **渐进式披露**：预加载平台列表name+capabilities，按需激活平台指令
- **上下文压缩**：保留最近3次平台调用raw结果，压缩早期调用历史
- **工具管理**：每个平台工具精准description，避免重叠，版本跟踪
- **架构约束**：工具白名单，API参数校验，权限边界
- **反馈循环**：跨平台结果互验，失败自动切换备选平台，最多切换3次
- **熵管理**：平台能力图谱持续更新，过时接口自动标记

### 8.5 68人剧组 Harness

- **上下文工程**：按需加载本剧组视角，不预加载全部68人
- **渐进式披露**：调度时仅预加载被激活剧组成员name+description，Activation加载完整心智模型
- **上下文路由**：任务类型导向（叙事→导演context，视觉→摄影context，声音→作曲context）
- **架构约束**：严格遵循本角色心智模型和决策启发式，不越界到其他部门
- **反馈循环**：输出必须包含"下游影响"字段，确保信息回流到下游部门
- **熵管理**：持续更新本角色知识，过时技法自动标记

---

## 九，检查清单（每个Skill必须包含）

- [ ] **上下文边界**：明确该Skill需要/不需要哪些上下文
- [ ] **渐进式披露**：Discovery/Activation/Execution三层加载策略
- [ ] **上下文压缩**：最近N轮raw保留策略，error traces保留策略
- [ ] **上下文路由**：query分类逻辑，导向哪个context
- [ ] **演进检索**：Agentic RAG / Graph RAG / Self-RAG适用场景
- [ ] **约束规则**：至少3条硬性约束（格式/质量/安全）
- [ ] **验证机制**：输出后如何自我验证
- [ ] **失败恢复**：出错时的降级/回滚策略
- [ ] **迭代上限**：最大循环次数，防止无限循环
- [ ] **熵管理**：如何保持知识/文档不腐烂

---

## 十，未来演进路线

| 阶段 | 系统 | 说明 |
|------|------|------|
| 当前 | Harness + Context Engineering | 已集成四大护栏+五大上下文模式 |
| 下一阶段 | Coordination Engineering | 多Agent协同，68人剧组互通，冲突仲裁 |
| 终极阶段 | Intention Engineering | 人类只需设定目标，AI自主完成全链路 |

---

*Harness Engineering 终极框架 v2.0 | 2026-05-07 | Mitchell Hashimoto + Anthropic + 5大前沿系统 | 灵枢系统适配*
