# 结构化流程树生成方法论与工具调研

> 本文由AI辅助生成 | 生成日期：2026-05-06
> 调研目的：为 iterative-flow AI skill 的分层流程树自动生成提供方法论基础和工具参考

---

## 目录

1. [结构化需求方法](#1-结构化需求方法)
2. [流程建模标准](#2-流程建模标准)
3. [AI辅助需求分析](#3-ai辅助需求分析)
4. [穷举完整性技术](#4-穷举完整性技术)
5. [交互细节捕获方法](#5-交互细节捕获方法)
6. [对 iterative-flow 的综合启示](#6-对-iterative-flow-的综合启示)

---

## 1. 结构化需求方法

### 1.1 用户故事地图（User Story Mapping）

**概述**

用户故事地图（User Story Mapping）由 Jeff Patton 创立，是一种以用户旅程为中心的可视化需求组织方法。它通过二维结构（水平轴=时间/用户旅程，垂直轴=优先级/细节深度）将需求组织成分层结构。

**核心分层结构**

| 层级 | 名称 | 描述 |
|------|------|------|
| 顶层 | **Backbone（骨干）** | 高层用户活动（Activities）构成产品旅程的主线，如"注册"、"搜索"、"结账" |
| 中层 | **Steps/Tasks（步骤/任务）** | 每个活动下的具体操作，如"输入邮箱"、"选择套餐" |
| 底层 | **Details（细节）** | 最细粒度的用户故事、边界条件、验收标准 |

**核心方法论步骤**

1. **确定用户画像**：明确目标用户是谁
2. **构建 Backbone**：梳理用户活动的时间序列
3. **逐层细化**：每个活动向下分解为任务和细节
4. **优先级排序**：按垂直方向划分发布切片（Release Slices）
5. **验证完整性**：确保用户旅程不遗漏关键步骤

**对流程树生成的启示**

- Backbone 结构与 iterative-flow 的 L1 主流程高度吻合
- "活动→步骤→细节"的三层结构与 L1→L2→L3 对应
- 验证完整性的方法可用于检查流程树是否遗漏关键路径

**来源**
- [Jeff Patton Associates - User Story Mapping](https://jpattonassociates.com/story-mapping/)
- [Jeff Patton - The New User Story Backlog is a Map](https://jpattonassociates.com/the-new-backlog/)
- [Nielsen Norman Group - Mapping User Stories in Agile](https://www.nngroup.com/articles/user-story-mapping/)
- [StoriesOnBoard - User Story Mapping Basics](https://storiesonboard.com/user-story-mapping-basics.html)
- [Mountain Goat Software - How to Create Story Maps](https://www.mountaingoatsoftware.com/blog/user-story-mapping-how-to-create-story-maps)
- [Asana - User Stories Explained (2025)](https://asana.com/resources/user-stories)

---

### 1.2 事件风暴（Event Storming）

**概述**

事件风暴（Event Storming）由 Alberto Brandolini 发明，是一种快速、协作式的领域建模技术。它通过彩色便签纸在大型墙面上探索和建模复杂业务领域，特别适合 DDD（Domain-Driven Design）场景。

**核心便签颜色编码**

| 颜色 | 代表 | 示例 |
|------|------|------|
| 橙色 | **Domain Events（领域事件）** | "订单已创建"、"支付已收到" |
| 蓝色 | **Commands（命令）** | 触发事件的动作 |
| 黄色 | **Aggregates（聚合）** | 处理命令并产生事件的实体 |
| 紫色/粉色 | **Policies（策略）** | 响应事件并触发新命令的规则 |
| 绿色 | **Read Models（读模型）** | 决策所需的数据视图 |
| 红色 | **Hotspots（热点）** | 问题、疑问或冲突 |
| 白色（大） | **External Systems（外部系统）** | 外部依赖 |

**Workshop 步骤**

1. **混乱探索（Orange Phase）**：参与者尽可能多地在橙色便签上写下领域事件，不做评判
2. **时间线排序**：将领域事件按时间顺序排列
3. **丰富模型**：添加命令、聚合、策略、读模型等
4. **识别热点**：用红色便签标记不确定性区域
5. **划分 Bounded Context**：识别自然边界以确定子域

**对流程树生成的启示**

- 事件驱动的思维方式可以帮助识别流程中的关键转换点
- "事件→命令→策略"的模式可映射为流程节点的"触发条件→动作→规则"
- 红色热点方法可用于标记流程树中需要进一步细化的区域
- 事件风暴的穷举精神（尽可能多列举事件）与流程树穷举要求一致

**来源**
- [EventStorming Masterclass - DDD Europe 2024](https://2024.dddeurope.com/program/eventstorming-masterclass/)
- [Lucidspark - 8 Steps in the Event Storming Process](https://lucid.co/blog/8-steps-in-the-event-storming-process)
- [Qlerify - Why Event Storming](https://www.qlerify.com/post/why-event-storming)
- [Medium - EventStorming for DDD: Strengths and Limitations](https://medium.com/@lambrych/eventstorming-for-domain-driven-design-strengths-and-limitations-3f0b49009c38)
- [SoftwareMill - Event Storming from a Non-Technical Perspective](https://softwaremill.com/event-storming-from-a-non-technical-domain-expert-perspective/)

---

### 1.3 IEEE 830 / IEEE 29148 SRS Template

**概述**

IEEE 830-1998 是经典的软件需求规格说明（SRS）标准，已被 ISO/IEC/IEEE 29148:2018 取代和扩展。该标准定义了高质量需求文档的结构和特性要求。

**标准 SRS 文档结构**

| 章节 | 内容 |
|------|------|
| 1. Introduction | 目的、范围、定义、缩写、参考文献、概述 |
| 2. Overall Description | 产品视角、产品功能、用户特征、约束、假设与依赖 |
| 3. Specific Requirements | 功能需求、外部接口需求（用户/硬件/软件/通信）、性能需求、设计约束、逻辑数据库需求、软件系统属性 |
| 4. Supporting Information | 附录、目录、索引 |

**IEEE 29148 定义的三层需求文档**

- **StRS**（Stakeholder Requirement Specification）：利益相关方需求
- **SyRS**（System Requirements Specification）：系统需求
- **SRS**（Software Requirements Specification）：软件需求

**关键最佳实践**

1. 用 "Shall" 表示强制性需求，"Should" 表示推荐，"Will" 表示事实陈述
2. 每个需求必须是可测试的（通过检查、演示、分析或测试验证）
3. 保持完整性和一致性
4. 使用 MoSCoW 方法（Must/Should/Could/Won't）优先级排序
5. 定义验收标准
6. 维护双向可追溯性（利益相关方需求→系统需求→设计→测试用例）
7. 避免包含实现细节
8. 使用唯一标识符（如 REQ-FUNC-001）
9. 明确区分功能需求和非功能需求

**对流程树生成的启示**

- 需求的分层结构（StRS→SyRS→SRS）与流程树的分层结构思路一致
- 可追溯性矩阵方法可用于确保流程树每个节点都有对应的需求来源
- 唯一标识符的做法可用于流程树节点的编号和引用
- 接口需求的详细说明方法可指导交互行为的穷举

**来源**
- [IEEE Xplore - IEEE 830-1998](https://ieeexplore.ieee.org/document/720574)
- [IEEE SA - IEEE/ISO/IEC 29148-2018](https://standards.ieee.org/standard/29148-2018.html)
- [ReqView - ISO/IEC/IEEE 29148 Templates](https://www.reqview.com/doc/iso-iec-ieee-29148-templates/)
- [Well-Architected Guide - 29148 Template](https://www.well-architected-guide.com/documents/iso-iec-ieee-29148-template/)
- [GitHub - Markdown SRS Template](https://github.com/jam01/SRS-Template)
- [Jama Software - How to Write SRS](https://www.jamasoftware.com/requirements-management-guide/writing-requirements/system-requirements-specification/)

---

## 2. 流程建模标准

### 2.1 BPMN 2.0 建模方法与最佳实践

**概述**

BPMN（Business Process Model and Notation）2.0 是 OMG 制定的业务流程建模国际标准，提供了丰富的图形符号来描述业务流程中的活动、网关、事件和流。

**层次化分解（Hierarchical Decomposition）**

BPMN 支持通过 Sub-Process 实现层次化分解，将复杂流程逐层展开：

| 层级 | BPMN 元素 | 描述 |
|------|-----------|------|
| L1 | **Process（流程）** | 顶层业务流程，包含主要活动和网关 |
| L2 | **Sub-Process（子流程）** | 嵌套子流程，可折叠或展开 |
| L3+ | **Call Activity（调用活动）** | 引用其他独立图中定义的流程，支持跨图复用 |

**三层建模方法**

根据 BPMN 最佳实践，流程建模通常分为三个层次：

1. **组织/景观层（Organizational Level）**：展示流程之间的关系和价值链
2. **流程层（Process Level）**：描述具体的业务流程，包含活动、网关、事件
3. **工作流/任务层（Workflow/Task Level）**：细化到具体的操作步骤和规则

**关键最佳实践**

- 每个分解层级保持 **5~9 个活动**（经典可读性规则）
- 使用 Gateway 明确决策分支（Exclusive/Inclusive/Parallel）
- 用 Error Boundary Event 描述异常处理
- 避免在单层放置过多元素，优先使用子流程封装
- 使用 Pool 和 Lane 区分参与者和职责

**对流程树生成的启示**

- BPMN 的层次化分解机制直接对应 iterative-flow 的 L1→L2→L3 结构
- Gateway 类型（排他/包容/并行）可用于描述分支逻辑
- Boundary Event 机制可用于捕获异常分支
- "5~9 个活动/层" 的经验规则可用于控制节点数量
- Call Activity 的概念可对应超过5层深度时拆分为独立子树的需求

**来源**
- [Trisotech - BPMN Modeling Best Practices](https://www.trisotech.com/bpmn-modeling-best-practices/)
- [Camunda - BPMN 2.0 Symbols Reference](https://camunda.com/bpmn/reference/)
- [SmartGecko Academy - Three Levels of BPMN Modeling](https://www.smartgecko.academy/en/trois-niveaux-modelisation-processus-bpmn/)
- [GBTEC - BPMN 2.0 Modeling Rules](https://www.gbtec.com/wiki/process-management/bpmn/)
- [Signavio - BPMN Modeling Conventions](https://www.signavio.com/post/bpmn-modeling-conventions/)
- [OMG - BPMN Official Specification](https://www.omg.org/bpmn/)
- [Lucidchart - BPMN Tutorial](https://www.lucidchart.com/pages/tutorial/bpmn)
- [Modern Analyst - Efficient BPMN: Anti-Patterns to Best Practices](https://www.modernanalyst.com/Resources/Articles/tabid/115/ID/2438/Efficient-BPMN-from-Anti-Patterns-to-Best-Practices.aspx)

---

### 2.2 UML Activity Diagram / State Machine Diagram

**Activity Diagram（活动图）**

UML Activity Diagram 用于描述系统中的行为流程，适合建模：

- 工作流（Workflow）
- 业务流程（Business Process）
- 用例场景（Use Case Scenario）

**核心元素**：Action、Control Flow、Decision Node、Merge Node、Fork/Join（并行）、Initial/Final Node、Swimlane（分区）

**State Machine Diagram（状态机图）**

UML State Machine Diagram 用于描述对象在其生命周期中的状态变化，适合建模：

- 事件驱动行为（Event-Driven Behavior）
- 对象生命周期（Object Lifecycle）
- UI 状态转换（UI State Transitions）

**核心元素**：State、Transition、Event、Guard Condition、Action、Initial/Final State、Composite State（嵌套状态）

**两种图的对比**

| 维度 | Activity Diagram | State Machine Diagram |
|------|-----------------|----------------------|
| **焦点** | 活动流程（做了什么） | 状态变化（处于什么状态） |
| **触发方式** | 前一活动完成即触发 | 事件触发状态转换 |
| **适用场景** | 建模端到端流程 | 建模事件驱动的UI行为 |
| **异常处理** | 通过 Decision 分支 | 通过 Guard Condition |
| **层次化** | 通过 CallBehaviorAction | 通过 Composite State |

**对流程树生成的启示**

- Activity Diagram 适合建模主流程（L1）和子流程（L2），描述"做了什么"
- State Machine Diagram 适合建模 L3+ 的详细步骤，特别是 UI 交互行为
- Composite State（嵌套状态）的概念可用于深层节点组织
- Guard Condition 可用于描述条件分支和异常处理
- 并行（Fork/Join）结构可用于描述可同时执行的步骤

**来源**
- [Visual Paradigm - State Machine vs Activity Diagram](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/state-machine-diagram-vs-activity-diagram/)
- [University of Warwick - UML Activity Diagrams, State-Machine Diagrams (PDF)](https://warwick.ac.uk/fac/sci/physics/research/condensedmatt/imr_cdt/students/david_goodwin/teaching/modelling/l2_umlactivity.pdf)
- [Lucidchart - UML Activity Diagram Tutorial](https://www.lucidchart.com/pages/tutorial/uml-activity-diagram)
- [GeeksforGeeks - UML State Diagrams](https://www.geeksforgeeks.org/system-design/unified-modeling-language-uml-state-diagrams/)
- [Archimetric - UML State Machine Diagram](https://www.archimetric.com/uml-state-machine-diagram-concepts-examples-vs-activity-diagram/)
- [Miro - Understanding UML Activity Diagrams](https://miro.com/diagramming/what-is-a-uml-activity-diagram/)

---

### 2.3 建模标准对 AI 生成流程树的指导意义

综合 BPMN 和 UML 的实践，以下要素可直接指导 AI 生成流程树：

| 建模要素 | 对应流程树节点属性 | 来源标准 |
|----------|-------------------|----------|
| 活动名称 | 节点 key_step | BPMN Task, UML Action |
| 前置条件 | 节点 precondition | UML Guard Condition |
| 后继条件 | 节点 postcondition | BPMN Sequence Flow |
| 异常处理 | 节点 exception_branch | BPMN Boundary Event, UML Transition |
| 参与者 | 节点 actor | BPMN Lane, UML Swimlane |
| 时间约束 | 节点 time_constraint | BPMN Timer Event |
| 空间约束 | 节点 space_constraint | BPMN Data Object, UML Object Flow |
| 层次分解 | 子树拆分 | BPMN Sub-Process, UML Composite State |
| 并行行为 | 可并行步骤 | BPMN Parallel Gateway, UML Fork/Join |
| 决策分支 | 条件分支 | BPMN Exclusive Gateway, UML Decision Node |

---

## 3. AI辅助需求分析

### 3.1 LLM 从 PRD 自动生成流程图/状态机的研究

**研究现状**

近年来（2024-2025），学术界和工业界在利用 LLM 自动化需求分析和流程生成方面取得了显著进展：

**关键研究方向**

1. **LLM 生成数据流图（DFD）**
   - HAL Science 发表的研究论文探索了从用户故事自动生成 DFD 的方法
   - 通过增强指令（Enhanced Instructions）提升 LLM 的抽象能力和合规性
   - 来源：[HAL Science - Automating DFD Generation from User Stories (PDF)](https://hal.science/hal-04525925v1/file/Generating_Data_Flow_Diagram_with_LLM-4.pdf)

2. **多 Agent LLM 系统用于自动化需求分析**
   - ResearchGate 2024：提出多 Agent LLM 框架，用于用户故事生成和优先级排序
   - 来源：[ResearchGate - Multi-agent LLM System for Automated Requirements Analysis](https://www.researchgate.net/publication/395498762_A_Multi-agent_LLM_System_for_Automated_Requirements_Analysis_A_Study_on_User_Story_Generation_and_Prioritization)

3. **LLM 结构化分解推理**
   - arXiv 2025：提出产生可审查推理轨迹的架构，每个提取决策持久化为 ABox 断言
   - 来源：[arXiv - Structured Decomposition for LLM Reasoning](https://arxiv.org/pdf/2601.01609)

4. **AI 需求分析助手与显式推理**
   - USF/EMMSAD 2024：探索将显式推理应用于 AI 需求分析
   - 来源：[USF - AI-Based Requirements Analysis Assistant](https://repository.usfca.edu/cgi/viewcontent.cgi?article=1111&context=at)

5. **GenAI 在需求工程中的系统综述**
   - Wiley 2024/2025：系统性文献综述，涵盖 GenAI 在需求工程各阶段的应用
   - 来源：[Wiley - Generative AI for Requirements Engineering](https://onlinelibrary.wiley.com/doi/10.1002/spe.70029)

6. **从文本生成图表的实践**
   - Dev.to：展示从简单 Prompt 生成 PRD、ERD、Swimlane 和 Flow 图的完整工作流
   - 来源：[Dev.to - AI for Requirements: PRD, ERD, Swimlanes, and Flows](https://dev.to/aaronksaunders/ai-for-requirements-prd-erd-swimlanes-and-flows-from-a-simple-prompt-13al)

7. **MermaidFlow：利用 LLM 生成结构化工作流**
   - OpenReview 论文：在 Prompt 中提供每种节点类型的详细描述，引导 LLM 生成结构有效的 Mermaid 工作流
   - 来源：[OpenReview - MERMAIDFLOW (PDF)](https://openreview.net/pdf/c2c41b2611966fe4233e3c0e262ab8d27595ee40.pdf)

**主流技术路径**

```
PRD 文本 → LLM 处理 → Mermaid/PlantUML 代码 → 渲染为可视化流程图
```

常用工具链：
- **Mermaid.js**：最流行的文本转图表格式，GitHub/Obsidian 原生支持
- **PlantUML**：支持 UML 图表的标准文本格式
- **Graphviz/DOT**：通用的图描述语言

---

### 3.2 Prompt Engineering 技巧：引导 LLM 穷举流程步骤

**核心技术**

以下 Prompt Engineering 技术对引导 LLM 生成穷举的流程步骤最为有效：

#### 3.2.1 Chain of Thought (CoT) - 链式思维

- **原理**：强制 LLM 逐步枚举推理过程，而非直接给出结论
- **适用场景**：逐步分解流程，确保每步都有明确的逻辑连接
- **实现方式**：在 Prompt 中加入 "Let's think step by step" 或提供多步推理示例
- **来源**：[Prompt Engineering Guide - Prompting Techniques](https://www.promptingguide.ai/techniques)

#### 3.2.2 Tree of Thought (ToT) - 树状思维

- **原理**：在每一步展开多个推理分支，评估后选择最优路径
- **适用场景**：穷举所有可能的流程分支，包括正常路径和异常路径
- **核心机制**：
  - 将每个"思维"（thought）作为推理的中间步骤
  - 支持 BFS（广度优先）和 DFS（深度优先）搜索策略
  - 可回溯，放弃不可行的路径
- **来源**：
  - [Prompt Engineering Guide - Tree of Thoughts](https://www.promptingguide.ai/techniques/tot)
  - [IBM - What is Tree of Thoughts Prompting](https://www.ibm.com/think/topics/tree-of-thoughts)
  - [Learn Prompting - Tree of Thoughts](https://learnprompting.org/docs/advanced/decomposition/tree_of_thoughts)
  - [Humanloop - ToT Prompting](https://humanloop.com/blog/tree-of-thoughts-prompting)
  - 原始论文：Yao et al. (2023), "Tree of Thoughts: Deliberate Problem Solving with Large Language Models", Princeton University & Google DeepMind

#### 3.2.3 Self-Ask Decomposition - 自问分解

- **原理**：将复杂问题分解为子问题，逐一回答后汇总
- **适用场景**：将复杂流程拆解为子流程，再逐步细化

#### 3.2.4 Few-Shot Prompting - 少样本提示

- **原理**：提供多个示例展示期望的输出格式和粒度
- **适用场景**：展示期望的流程树结构，让 LLM 模仿输出格式
- **关键点**：示例应覆盖正常路径、异常分支、UI 交互等

#### 3.2.5 Step-Back Prompting - 退一步提示

- **原理**：先让 LLM 回答底层原理或宏观框架，再深入具体步骤
- **适用场景**：先梳理整体流程架构，再逐层细化

#### 3.2.6 Self-Consistency - 自一致性

- **原理**：生成多个推理路径，选择最一致的答案
- **适用场景**：验证流程穷举的完整性，多轮生成取并集

**针对流程树生成的 Prompt 策略建议**

```text
策略1：层次化分解 Prompt
  - 第一轮：生成 L1 主流程（仅关键步骤）
  - 第二轮：对每个 L1 步骤生成 L2 子流程
  - 第三轮：对每个 L2 步骤生成 L3 详细步骤

策略2：角色扮演 + 检查清单
  - 让 LLM 扮演 QA 工程师，用检查清单验证每个步骤的完整性
  - 明确要求列出"所有可能的交互：点击、弹窗、跳转、确认、取消"

策略3：ToT + 自验证循环
  - 用 ToT 展开，生成后让 LLM 自我检查是否遗漏步骤
  - 迭代直到满足完整性条件
```

**来源**
- [Prompt Engineering Guide](https://www.promptingguide.ai/techniques)
- [IBM - Prompt Engineering Techniques](https://www.ibm.com/think/topics/prompt-engineering-techniques)
- [K2view - Top Prompt Engineering Techniques for 2026](https://www.k2view.com/blog/prompt-engineering-techniques/)
- [Patronus AI - Advanced Prompt Engineering Techniques](https://www.patronus.ai/llm-testing/advanced-prompt-engineering-techniques)
- [KDnuggets - 3 Easy Ways to Create Flowcharts Using LLMs](https://www.kdnuggets.com/3-easy-ways-create-flowcharts-diagrams-using-llms)

---

## 4. 穷举完整性技术

### 4.1 确保 流程树不遗漏步骤/交互的方法

**核心原则**

穷举完整性是流程树质量的关键指标。以下方法可系统性确保不遗漏：

#### 方法一：需求可追溯性矩阵（RTM）

将流程树的每个节点映射回 PRD 中的原始需求：

| PRD 需求 ID | PRD 需求描述 | 对应流程树节点 | 节点层级 | 覆盖状态 |
|-------------|-------------|---------------|----------|----------|
| REQ-001 | 用户注册 | L1-注册流程 | L1 | 已覆盖 |
| REQ-001.1 | 邮箱验证 | L2-邮箱验证 | L2 | 已覆盖 |
| REQ-001.1a | 验证码过期 | L3-验证码过期处理 | L3 | 已覆盖 |

**验证方式**：所有 PRD 需求都至少有一个对应的流程树节点；所有流程树节点都能追溯到 PRD 需求。

#### 方法二：正交维度交叉检查

从多个独立维度交叉验证完整性：

1. **用户操作维度**：每个可能的用户操作（点击、输入、滑动等）是否都已覆盖
2. **系统响应维度**：每个可能的系统响应（成功、失败、超时等）是否都已覆盖
3. **状态维度**：每个可能的 UI 状态（加载中、空状态、错误状态等）是否都已覆盖
4. **数据维度**：每个关键数据对象的 CRUD 操作是否都已覆盖

#### 方法三：边界值与异常路径穷举

针对每个正常流程步骤，系统性检查：

- 正常输入 → 正常输出
- 空输入 → 验证提示
- 无效输入 → 错误提示
- 超长/超大输入 → 截断/限制
- 超时 → 重试/取消选项
- 网络断开 → 离线处理
- 权限不足 → 拒绝访问
- 并发冲突 → 冲突解决

#### 方法四：角色视角穷举

从每个参与角色的视角遍历流程：

- 管理员看到什么？能做什么？
- 普通用户看到什么？能做什么？
- 游客（未登录）看到什么？能做什么？
- 第三方系统如何交互？

### 4.2 检查清单方法（Checklist-Based Approach）

**交互类型穷举清单**

以下清单可用于逐项检查每个流程步骤是否已覆盖所有交互类型：

```markdown
## 交互类型检查清单

### 用户主动操作
- [ ] 按钮/链接点击（Button/Link Click）
- [ ] 表单输入（Form Input）
- [ ] 下拉选择（Dropdown Selection）
- [ ] 复选/单选（Checkbox/Radio）
- [ ] 文件上传（File Upload）
- [ ] 拖拽操作（Drag & Drop）
- [ ] 滚动加载（Scroll/Infinite Load）
- [ ] 搜索/过滤（Search/Filter）
- [ ] 手势操作（Gesture - 移动端）

### 系统弹窗/提示
- [ ] 确认弹窗（Confirmation Dialog）
- [ ] 警告提示（Alert/Warning）
- [ ] 错误提示（Error Message）
- [ ] 成功提示（Success Toast）
- [ ] 加载状态（Loading State）
- [ ] 空状态（Empty State）
- [ ] 权限提示（Permission Denied）

### 页面跳转
- [ ] 正常跳转（Navigate）
- [ ] 重定向（Redirect）
- [ ] 返回（Back/Return）
- [ ] 外部链接（External Link）
- [ ] 深度链接（Deep Link）

### 数据交互
- [ ] 数据提交（Submit）
- [ ] 数据读取（Read/Load）
- [ ] 数据更新（Update）
- [ ] 数据删除（Delete）
- [ ] 批量操作（Batch Operation）
- [ ] 导入/导出（Import/Export）

### 时间相关
- [ ] 超时处理（Timeout）
- [ ] 定时任务（Scheduled Task）
- [ ] 倒计时（Countdown）
- [ ] 过期处理（Expiration）
```

**完整性验证检查清单**

```markdown
## 完整性验证检查清单

### 需求覆盖
- [ ] PRD 中每条功能需求都有对应的流程步骤
- [ ] PRD 中的非功能需求（性能、安全等）已体现在约束中
- [ ] PRD 中的边界条件已体现为异常分支

### 流程完整性
- [ ] 每个流程有明确的开始和结束
- [ ] 每个决策点都有至少两个分支（Yes/No）
- [ ] 每个异常都有明确的处理路径
- [ ] 不存在"死胡同"（没有出口的分支）

### 交互完整性
- [ ] 每个页面的主要交互都已列出
- [ ] 每个弹窗的触发条件和操作选项都已列出
- [ ] 每个表单的验证逻辑都已描述
- [ ] 加载状态和错误状态都已覆盖

### 一致性
- [ ] 术语使用一致
- [ ] 节点间的数据传递逻辑正确
- [ ] 并行步骤的同步点明确
```

**来源**
- [Requirements Verification Checklist (PDF)](https://greg4cr.github.io/courses/fall16csce740/Documents/RequirementsChecklist.pdf)
- [GeeksforGeeks - Requirements Engineering Process](https://www.geeksforgeeks.org/software-engineering/software-engineering-requirements-engineering-process/)
- [Verification of SRS: Criteria and Techniques](https://kindsonthegenius.com/blog/verification-of-software-requirement-specification-criteria-and-techniques/)
- [Cal Poly - Requirements Checklist](http://users.csc.calpoly.edu/~jdalbey/308/Deliver/QAChkList.html)
- [Visure Solutions - Requirements V&V](https://visuresolutions.com/alm-guide/requirements-verification-and-validation/)
- [Medium - Requirements Validation](https://medium.com/omarelgabrys-blog/requirements-engineering-requirements-validation-part-6-29778d7bde24)
- [ResearchGate - Using Checklists for Requirements Verification](https://www.researchgate.net/figure/Using-Checklists-to-Support-Requirements-Verification_fig4_287241511)

---

## 5. 交互细节捕获方法

### 5.1 用户旅程地图（User Journey Map）

**概述**

用户旅程地图是一种可视化工具，描述用户与产品/服务交互的完整体验过程，包括行为、想法和情绪。

**核心要素**

| 要素 | 描述 |
|------|------|
| **Persona（用户画像）** | 旅程的主角 |
| **Scenario（场景）** | 旅程的背景和目标 |
| **Phases（阶段）** | 旅程的主要阶段 |
| **Actions（行为）** | 用户在每个阶段的具体行为 |
| **Touchpoints（触点）** | 用户与产品的交互点 |
| **Emotions（情绪）** | 用户在各阶段的心情曲线 |
| **Pain Points（痛点）** | 用户遇到的困难 |
| **Opportunities（机会）** | 改善体验的方向 |

**与 User Flow 的区别**

| 维度 | User Journey Map | User Flow |
|------|-----------------|-----------|
| **范围** | 宏观——完整体验生命周期 | 微观——逐屏的任务流程 |
| **焦点** | 情绪、痛点、触点 | 顺序步骤、UI 交互 |
| **使用阶段** | 研究 & 策略阶段 | 设计 & 原型阶段 |
| **输出** | 包含画像、时间线、情绪的视觉地图 | 屏幕 & 决策的流程图 |

**对流程树生成的启示**

- User Journey Map 的"阶段"对应 L1 主流程
- 每个 Touchpoint 可展开为 L2/L3 详细步骤
- Pain Points 提示需要特别关注的异常分支
- "宏观+微观"的双层视角确保既看到全局又不遗漏细节

**来源**
- [Nielsen Norman Group - User Journeys vs. User Flows](https://www.nngroup.com/articles/user-journeys-vs-user-flows/)
- [Figma - User Journey Mapping](https://www.figma.com/resource-library/user-journey-map/)
- [IxDF - Customer Journey Map](https://ixdf.org/literature/topics/customer-journey-map)
- [UXPressia - User Flow vs. User Journey](https://uxpressia.com/blog/user-flow-vs-user-journey)
- [Coursera - Creating User Journey Maps](https://www.coursera.com/articles/creating-user-journey-maps-a-guide)

---

### 5.2 UI Flow / Screen Flow 方法

**概述**

UI Flow（也称 Screen Flow）是一种详细描述用户界面中屏幕之间导航关系的可视化方法。它聚焦于具体的屏幕流转和交互细节，是 User Journey Map 的微观层面实现。

**核心构成**

1. **Screen（屏幕/页面）**：每个独立的 UI 界面
2. **Action（动作）**：用户在屏幕上执行的操作
3. **Transition（转换）**：从当前屏幕到下一屏幕的路径
4. **State（状态）**：屏幕在不同条件下的表现（加载中、错误、空等）

**UI Flow 的层次化方法**

```
Level 1: 页面地图（Sitemap）
  - 所有页面/屏幕的层级关系

Level 2: 页面间流转（Page Flow）
  - 页面之间的跳转关系和触发条件

Level 3: 页面内交互（In-page Interaction）
  - 每个页面内的所有交互元素和行为
  - 包括：按钮、表单、弹窗、下拉等

Level 4: 状态变体（State Variants）
  - 每个交互的所有可能状态
  - 包括：正常、错误、加载中、空、禁用等

Level 5: 响应细节（Response Details）
  - 每个状态变化的具体响应
  - 包括：动画、提示文案、数据变化等
```

**UI Flow 穷举模式**

为确保不遗漏任何 UI 交互，推荐以下穷举模式：

```
For each Screen:
  For each Element on Screen:
    For each Action on Element:
      For each State of Element:
        Describe: Trigger → Response → Next State
```

**对流程树生成的启示**

- UI Flow 的 5 层分解方法与 iterative-flow 的最大深度 5 级天然匹配
- "页面→流转→交互→状态→响应"的分解可直接映射为 L1→L5
- 穷举模式可作为 Prompt 中的模板，指导 LLM 逐层枚举
- 状态变体的列举方式可用于异常分支的生成

**来源**
- [Nielsen Norman Group - User Journeys vs. User Flows](https://www.nngroup.com/articles/user-journeys-vs-user-flows/)
- [Medium - User Flow Diagrams vs. User Journey Maps](https://medium.com/@bunrithy/differentiating-user-flow-diagrams-vs-user-journey-maps-in-ux-design-8b991b3ab6e3)
- [UX Planet - User Journey Map and User Flow Difference](https://uxplanet.org/user-journey-map-and-user-flow-what-is-the-difference-between-them-and-when-to-use-each-0e9998c039d0)
- [Slickplan - User Journey vs User Flow](https://slickplan.com/blog/user-journey-vs-user-flow)
- [MockFlow - User Journey Vs. User Flow](https://mockflow.com/blog/User-Journey-Vs-User-Flow)

---

## 6. 对 iterative-flow 的综合启示

### 6.1 方法论融合建议

综合以上调研，iterative-flow 的流程树生成可融合以下方法论：

| 方法论 | 融合方式 | 对应 iterative-flow 特性 |
|--------|----------|--------------------------|
| User Story Mapping | 骨干→步骤→细节的三层分解 | L1→L2→L3 层次结构 |
| Event Storming | 事件驱动的流程节点识别 | 关键步骤和触发条件 |
| IEEE 29148 | 需求可追溯性和唯一标识 | 节点编号和 PRD 映射 |
| BPMN | Gateway/Branch/SubProcess 建模 | 分支逻辑和子树拆分 |
| UML State Machine | 状态转换和 Guard Condition | 异常分支和条件判断 |
| Tree of Thought | 多分支推理和评估 | 穷举所有可能的路径 |
| Checklist Method | 交互类型逐项检查 | 确保不遗漏交互行为 |
| UI Flow | 页面→交互→状态的多层分解 | L3→L5 的详细交互描述 |

### 6.2 Prompt 策略组合建议

```
推荐策略：层次化 CoT + ToT 穷举 + 检查清单验证

Phase 1: 结构提取
  - 使用 Step-Back Prompting 先提取 PRD 的宏观结构
  - 用 CoT 逐步分解为 L1 主流程

Phase 2: 逐层细化
  - 对每个 L1 节点，用 Few-Shot 提供期望的 L2 结构示例
  - 迭代细化至 L3~L5

Phase 3: 穷举分支
  - 用 ToT 方法对每个决策点展开所有可能的分支
  - 明确要求包含正常路径、异常路径、边界条件

Phase 4: 完整性验证
  - 使用检查清单逐项验证
  - 用 RTM 方法确保 PRD 需求全覆盖
  - Self-Consistency 多轮验证
```

### 6.3 深度控制策略

基于调研中的建模最佳实践（BPMN 的 5~9 活动/层规则，UI Flow 的 5 层分解）：

```
L1: 主流程 —— 3~7 个关键步骤（对应 BPMN Process Level）
L2: 子流程 —— 每个步骤 3~9 个子步骤（对应 BPMN Sub-Process）
L3: 详细步骤 —— 具体操作和交互（对应 BPMN Task Level / UML Action）
L4: 交互细节 —— UI 元素和状态（对应 UI Flow In-page Interaction）
L5: 响应细节 —— 具体触发条件和响应（对应 UI Flow State Variants）

超过 L5: 自动拆分为独立子树（对应 BPMN Call Activity）
```

---

## 参考文献汇总

### 结构化需求方法
- [Jeff Patton Associates - User Story Mapping](https://jpattonassociates.com/story-mapping/)
- [Jeff Patton - The New Backlog is a Map](https://jpattonassociates.com/the-new-backlog/)
- [NN/g - Mapping User Stories in Agile](https://www.nngroup.com/articles/user-story-mapping/)
- [StoriesOnBoard - User Story Mapping Basics](https://storiesonboard.com/user-story-mapping-basics.html)
- [DDD Europe 2024 - EventStorming Masterclass](https://2024.dddeurope.com/program/eventstorming-masterclass/)
- [Lucidspark - 8 Steps in the Event Storming Process](https://lucid.co/blog/8-steps-in-the-event-storming-process)
- [Qlerify - Why Event Storming](https://www.qlerify.com/post/why-event-storming)
- [IEEE SA - IEEE/ISO/IEC 29148-2018](https://standards.ieee.org/standard/29148-2018.html)
- [ReqView - ISO/IEC/IEEE 29148 Templates](https://www.reqview.com/doc/iso-iec-ieee-29148-templates/)
- [Jama Software - How to Write SRS](https://www.jamasoftware.com/requirements-management-guide/writing-requirements/system-requirements-specification/)

### 流程建模标准
- [Trisotech - BPMN Modeling Best Practices](https://www.trisotech.com/bpmn-modeling-best-practices/)
- [Camunda - BPMN 2.0 Reference](https://camunda.com/bpmn/reference/)
- [OMG - BPMN Official](https://www.omg.org/bpmn/)
- [GBTEC - BPMN 2.0 Explained](https://www.gbtec.com/wiki/process-management/bpmn/)
- [Signavio - BPMN Modeling Conventions](https://www.signavio.com/post/bpmn-modeling-conventions/)
- [Visual Paradigm - State Machine vs Activity Diagram](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/state-machine-diagram-vs-activity-diagram/)
- [Lucidchart - UML Activity Diagram Tutorial](https://www.lucidchart.com/pages/tutorial/uml-activity-diagram)

### AI 辅助需求分析
- [HAL Science - Automating DFD Generation from User Stories](https://hal.science/hal-04525925v1/file/Generating_Data_Flow_Diagram_with_LLM-4.pdf)
- [ResearchGate - Multi-agent LLM for Requirements Analysis](https://www.researchgate.net/publication/395498762_A_Multi-agent_LLM_System_for_Automated_Requirements_Analysis_A_Study_on_User_Story_Generation_and_Prioritization)
- [arXiv - Structured Decomposition for LLM Reasoning](https://arxiv.org/pdf/2601.01609)
- [Wiley - Generative AI for Requirements Engineering](https://onlinelibrary.wiley.com/doi/10.1002/spe.70029)
- [Dev.to - AI for Requirements: PRD, ERD, Swimlanes, Flows](https://dev.to/aaronksaunders/ai-for-requirements-prd-erd-swimlanes-and-flows-from-a-simple-prompt-13al)
- [OpenReview - MERMAIDFLOW](https://openreview.net/pdf/c2c41b2611966fe4233e3c0e262ab8d27595ee40.pdf)
- [KDnuggets - Create Flowcharts Using LLMs](https://www.kdnuggets.com/3-easy-ways-create-flowcharts-diagrams-using-llms)

### Prompt Engineering
- [Prompt Engineering Guide - Techniques](https://www.promptingguide.ai/techniques)
- [Prompt Engineering Guide - Tree of Thoughts](https://www.promptingguide.ai/techniques/tot)
- [IBM - Tree of Thoughts Prompting](https://www.ibm.com/think/topics/tree-of-thoughts)
- [IBM - Prompt Engineering Techniques](https://www.ibm.com/think/topics/prompt-engineering-techniques)
- [Learn Prompting - Tree of Thoughts](https://learnprompting.org/docs/advanced/decomposition/tree_of_thoughts)
- [K2view - Top Prompt Engineering Techniques](https://www.k2view.com/blog/prompt-engineering-techniques/)
- [Patronus AI - Advanced Prompt Engineering](https://www.patronus.ai/llm-testing/advanced-prompt-engineering-techniques)

### 完整性验证
- [Requirements Verification Checklist (PDF)](https://greg4cr.github.io/courses/fall16csce740/Documents/RequirementsChecklist.pdf)
- [GeeksforGeeks - Requirements Engineering Process](https://www.geeksforgeeks.org/software-engineering/software-engineering-requirements-engineering-process/)
- [Visure Solutions - Requirements V&V](https://visuresolutions.com/alm-guide/requirements-verification-and-validation/)
- [Cal Poly - Requirements Checklist](http://users.csc.calpoly.edu/~jdalbey/308/Deliver/QAChkList.html)

### 交互细节捕获
- [NN/g - User Journeys vs. User Flows](https://www.nngroup.com/articles/user-journeys-vs-user-flows/)
- [Figma - User Journey Mapping](https://www.figma.com/resource-library/user-journey-map/)
- [IxDF - Customer Journey Map](https://ixdf.org/literature/topics/customer-journey-map)
- [UXPressia - User Flow vs. User Journey](https://uxpressia.com/blog/user-flow-vs-user-journey)
- [Slickplan - User Journey vs User Flow](https://slickplan.com/blog/user-journey-vs-user-flow)
