# 调研报告：结构化需求流程（Flow+Data）驱动AI实现

> 本文由AI辅助生成
>
> **核心命题**：软件需求本质上是流程（Flow）+ 数据（Data），只需要穷举补充好流程步骤，AI应该可以准确实现代码。
>
> **版本**：v2.0 | **日期**：2026-05-06

---

## 目录

1. [命题分析](#1-命题分析)
2. [现有研究概览](#2-现有研究概览)
3. [关键发现](#3-关键发现)
4. [差距分析](#4-差距分析)
5. [可行性评估](#5-可行性评估)
6. [建议](#6-建议)

---

## 1. 命题分析

### 1.1 命题拆解

本命题包含三个核心论断：

| 论断 | 内容 | 隐含假设 |
|------|------|----------|
| **论断A** | 软件需求 = Flow + Data | 需求可以完整映射为流程步骤和数据结构 |
| **论断B** | 穷举流程步骤是可行的 | 复杂系统可以被有界地分解为离散步骤 |
| **论断C** | AI可据此准确实现代码 | LLM具备从结构化规格到正确代码的转换能力 |

### 1.2 理论基础：结构化分析方法的现代化回归

该命题的理论根源可追溯至1970年代的结构化分析方法（Structured Analysis, SA）。**"需求 = 流程 + 数据"并非全新的假设，而是经典软件工程理论在LLM时代的回归与应用。**

**a) 数据流图（Data Flow Diagram, DFD）+ 数据字典（Data Dictionary, DD）**

1970年代，Larry Constantine提出了数据流图的概念，随后Tom DeMarco在其1978年的经典著作《Structured Analysis and System Specification》中将其系统化，Ed Yourdon进一步推广了SA/SD（Structured Analysis / Structured Design）方法[[SA历史综述]](https://www.geeksforgeeks.org/system-design/structured-analysis-and-structured-design-sa-sd/)。SA方法的核心工具包括：

- **DFD**：以图形化方式描述系统中数据的流动、处理过程和数据存储，刻画系统的行为需求（即Flow）。
- **DD**：集中定义所有数据元素的结构、类型和约束，刻画系统的数据需求（即Data）。
- **结构化英语/伪代码**：对处理逻辑进行精确描述。

SA方法的核心理念正是：**需求 = 行为（流程）+ 数据**。DFD描述"做什么"（流程），数据字典描述"操作什么"（数据）。本命题的"Flow+Data"分解与SA方法的"DFD+DD"完全对应。

**b) 有限状态机（Finite State Machine, FSM）理论**

FSM将系统行为建模为状态（State）、事件（Event）和转换（Transition）的组合，是Flow的形式化表达基础。IREB提出的分层FSM模型（Layered FSM Model）明确指出："The finite state machine (FSM) model is very popular with requirement engineers and developers"，并提出了需求层（Requirements Layer）作为独立建模层级的概念[[IREB, Avnur]](https://re-magazine.ireb.org/print/a-finite-state-machine-model)。FSM理论为本命题中"穷举流程步骤"的可行性提供了形式化基础——通过Event-State Analysis（ESA），可以系统化地检查所有事件与所有状态的笛卡尔积，确保流程定义的完备性。

**c) 模型驱动架构（Model-Driven Architecture, MDA）**

OMG提出的MDA框架主张从平台无关模型（PIM）到平台特定模型（PSM）的自动转换，其核心理念即"规格驱动实现"。本命题可视为MDA思想在LLM时代的具体化——将"Flow+Data规格"作为PIM，LLM的代码输出作为PSM。

**d) Domain-Driven Design（DDD）**

DDD强调通过对业务领域（数据模型+业务流程）的精确建模来驱动软件设计。实体（Entity）、值对象（Value Object）、聚合根（Aggregate Root）对应Data维度，领域服务（Domain Service）、应用服务（Application Service）对应Flow维度。

**e) GenAI for RE综述的印证**

2024年的系统性综述《Generative AI for Requirements Engineering》分析了238篇相关论文，覆盖需求获取（Elicitation）、分析（Analysis）、规约（Specification）、验证（Validation）和管理（Management）各阶段[[arXiv:2409.06741]](https://arxiv.org/abs/2409.06741)。该综述表明，LLM在需求规约阶段表现突出——能够将自然语言需求转化为更结构化的格式，这从实证角度支持了"结构化需求规格可以更有效地指导AI代码生成"的假设。

### 1.3 命题的合理性与边界

**合理性**：命题在以下场景中具有高度合理性——

- 业务流程管理系统（BPM），天然以流程和数据为中心
- 状态驱动的嵌入式系统，FSM可完全描述行为
- CRUD类应用，数据模型 + 操作流程即可覆盖需求
- API接口实现，输入/输出数据模型明确，处理流程相对标准化
- 表单处理与数据验证，Flow（验证规则）+ Data（字段定义）清晰可穷举

**边界条件**：以下场景需要额外关注——

- 涉及复杂算法或数学优化的系统（Flow难以描述算法选择策略）
- 非功能需求（性能、安全性、可用性）的建模超出了Flow+Data的描述范围
- 跨系统集成中的协议适配和异常处理
- 高并发分布式系统（并发控制、一致性保障的流程极难完整穷举）

---

## 2. 现有研究概览

### 2.1 学术论文

#### 2.1.1 Flow2Code：流程图到代码生成基准（ACL 2025 Findings）

- **论文**：*Flow2Code: Evaluating Large Language Models for Flowchart-based Code Generation Capability*
- **来源**：ACL 2025 Findings
- **链接**：[ACL Anthology](https://aclanthology.org/2025.findings-acl.425/) | [arXiv:2506.02073](https://arxiv.org/abs/2506.02073)
- **作者**：Mengliang He, Jiayi Zeng, Yankai Jiang, Wei Zhang 等

Flow2Code是首个系统性评估多模态LLM从流程图生成代码能力的研究。

| 维度 | 数据 |
|------|------|
| 代码片段数量 | 5,622个 |
| 流程图数量 | 16,866个 |
| 编程语言 | 15种 |
| 流程图类型 | 代码流程图（Code Flowchart）、UML活动图（UML Activity Diagram）、伪代码流程图（Pseudocode Flowchart） |
| 测试LLM数量 | 13个多模态LLM |

**核心结论**：

1. LLM具备从结构化流程图规格生成代码的基本能力，这是"流程驱动代码生成"可行性的直接实证。
2. **当前模型均未达到完美水平**——没有任何模型能完美处理所有流程图到代码的生成任务。
3. 流程图类型对生成效果有影响：代码流程图的效果优于UML和伪代码流程图。
4. 编程语言的差异也会影响生成质量。
5. 通过SFT（Supervised Fine-Tuning），模型性能可获得显著提升。

**对本命题的启示**：Flow2Code直接验证了"结构化流程规格 → LLM代码生成"这条路径的可行性，但也揭示了当前模型在处理复杂流程时的局限性。穷举流程步骤的质量和表达形式直接影响生成代码的准确性。

#### 2.1.2 Generative AI for Requirements Engineering（arXiv 2024）

- **论文**：*Generative AI for Requirements Engineering: A Systematic Literature Review*
- **来源**：arXiv 2024
- **链接**：[arXiv:2409.06741](https://arxiv.org/abs/2409.06741) | [HTML版](https://arxiv.org/html/2409.06741v1)

该综述系统回顾了**238篇论文**（2019年至今），全面考察了GenAI在需求工程各阶段的应用。

**与本命题相关的发现**：

1. LLM在需求规约（Specification）阶段表现突出——能够将自然语言需求转化为更结构化的格式。
2. LLM在需求验证阶段可以辅助生成测试用例，这与"流程步骤穷举后自动生成验收测试"的设想高度契合。
3. 当前研究多关注需求文本的处理，较少涉及从结构化需求规格直接驱动代码生成的端到端流程。
4. 需求工程面临的核心挑战——需求的模糊性、不完备性和不一致性——正是结构化Flow+Data规格试图解决的问题。

#### 2.1.3 FSM-Driven Streaming Inference（arXiv 2026）

- **论文**：*Boosting AI Reliability with an FSM-Driven Streaming Inference Pipeline: An Industrial Case*
- **来源**：arXiv
- **链接**：[arXiv:2603.01528](https://arxiv.org/abs/2603.01528) | [HTML版](https://arxiv.org/html/2603.01528v1)

该论文由清华大学团队完成，将有限状态机（FSM）与AI推理模型结合，用FSM编码领域知识来引导和纠正AI模型在流式推理中的预测结果。应用场景为从监控视频自动计数挖掘机工作量。

**方法架构**：

```
Object Detection (YOLOv9) -> Event Identification -> FSM State Machine -> Business Logic
```

**核心思想**：用FSM封装业务流程知识，引导和纠正AI模型的预测。FSM为每个状态定义有效事件空间，过滤掉无效或噪声事件。

**实验结果**：

| 指标 | 启发式规则 | FSM方案 |
|------|-----------|----------|
| Precision | 0.91 | **0.96** |
| Recall | 0.94 | 0.93 |
| F1-Score | 0.92 | **0.94** |
| Fake Workloads | 34(loose)/14(strict) | **13** |
| Missing Workloads | 18(loose)/47(strict) | **23** |

**关键引述**："The state machine prevents overfitting by decomposing the complex working process into a series of separated business states. The inference for each state is much simpler and more stable than the global inference based on the manual heuristic rules"[[Zhang et al.]](https://arxiv.org/html/2603.01528v1)

**对本命题的启示**：本命题的"Flow+Data"模式与此论文的"FSM+AI"思路异曲同工——通过结构化的流程规格约束和引导AI的行为，可以显著提升系统可靠性。这为"穷举流程步骤后AI可以更准确地工作"提供了工业级的实证支持。

#### 2.1.4 IREB 分层 FSM 模型

- **来源**：IREB Magazine
- **作者**：Arie Avnur
- **链接**：[IREB RE-Magazine](https://re-magazine.ireb.org/print/a-finite-state-machine-model)

IREB杂志发表的分层有限状态机模型（Layered FSM Model），为"需求即Flow"提供了系统的理论框架。

**五层FSM架构**：

| 层级 | 名称 | 内容 |
|------|------|------|
| Layer 1 | General Layer | 状态转换图/表、控制结构、状态定义 |
| Layer 2 | Requirements Layer | 需求声明、控制变量值、状态不变量 |
| Layer 3 | Design Layer | 入口/执行/退出动作设计 |
| Layer 4 | Implementation Layer | FSM代码 |
| Layer 5 | Test Layer | 测试场景（从FSM自动生成） |

**核心创新——控制结构（Control Structure）**：定义了FSM所控制的系统元素，要求所有状态必须显式定义控制结构值。"Key FSM adds this element—the control structure. It specifies system elements controlled or affected by the FSM, reducing error risk by requiring ALL states to define control structure values"[[IREB, Avnur]](https://re-magazine.ireb.org/print/a-finite-state-machine-model)。

**Event-State Analysis（ESA）**：一种系统化的FSM完整性验证方法，通过检查所有Events x States的笛卡尔积来发现遗漏的转换。"Performed rigorously, it achieves model completeness and thus contributes to its correctness"[[IREB, Avnur]](https://re-magazine.ireb.org/print/a-finite-state-machine-model)。

### 2.2 开源项目

#### 2.2.1 StateSmith：状态机代码生成器

- **GitHub**：[StateSmith/StateSmith](https://github.com/StateSmith/StateSmith)
- **定位**：从状态图/流程图直接生成可读代码的开源工具

StateSmith验证了"流程规格 → 自动代码生成"的技术可行性。

| 特性 | 描述 |
|------|------|
| 零运行时依赖 | 生成的代码完全独立，无需额外库 |
| 人类可读 | 生成的代码结构清晰，可人工审查 |
| 多语言支持 | C、C++、C#、JavaScript、TypeScript、Python等 |
| 多输入格式 | PlantUML、Draw.io、yEd图形文件 |
| UML Statechart支持 | 层级嵌套状态、进入/退出动作、带条件和动作的转换 |
| 测试覆盖 | 730+测试用例 |
| 生产验证 | 消费电子、自动驾驶等领域实际使用 |

**关键意义**：StateSmith不是AI方案，而是一个确定性的代码生成器。它的成功说明：**只要流程规格足够精确和完整，自动代码生成在工程上是完全可行的**。LLM可以在确定性的FSM骨架基础上，处理需要灵活实现的部分（如Entry/Do/Exit动作的具体逻辑）[[StateSmith]](https://github.com/StateSmith/StateSmith)。

#### 2.2.2 Quantum Leaps QP Framework

- **官网**：[state-machine.com](https://www.state-machine.com/qpcpp/srs-qp.html)
- **GitHub**：[QuantumLeaps/qpc](https://github.com/QuantumLeaps/qpc)
- **定位**：基于状态机的工业级实时嵌入式软件框架
- **Wikipedia**：[QP (Framework)](https://en.wikipedia.org/wiki/QP_(framework))

QP框架提供了从需求规格（以状态机形式）到实现的完整路径：

- 基于Active Object模式和层级状态机（UML Statecharts）构建。
- 提供QM可视化建模工具，支持从图形化规格直接生成框架代码。
- 在ARM Cortex-M等嵌入式平台上有广泛的工业应用。
- 获得安全认证：IEC-61508 SIL-3（功能安全）、IEC-62304 Class-C（医疗设备软件）。
- 开源（双重许可），在嵌入式社区有活跃的用户基础。

**核心抽象**：

| 概念 | 说明 |
|------|------|
| Active Objects | 并发执行的自治实体 |
| Events | 状态转换的触发器 |
| State Machines | 行为的形式化描述 |
| Event Delivery | 对象间通信机制 |

**关键意义**：QP框架在安全关键级嵌入式领域的成功，验证了"需求 = 状态转换流程"模式在实际项目中的可行性。其SRS文档本身就是Flow+Data结构化描述的范例。证明了以FSM为核心的需求规格可以直接驱动安全关键级软件实现[[Quantum Leaps]](https://www.state-machine.com/qpcpp/srs-qp.html)。

### 2.3 工业实践

#### 2.3.1 Requirements-as-Code（Emmanuel Goossaert）

- **作者**：Emmanuel Goossaert
- **链接**：[LinkedIn Article](https://www.linkedin.com/pulse/requirements-as-code-ai-augmented-software-engineers-goossaert-bbpje)
- **个人网站**：[goossaert.com](http://www.goossaert.com/)

Goossaert提出了"Requirements-as-Code"范式，认为软件工程将从编写代码转向编写需求规格文档和验收测试。

**核心观点**：

1. **编码工作重心转移**："Software engineering will shift from writing code to writing PRDs and acceptance tests"
2. **数据成为新瓶颈**："Data will be the new bottleneck"——不是模型能力，而是高质量的结构化需求描述数据
3. **架构设计仍是人类护城河**：即使AI能从规格生成代码，系统架构决策仍需要人类判断
4. **代码可维护性技术债将消失**：AI生成代码使得重构成本趋近于零
5. **QA角色更加关键**：验证AI产出正确性成为核心能力

这一观点实质上是本命题的延伸——将结构化需求（Flow+Data）视为新范式下的"代码"[[Goossaert]](https://www.linkedin.com/pulse/requirements-as-code-ai-augmented-software-engineers-goossaert-bbpje)。

#### 2.3.2 Spec-Driven Development + AI（Augment Code）

- **来源**：[Augment Code Guide](https://www.augmentcode.com/guides/ai-spec-driven-development-workflows)
- **关键数据**：报告了**26%的生产力提升**

Augment Code的SDD（Spec-Driven Development）方法展示了规格驱动开发的工业化实践。

**五阶段工作流**：

```
Spec Authoring -> Planning -> Task Breakdown -> Implementation -> Validation
```

**关键数据**：

| 指标 | 数值 |
|------|------|
| 生产力提升 | 26% |
| 开发者采用率（DORA报告） | 90% |
| 开发者认为有益的比例 | 80%+ |
| SWE-bench得分 | 70.6% |
| 多文件上下文Pass@1 | ~20%（上下文退化时） |

**层级定位**：SDD工作在架构层级（Architecture Level），区别于TDD（单元级）和BDD（功能级）。这恰恰对应了"需求规格"作为高抽象层级输入驱动的理念。

**实践表明**：

1. 先编写规格再让AI生成代码，相比直接让AI从头写代码，产出质量更高、迭代更快。
2. 结构化规格为AI提供了更好的上下文（Context），减少了歧义和幻觉。
3. 26%的生产力提升是可量化的正面信号，但也说明该模式并非银弹。
4. 多文件上下文退化（Pass@1降至~20%）表明上下文管理是关键瓶颈。

[[Augment Code]](https://www.augmentcode.com/guides/ai-spec-driven-development-workflows)

#### 2.3.3 GitHub Blog: Reliable AI Workflows

- **链接**：[GitHub Blog](https://github.blog/ai-and-ml/github-copilot/how-to-build-reliable-ai-workflows-with-agentic-primitives-and-context-engineering/)

GitHub提出了构建可靠AI工作流的关键概念和三层Agent编码框架：

**三层架构**：

```
Layer 1: Markdown Prompt Engineering
    - .instructions.md（全局指令）
    - .chatmode.md（对话模式）
    - .prompt.md（任务模板）
Layer 2: Agent Primitives
    - .spec.md（规格文件）
    - .memory.md（持久记忆）
Layer 3: Context Engineering
    - 上下文组装策略
```

**核心概念**：

1. **Agentic Primitives（智能体原语）**：将AI工作流分解为工具使用、规划、记忆、检索、护栏等基础构建块。
2. **Context Engineering（上下文工程）**：超越Prompt Engineering，关注如何动态组装正确的上下文信息。
3. **确定性脚手架（Deterministic Scaffolding）**：用可预测的代码路径包裹LLM调用，提高可靠性。

**对本命题的启示**：本命题的"Flow+Data"结构化规格恰好提供了高质量的上下文（Context），这与GitHub倡导的Context Engineering理念一致。GitHub将"规格驱动"从学术概念推进到了工程基础设施层面，通过.spec.md文件格式和APM（Agent Package Manager）实现了规格的可复用和可分发[[GitHub Blog]](https://github.blog/ai-and-ml/github-copilot/how-to-build-reliable-ai-workflows-with-agentic-primitives-and-context-engineering/)。

#### 2.3.4 Addy Osmani: LLM Coding Workflow 2026

- **作者**：Addy Osmani（Google Chrome团队工程师）
- **链接**：[Medium Article](https://medium.com/@addyosmani/my-llm-coding-workflow-going-into-2026-52fe1681325e)

Addy Osmani基于个人实践总结了LLM辅助编码的工作流。

**核心实践**：

1. **Specs Before Code**：先写规格，再生成代码。
2. **小步迭代（Small Iterative Chunks）**：将大型任务分解为可验证的小步骤。
3. **丰富上下文（Extensive Context）**：通过CLAUDE.md等机制提供项目上下文。
4. **Human-in-the-Loop**：人类始终在关键决策环节参与审查。
5. **"Waterfall in 15 minutes"**：在极短时间内完成规格-设计-实现-验证的完整循环。

**量化数据**：Osmani声称Claude Code项目中约90%的代码由Claude自身生成。

**关键意义**：这是一个来自一线工程实践者的案例，证明了在"良好的规格输入 + 迭代验证"条件下，AI编码可以承担绝大部分代码实现工作[[Osmani]](https://medium.com/@addyosmani/my-llm-coding-workflow-going-into-2026-52fe1681325e)。

---

## 3. 关键发现

### 3.1 发现一：Flow到Code的映射已被工程验证，但AI尚未完美胜任

**证据**：

- StateSmith（确定性代码生成器）和QP Framework（安全认证级框架）已证明从FSM描述到生产级代码的工程可行性[[StateSmith]](https://github.com/StateSmith/StateSmith)[[Quantum Leaps]](https://www.state-machine.com/qpcpp/srs-qp.html)。
- Flow2Code基准测试显示，13个多模态LLM在流程图到代码生成任务上均未达到完美水平，但SFT可显著提升能力[[Flow2Code]](https://aclanthology.org/2025.findings-acl.425/)。
- 清华大学的工业案例证明FSM能显著提升AI系统的可靠性（F1从0.92提升至0.94）[[Zhang et al.]](https://arxiv.org/html/2603.01528v1)。

**结论**：Flow到Code的映射在理论和方法上是可行的，确定性工具已实现完美映射，但当前LLM的理解能力和推理能力尚存差距。LLM可以在确定性FSM骨架的基础上处理需要灵活实现的部分，两者互补。

### 3.2 发现二：结构化规格是AI编码效率的关键杠杆

**证据**：

- Augment Code的SDD方法报告26%生产力提升，其核心在于规格先行的开发流程[[Augment Code]](https://www.augmentcode.com/guides/ai-spec-driven-development-workflows)。
- Addy Osmani的实践表明，在良好规格输入下，90%的代码可由AI生成[[Osmani]](https://medium.com/@addyosmani/my-llm-coding-workflow-going-into-2026-52fe1681325e)。
- DORA报告显示90%开发者采用AI辅助工具，80%+报告获益[[Augment Code引用DORA]](https://www.augmentcode.com/guides/ai-spec-driven-development-workflows)。

**结论**：规格质量是AI编码产出的决定性因素之一。"垃圾进、垃圾出"的规律在AI编码中同样适用。结构化的Flow+Data规格为LLM提供了高质量的上下文（Context），减少了歧义和幻觉。

### 3.3 发现三：FSM是连接需求与实现的最佳形式化桥梁

**证据**：

- IREB的分层FSM模型提供了从需求到测试的完整五层映射[[IREB, Avnur]](https://re-magazine.ireb.org/print/a-finite-state-machine-model)。
- 清华大学的工业案例证明FSM能显著提升AI系统的可靠性[[Zhang et al.]](https://arxiv.org/html/2603.01528v1)。
- QP Framework的安全认证（SIL-3, Class-C）证明FSM方法满足安全关键系统的严格要求[[Quantum Leaps]](https://www.state-machine.com/qpcpp/srs-qp.html)。
- StateSmith的730+测试用例覆盖和跨领域生产部署证明了FSM方法的工程成熟度[[StateSmith]](https://github.com/StateSmith/StateSmith)。

**结论**：FSM提供了形式化的Flow描述能力，且具有从需求分析到代码生成再到测试用例生成的全生命周期支持。它是"Flow+Data"范式中Flow侧的最佳形式化工具。

### 3.4 发现四：穷举的工程方法已经存在

**证据**：

- IREB提出的Event-State Analysis（ESA）方法通过检查所有Events x States的笛卡尔积来确保完整性。"Performed rigorously, it achieves model completeness and thus contributes to its correctness"[[IREB, Avnur]](https://re-magazine.ireb.org/print/a-finite-state-machine-model)。
- Key FSM的控制结构（Control Structure）概念要求每个状态显式定义对所有控制变量/实体的值，强制完整性约束[[IREB, Avnur]](https://re-magazine.ireb.org/print/a-finite-state-machine-model)。
- 清华大学的工业案例通过将复杂流程分解为5个业务状态和5个事件，实现了FSM的可管理规模[[Zhang et al.]](https://arxiv.org/html/2603.01528v1)。

**结论**：命题中的"穷举"并非不可企及的目标。ESA、控制结构约束、分层分解等工程方法提供了系统化的穷举路径。但穷举的完整性与FSM的规模需要权衡——对于极大规模系统，层次化FSM（Hierarchical Statechart）可以缓解组合爆炸问题。

### 3.5 发现五：上下文退化是AI编码的核心挑战

**证据**：

- Augment Code报告多文件上下文下Pass@1降至约20%，表明上下文管理是关键瓶颈[[Augment Code]](https://www.augmentcode.com/guides/ai-spec-driven-development-workflows)。
- GitHub的Agentic Primitives框架专门设计了Context Engineering层来解决此问题[[GitHub Blog]](https://github.blog/ai-and-ml/github-copilot/how-to-build-reliable-ai-workflows-with-agentic-primitives-and-context-engineering/)。
- Addy Osmani强调"Extensive Context"和CLAUDE.md机制的重要性[[Osmani]](https://medium.com/@addyosmani/my-llm-coding-workflow-going-into-2026-52fe1681325e)。
- Flow2Code研究表明LLM对复杂流程图的理解能力有限[[Flow2Code]](https://aclanthology.org/2025.findings-acl.425/)。

**结论**：即使有了完美的Flow+Data规格，如何将规格有效地传递给AI（即Context Engineering）仍是一个需要解决的工程问题。规格的规模、结构和表示形式都会影响AI的理解效果。

### 3.6 发现六：需求到代码的自动化存在确定性路径

**证据**：

- IREB文章明确描述了从Requirements Layer到Design Layer到Implementation Layer的映射规则，包括状态动作（Entry/Do/Exit）的通用模板[[IREB, Avnur]](https://re-magazine.ireb.org/print/a-finite-state-machine-model)。
- QP Framework的FSM Engine实现了从FSM XML定义到运行时代码的自动生成，"leaves very little application code"[[Quantum Leaps via IREB]](https://re-magazine.ireb.org/print/a-finite-state-machine-model)。
- StateSmith从PlantUML/draw.io图直接生成C/C#/JavaScript等多语言代码[[StateSmith]](https://github.com/StateSmith/StateSmith)。
- 测试用例可从FSM自动生成——"We can generate productions that are strings of events and the sequences of states they create"[[IREB, Avnur]](https://re-magazine.ireb.org/print/a-finite-state-machine-model)。

**结论**：需求到代码的映射已经存在确定性的自动化路径（如FSM Engine、Code Generator），而LLM可以在此基础上处理FSM中需要灵活实现的部分。确定性骨架 + LLM灵活填充的混合模式可能是最优路径。

---

## 4. 差距分析

### 4.1 端到端验证的缺失

现有研究大多关注流程中的单一环节：

- Flow2Code[[Flow2Code]](https://aclanthology.org/2025.findings-acl.425/)关注"流程图 → 代码"的生成能力评测，但未涉及完整的软件开发流程。
- GenAI for RE综述[[arXiv:2409.06741]](https://arxiv.org/abs/2409.06741)关注需求工程本身，未延伸到代码生成。
- StateSmith[[StateSmith]](https://github.com/StateSmith/StateSmith)和QP框架[[Quantum Leaps]](https://www.state-machine.com/qpcpp/srs-qp.html)实现了流程到代码的自动生成，但主要限于状态机场景。
- Augment Code[[Augment Code]](https://www.augmentcode.com/guides/ai-spec-driven-development-workflows)和Osmani[[Osmani]](https://medium.com/@addyosmani/my-llm-coding-workflow-going-into-2026-52fe1681325e)提供了实践经验，但缺乏严格的学术验证。

**尚缺少从"需求输入 → Flow+Data建模 → LLM代码生成 → 自动化验证"的端到端研究和实践。**本命题需要一个完整的工具链和方法论来支撑从需求到实现的闭环。

### 4.2 流程穷举的方法论空白

"穷举补充好流程步骤"是本命题的关键假设，但如何做到"穷举"本身面临挑战：

- **简单场景**：线性业务流程、CRUD操作，穷举相对容易。
- **中等复杂场景**：含分支、循环、异常处理的业务流程，ESA方法可以系统化处理。
- **高复杂场景**：并发、分布式、跨系统集成，穷举的难度急剧上升，Events x States的组合可能呈指数级增长。
- 现有研究未提供面向不同复杂度的分级穷举策略。
- 缺少自动化的流程完整性检查工具。
- 异常处理在需求阶段的建模仍被大量忽视——"Most FSM models however ignore exceptions, leaving the issue to the implementation level"[[IREB, Avnur]](https://re-magazine.ireb.org/print/a-finite-state-machine-model)。

### 4.3 复杂场景下的数据建模挑战

"Data"维度在现有研究中获得的关注远少于"Flow"维度：

- 大多数工作聚焦于流程/行为建模，对数据模型（Data Model）的建模指导不够充分。
- 数据模型与流程模型之间的交互关系（如：流程步骤如何读写数据、数据状态如何影响流程分支）的建模方法尚不成熟。
- 在涉及复杂数据转换、多表关联、事务一致性的场景中，纯粹依靠"Flow+Data"规格让LLM生成正确代码的可靠性尚未得到充分验证。
- 缺乏Flow与Data交叉一致性检查的自动化方法。

### 4.4 LLM能力边界尚未充分探索

- Flow2Code[[Flow2Code]](https://aclanthology.org/2025.findings-acl.425/)的评测显示当前LLM无法完美处理流程图到代码生成，但缺少对"LLM在何种复杂度阈值下开始失效"的精确测量。
- 多文件上下文退化（Pass@1降至~20%）[[Augment Code]](https://www.augmentcode.com/guides/ai-spec-driven-development-workflows)表明规模是核心挑战，但缺少对不同规模管理策略（如分层、分模块）效果的系统性研究。
- LLM对非结构化到结构化的转换（从自然语言需求到FSM）能力未被充分评估。
- 如何在AI的上下文窗口中有效表达大规模FSM仍是一个开放问题。

### 4.5 规格表达格式的碎片化

- 缺乏统一的"Flow+Data"规格标准格式。
- 现有的FSM工具（StateSmith、QP）使用各自的输入格式。
- LLM原生的规格格式（如GitHub的.spec.md）尚在早期阶段[[GitHub Blog]](https://github.blog/ai-and-ml/github-copilot/how-to-build-reliable-ai-workflows-with-agentic-primitives-and-context-engineering/)。
- 缺少规格格式的互操作标准，导致工具链之间难以集成。

### 4.6 规模化应用的实证不足

- 现有案例（如StateSmith、QP框架）主要集中在嵌入式、状态机等相对确定的领域。
- 在Web应用、微服务架构、数据处理管道等大规模业务场景中，"Flow+Data驱动AI实现"的效果尚缺乏系统性的实证数据。
- Spec-Driven Development报告的26%生产力提升[[Augment Code]](https://www.augmentcode.com/guides/ai-spec-driven-development-workflows)是单一来源的数据点，需要更多独立验证。
- 缺少不同规模项目（小型/中型/大型）的对比实验。

### 4.7 质量保障机制的研究空白

即使LLM能从Flow+Data规格生成代码，如何保证生成代码的质量：

- 缺少从Flow+Data规格自动生成测试用例的系统方法（虽然FSM测试用例生成有理论方法，但与LLM代码的对接尚不完善）。
- 生成代码的可维护性、可读性、安全性等质量属性的评估框架不完善。
- 当生成代码存在缺陷时，如何回溯到Flow+Data规格中的具体问题点，这一"诊断-修复"循环尚未被系统研究。
- 规格-代码双向同步机制不存在——当AI生成代码被人工修改后，如何反向更新规格。

---

## 5. 可行性评估

### 5.1 技术可行性：高（有条件）

**支持因素**：

1. **理论基础扎实**：SA/SD方法、FSM/Statechart理论经过数十年验证，IREB的分层模型提供了完整的需求到实现映射框架[[IREB, Avnur]](https://re-magazine.ireb.org/print/a-finite-state-machine-model)。
2. **工程先例丰富**：StateSmith、QP Framework等工具验证了确定性代码生成的可行性[[StateSmith]](https://github.com/StateSmith/StateSmith)[[Quantum Leaps]](https://www.state-machine.com/qpcpp/srs-qp.html)。
3. **AI能力快速提升**：Flow2Code虽然显示当前LLM有差距，但SFT可显著改善；LLM能力仍在持续进化[[Flow2Code]](https://aclanthology.org/2025.findings-acl.425/)。
4. **实践案例增多**：Osmani的90% AI生成率、Augment Code的26%效率提升都表明方向正确[[Osmani]](https://medium.com/@addyosmani/my-llm-coding-workflow-going-into-2026-52fe1681325e)[[Augment Code]](https://www.augmentcode.com/guides/ai-spec-driven-development-workflows)。
5. **FSM+AI混合方案有效**：清华大学的工业案例证明FSM约束可显著提升AI可靠性[[Zhang et al.]](https://arxiv.org/html/2603.01528v1)。

**限制条件**：

1. 需要结构化的规格输入（非自然语言自由描述）。
2. 需要分层的FSM建模（避免单一FSM过于复杂）。
3. 需要Human-in-the-Loop验证环节。
4. 当前不适用于需要复杂算法设计的场景。

### 5.2 场景化可行性评估

| 场景 | 可行性 | 依据 |
|------|--------|------|
| CRUD类业务功能（增删改查） | **高** | 流程简单、数据模型明确，LLM生成代码已较为成熟 |
| 状态机驱动的业务流程（订单流转、审批链） | **高** | 有StateSmith、QP等工具的直接支撑 |
| 表单处理与数据验证 | **高** | Flow（验证规则）+ Data（字段定义）清晰可穷举 |
| API接口实现 | **中-高** | 输入/输出数据模型明确，处理流程相对标准化 |
| 定时任务与批处理 | **中** | 流程可枚举，但异常处理和数据边界情况需仔细定义 |
| 算法实现 | **中** | Flow（算法步骤）可精确描述，但复杂算法的边界条件穷举有难度 |
| 高并发分布式系统 | **低** | 并发控制、一致性保障的流程极难完整穷举 |
| 复杂UI交互 | **低** | 用户交互流程分支繁多，状态组合爆炸 |
| 安全敏感系统 | **低** | 安全漏洞往往出现在"未预料到的流程路径"中，与穷举假设矛盾 |
| 跨系统集成 | **低-中** | 外部系统的行为不确定性增加了流程穷举的难度 |

### 5.3 经济可行性：高

**成本分析**：

| 投入项 | 估计成本 | 说明 |
|--------|----------|------|
| FSM建模培训 | 低 | IREB已有成熟的CPRE认证体系 |
| 规格编写工具 | 低 | StateSmith、draw.io等免费工具可用 |
| LLM调用成本 | 中 | 随模型优化持续下降 |
| 人工审查成本 | 中 | 仍需有经验的工程师审查AI输出 |

**收益分析**：

- 26%生产力提升[[Augment Code]](https://www.augmentcode.com/guides/ai-spec-driven-development-workflows)
- 减少"需求误解"导致的返工
- 自动化测试用例生成节省QA成本
- FSM规格的可复用性降低长期维护成本

### 5.4 时间可行性：中

**短期（0-6个月）**：

- 可实现：在已有FSM建模经验的项目中，使用LLM辅助代码生成
- 挑战：工具链整合、规格格式标准化

**中期（6-18个月）**：

- 可实现：AI辅助的FSM生成、规格-代码一致性验证
- 挑战：大规模系统的层次化FSM管理

**长期（18+个月）**：

- 可实现：端到端的规格驱动开发流水线
- 挑战：非FSM友好场景的适配、组织流程变革

### 5.5 风险评估

| 风险 | 概率 | 影响 | 缓解措施 |
|------|------|------|----------|
| LLM生成错误代码 | 高 | 高 | FSM测试用例自动验证 + 人工审查 |
| FSM建模不完整 | 中 | 高 | ESA方法 + Control Structure约束 |
| 团队FSM能力不足 | 中 | 中 | IREB培训 + 工具辅助 |
| 规格与代码不同步 | 中 | 中 | 自动化一致性检查工具 |
| 过度依赖FSM导致灵活性下降 | 低 | 中 | 保留自然语言补充描述 |

### 5.6 总体评估

本命题**在限定场景下具有较高的可行性**，但并非通用解决方案。其核心价值在于：

- 为AI辅助开发提供了一种**结构化、可重复、可验证**的方法论。
- 将需求工程与代码生成的鸿沟通过"Flow+Data"这座桥梁连接起来。
- 符合软件工程从"编码驱动"向"需求驱动"演进的大趋势。

但需要注意：**"穷举流程步骤"本身就是一个NP-hard性质的问题**——对于复杂系统，完全穷举在理论上和实践中都面临根本性挑战。因此，本命题更现实的定位是：**通过系统化的Flow+Data建模，最大化AI代码生成的覆盖率和准确率，同时承认仍需要人类参与处理边界情况和复杂场景。**

---

## 6. 建议

### 6.1 短期行动（0-3个月）

**建议一：建立Flow+Data规格模板体系**

基于IREB分层FSM模型[[IREB, Avnur]](https://re-magazine.ireb.org/print/a-finite-state-machine-model)和GitHub的.spec.md格式[[GitHub Blog]](https://github.blog/ai-and-ml/github-copilot/how-to-build-reliable-ai-workflows-with-agentic-primitives-and-context-engineering/)，创建项目规格模板：

```
模板结构：
1. General Layer
   - 状态列表及定义
   - 事件列表及定义
   - 状态转换表（States x Events）
2. Requirements Layer
   - 控制结构定义
   - 各状态的需求声明
3. Data Layer
   - 实体定义
   - 控制变量定义
   - 数据约束与关系
```

**建议二：在合适的项目中试点FSM+LLM开发流程**

选择特征：
- 业务流程清晰、状态转换明确的功能模块
- 已有FSM建模经验的团队
- 非安全关键级系统（降低试错风险）

工具选择：
- FSM建模：PlantUML + StateSmith
- LLM编码：Claude Code + CLAUDE.md规格文件
- 验证：FSM测试用例 + 人工审查

**建议三：积累规格到代码的配对数据集**

为后续SFT（Supervised Fine-Tuning）做准备：
- 记录每次FSM规格 -> AI生成代码 -> 人工修正的完整过程。
- 按Flow2Code[[Flow2Code]](https://aclanthology.org/2025.findings-acl.425/)的数据格式标准化。
- 目标：形成内部的高质量Flow-Code对齐数据集。

### 6.2 中期建设（3-12个月）

**建议四：开发AI辅助FSM生成工具**

利用LLM的自然语言理解能力，实现从自然语言需求到FSM的半自动转换：

```
输入：用户故事 + 领域知识
处理：LLM提取States、Events、Transitions
输出：FSM草图 + ESA完整性检查建议
人工：审查和修正FSM
```

参考清华大学的Event Identification方法[[Zhang et al.]](https://arxiv.org/html/2603.01528v1)和IREB的ESA流程[[IREB, Avnur]](https://re-magazine.ireb.org/print/a-finite-state-machine-model)。

**建议五：构建规格-代码一致性验证机制**

基于FSM的形式化特性，实现自动化的验证：

1. **转换覆盖率验证**：AI生成的代码是否覆盖了FSM中的所有状态转换？
2. **控制结构一致性**：代码中的变量赋值是否与FSM的控制变量定义一致？
3. **测试用例生成**：从FSM自动生成测试场景，验证AI代码的行为。

参考QP Framework的FSM Engine实现思路[[Quantum Leaps via IREB]](https://re-magazine.ireb.org/print/a-finite-state-machine-model)。

**建议六：建立组织级的规格驱动开发流程**

将SDD（Spec-Driven Development）方法[[Augment Code]](https://www.augmentcode.com/guides/ai-spec-driven-development-workflows)与FSM建模方法[[IREB, Avnur]](https://re-magazine.ireb.org/print/a-finite-state-machine-model)结合：

```
1. 需求分析 -> FSM建模（含Control Structure和ESA）
2. 规格评审 -> 团队审查FSM完整性
3. AI代码生成 -> LLM根据FSM规格 + 上下文生成代码
4. 自动验证 -> FSM测试用例 + 一致性检查
5. 人工审查 -> 关键路径的人工代码审查
```

### 6.3 长期愿景（12+个月）

**建议七：建设端到端的规格驱动AI开发平台**

整合以下能力：

- **规格编辑器**：支持PlantUML/draw.io + 自然语言混合输入
- **AI FSM生成**：从自然语言需求半自动生成FSM
- **LLM代码生成**：基于FSM规格的结构化Prompt生成代码
- **自动验证**：FSM一致性检查 + 测试用例自动生成
- **迭代反馈**：代码修改反向更新规格（规格-代码双向同步）

**建议八：研究层次化Flow对AI的可理解性**

面对复杂系统的规模挑战，研究：

- 如何将层次化FSM（Hierarchical Statechart）有效地表达为AI可理解的格式。
- 子FSM之间的接口和数据流如何在LLM上下文中高效传递。
- 参考QP Framework的Active Object模型[[Quantum Leaps]](https://www.state-machine.com/qpcpp/srs-qp.html)和GitHub的Context Engineering层[[GitHub Blog]](https://github.blog/ai-and-ml/github-copilot/how-to-build-reliable-ai-workflows-with-agentic-primitives-and-context-engineering/)。

**建议九：推动Flow+Data规格格式标准化**

联合学术界和工业界，推动Flow+Data规格格式的标准化，形成类似于OpenAPI对于API描述的行业标准。标准化内容包括：

- Flow描述的标准语法和语义（状态、事件、转换、条件）。
- Data描述的标准格式（实体、关系、约束）。
- Flow与Data交叉引用的标准规范。
- LLM可消费的标准输出格式。

---

## 参考来源汇总

| 编号 | 来源 | 作者/机构 | 类型 | 链接 |
|------|------|-----------|------|------|
| [1] | Structured Analysis and Structured Design (SA/SD) | GeeksforGeeks | 知识文章 | https://www.geeksforgeeks.org/system-design/structured-analysis-and-structured-design-sa-sd/ |
| [2] | Flow2Code: Evaluating LLMs for Flowchart-based Code Generation | ACL 2025 Findings | 学术论文 | https://aclanthology.org/2025.findings-acl.425/ |
| [3] | Generative AI for Requirements Engineering: A SLR | arXiv 2024 | 学术论文 | https://arxiv.org/abs/2409.06741 |
| [4] | FSM-Driven Streaming Inference: An Industrial Case | arXiv 2026 | 学术论文 | https://arxiv.org/abs/2603.01528 |
| [5] | StateSmith | 开源社区 | 开源工具 | https://github.com/StateSmith/StateSmith |
| [6] | QP Framework / Quantum Leaps | Quantum Leaps, LLC | 商业框架 | https://www.state-machine.com/qpcpp/srs-qp.html |
| [7] | Layered FSM Model | Arie Avnur / IREB Magazine | 学术文章 | https://re-magazine.ireb.org/print/a-finite-state-machine-model |
| [8] | Spec-Driven Development + AI | Augment Code | 技术博客 | https://www.augmentcode.com/guides/ai-spec-driven-development-workflows |
| [9] | Agentic Primitives & Context Engineering | GitHub Blog | 技术博客 | https://github.blog/ai-and-ml/github-copilot/how-to-build-reliable-ai-workflows-with-agentic-primitives-and-context-engineering/ |
| [10] | Requirements-as-Code | Emmanuel Goossaert | 行业文章 | https://www.linkedin.com/pulse/requirements-as-code-ai-augmented-software-engineers-goossaert-bbpje |
| [11] | LLM Coding Workflow Going Into 2026 | Addy Osmani | 行业文章 | https://medium.com/@addyosmani/my-llm-coding-workflow-going-into-2026-52fe1681325e |

---

*本报告基于2026年5月可获取的公开资料编写。所有数据引用均标注来源。AI领域发展迅速，部分数据和结论可能随时间推移需要更新。*
