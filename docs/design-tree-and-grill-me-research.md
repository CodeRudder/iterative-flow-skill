# 调研报告：Design Tree 与 Grill Me Skill

> 调研日期：2026-05-07
> 调研目标：(1) Design Tree 概念、方法论及其在软件设计中的应用；(2) Grill Me 技能的原理、用法及与 iterative-flow 的关联

---

## 一、Design Tree 概念调研

### 1.1 概念定义

**Design Tree（设计树）**是将一个设计问题分解为一连串分支选择的结构化方法。每个分支节点代表一个设计决策点，沿着每条分支往下追，逐步把方案想清楚，直到所有决策都被解决。

```
                        ┌── 选择A → 子决策A1 → ...
        ┌── 决策1 ──────┤
        │               └── 选择B → 子决策B1 → ...
根问题 ──┤
        │               ┌── 选择C → 子决策C1 → ...
        └── 决策2 ──────┤
                        └── 选择D → 子决策D2 → ...
```

### 1.2 学术起源

#### 1.2.1 Frederick Brooks — 理性模型中的设计树

**来源**：Frederick P. Brooks Jr., *The Design of Design: Essays from a Computer Scientist* (2010), Chapter 2

Brooks 在《设计原本》中提出 **Rational Model（理性模型）**，将设计过程概念化为对一棵 **"design tree"** 的搜索：

- 根节点 = 整体设计问题
- 每个分支 = 一个设计决策/选择
- 子分支 = 子决策
- 叶子节点 = 最终设计方案

> "The Rational Model assumes that design involves a search of the design tree."
> — Brooks, *The Design of Design*, Ch.2

**Brooks 自身的批判**：
1. 设计不是真正的树 — 实际决策间有跨分支的依赖，形成的是 **格（lattice）或 DAG**，不是干净的树
2. 需求无法预先完全确定 — 设计过程中需求会演化和涌现
3. 回溯是必要的 — 设计者经常根据后期发现来重新审视早期决策
4. 树的隐喻过度简化 — 忽略了约束和权衡之间的复杂网络

#### 1.2.2 Mary Shaw — 设计空间与设计树

**来源**：Mary Shaw, "The Role of Design Spaces" (IEEE Software, 2012); Mary Shaw & Marian Petre, "Design Spaces and How Software Designers Use Them" (Designing '24, 2024, arXiv:2407.18502)

Shaw 将 Brooks 的设计树扩展为 **Design Space（设计空间）** 概念：

- 设计空间是多维结构，每个维度代表一个关键架构决策
- 设计树是设计空间的一种便捷表示，但有暗示决策顺序的缺点
- 设计空间有 **两种节点类型**：choice（选择点）和 sub-group（子决策组）

**关键洞察**：
- 设计空间帮助设计者 **系统化理解备选方案**（范围和特征）
- 设计空间帮助 **系统化导航备选方案**（探索和决策过程）
- 设计空间支持 **有序的理解演化**（通过探索过程）
- 设计空间支持 **有序复用先验知识**（当现有方案适用时）

#### 1.2.3 Penders et al. — 紧耦合决策与依赖规则

**来源**：Penders et al., "Design-Space Exploration for Decision-Support Software" (ASE '22, 2022)

- 不同设计维度之间存在 **Inter-dependency Rules（交互依赖规则）**
- "当限制一个维度中的选择时，会直接影响依赖维度中可能的设计选择"
- 依赖关系形成 **导航指南**：检查一个设计维度后，逻辑上的下一步是检查依赖维度
- 强调 **对每个维度强制做出系统性决策** 的优势

### 1.3 工业实践

#### 1.3.1 Architecture Decision Records (ADR)

**来源**：Martin Fowler, [ArchitectureDecisionRecord](https://martinfowler.com/bliki/ArchitectureDecisionRecord.html); [adr.github.io](https://adr.github.io/)

ADR 是 Design Tree 思想在工业界的标准化实践：

- 每个 ADR 记录单个架构决策及其理由
- ADR 之间形成 **树状/链式关系**（superseded, amended, dependency-linked）
- 每个决策影响后续决策，形成可追溯的 **决策树**
- 被广泛采用：AWS、Spotify、Microsoft Azure、Red Hat 等

#### 1.3.2 Amazon 的单向门/双向门决策框架

- **单向门（One-Way Door）**：不可逆决策，需要慎重分析
- **双向门（Two-Way Door）**：可逆决策，可以快速尝试和调整
- 与设计树的关联：在设计树遍历中，先解决双向门决策（可回退），最后处理单向门决策

#### 1.3.3 Real Options Theory 在软件架构中的应用

**来源**：Sullivan et al., "Software Design as an Investment Activity: A Real Options Perspective"

- 将架构决策视为 **金融期权**
- 延迟不可逆决策能保留灵活性、最大化价值
- 好的架构不是"尽早做出所有决策"，而是 **"创造和保留选择权"**
- 敏捷实践天然利用了 Real Options：迭代式、可逆的决策

#### 1.3.4 火山引擎 — 模块树驱动设计

**来源**：[火山引擎开发者社区](https://developer.volcengine.com/articles/7322139449091948570)

- 提出基于 **"模块树"** 的系统设计方法论
- 用可视化的模块树指导系统设计
- 新增/修改/扩展系统能力时，对照模块树明确哪些模块需要修改
- 这是 Design Tree 思想在中文技术社区的工业应用案例

### 1.4 Design Tree 的方法论总结

| 阶段 | 活动 | 产出 |
|------|------|------|
| **识别** | 列出所有设计决策维度 | 决策维度清单 |
| **建模** | 构建设计树（决策→选项→子决策） | 设计树图 |
| **标注依赖** | 标识决策间的依赖关系 | 依赖关系图 |
| **遍历** | 逐分支遍历，每个节点做出选择 | 决策记录 |
| **验证** | 检查跨分支依赖是否满足 | 一致性报告 |
| **迭代** | 根据后续发现回溯修改早期决策 | 更新后的设计树 |

### 1.5 Design Tree vs Structured Flow Tree 对比

| 维度 | Design Tree | Structured Flow Tree |
|------|-------------|---------------------|
| **目标** | 探索设计空间，做出架构决策 | 穷举需求流程步骤，覆盖所有路径 |
| **问题性质** | 开放式（"怎么设计？"） | 封闭式（"怎么实现？"） |
| **树节点含义** | 决策选项 | 流程步骤 |
| **分支含义** | 可选方案 | 条件分支/异常路径 |
| **遍历策略** | 选择一条路径（其他被排除） | 穷举所有路径 |
| **终止条件** | 达到共识/理解 | 覆盖所有PRD需求 |
| **依赖关系** | 跨维度依赖（DAG） | 上下游步骤依赖（树） |
| **适用阶段** | 设计前期（What & How） | 需求分析后期（Step by Step） |
| **产出物** | ADR/设计文档 | 结构化流程文档 |

**关键互补性**：Design Tree 解决 **"做什么决策"**，Structured Flow Tree 解决 **"怎么执行"**。两者可以串联使用：
1. 先用 Design Tree 做设计决策
2. 再用 Structured Flow Tree 细化执行步骤

---

## 二、Grill Me Skill 调研

### 2.1 起源与背景

| 属性 | 信息 |
|------|------|
| **创建者** | Matt Pocock（TypeScript 专家，AI工程意见领袖） |
| **原始灵感** | Boris Cherny（Claude Code 创建者，Anthropic） |
| **仓库** | [github.com/mattpocock/skills](https://github.com/mattpocock/skills) |
| **安装量** | 90.5K+ |
| **走红** | 2025-2026 年，被称为 "最高 ROI 的 AI 编码技能之一" |

Boris Cherny 最初在社交媒体分享的提示：
> "Grill me on these changes and don't make a PR until I pass your test."

Matt Pocock 将其提炼为一个可复用的 Claude Code Skill。

### 2.2 完整技能内容

```
description: Interview the user relentlessly about a plan or design
until reaching shared understanding, resolving each branch of the
decision tree. Use when user wants to stress-test a plan, get grilled
on their design, or mentions "grill me".

Interview me relentlessly about every aspect of this plan until
we reach a shared understanding. Walk down each branch of the design
tree resolving dependencies between decisions one by one.

If a question can be answered by exploring the codebase, explore

For each question, provide your recommended answer.
```

仅 **4句话**，却产生了巨大的效果。

### 2.3 工作原理

```
用户提出模糊想法/计划
       │
       ▼
┌──────────────────────┐
│  AI 提出第一个问题     │ ← 从设计树的根节点开始
│  + 推荐答案            │
└──────────────────────┘
       │
       ▼
  用户回答（可直接说"yes"接受推荐）
       │
       ▼
┌──────────────────────┐
│  AI 提出下一个问题     │ ← 沿着设计树继续遍历
│  + 推荐答案            │
└──────────────────────┘
       │
       ▼
     ... 循环 ...
       │
       ▼
  所有分支解决 → AI 输出共识摘要
```

### 2.4 核心设计原则

| 原则 | 说明 |
|------|------|
| **逐步追问** | 每次只问一个问题，不一次抛出所有问题 |
| **Design Tree 遍历** | 沿设计树的每条分支走下去，逐个解决决策 |
| **依赖顺序** | 先解决被依赖的决策，再解决依赖它的决策 |
| **自动探索** | 如果问题可以通过读代码回答，AI 直接去读 |
| **推荐答案** | 对有明显好答案的问题，AI 直接给出推荐，用户只需说"yes" |
| **持续到共识** | 直到双方对方案达成完全理解才停止 |

### 2.5 解决的问题

| 问题 | Grill Me 如何解决 |
|------|------------------|
| **需求模糊** | 通过逐个追问澄清每个模糊点 |
| **AI 写了一堆垃圾** | 先达成共识再动手写代码 |
| **遗漏边界情况** | AI 作为反方角色，主动提出边界问题 |
| **隐性假设** | 通过提问暴露设计者自己没意识到的假设 |
| **跳跃到方案** | 强制逐分支解决，防止跳过重要决策 |
| **上下文窗口溢出** | 在"聪明区间"（<100K tokens）内达成共识 |

### 2.6 工作流集成

Matt Pocock 推荐的完整工作流：

```
1. /grill-me      → 通过追问达成设计共识
2. /write-a-prd   → 将共识转化为 PRD
3. /prd-to-issues → 将 PRD 拆解为 GitHub Issues
4. /tdd           → TDD 方式实现
```

### 2.7 变体

| 变体 | 说明 |
|------|------|
| `/grill-me` | 标准版，通用追问 |
| `/grill-with-docs` | 结合代码库上下文的追问 |
| 非编码版 | 去掉 "plan" 限定，用于课程设计、商业决策等 |
| `domain-model` | Matt 现在更推荐的起点，用领域建模替代直接追问 |

### 2.8 与 iterative-flow 的关系分析

#### 2.8.1 互补性

```
Grill Me (Design Tree 遍历)
    │
    │ 达成设计共识 → 输出：设计决策 + 理解
    ▼
iterative-flow (Structured Flow Tree 生成)
    │
    │ 穷举流程步骤 → 输出：结构化流程文档
    ▼
代码实现
```

- **Grill Me** 解决的是 **"我们要做什么决策"** — 在设计空间中导航
- **iterative-flow** 解决的是 **"这些决策怎么执行"** — 将决策细化为可实现的步骤

#### 2.8.2 可以借鉴的模式

| Grill Me 模式 | iterative-flow 可借鉴之处 |
|--------------|--------------------------|
| 逐分支解决 | 在流程分解时，也可以逐阶段确认，而不是一次性生成整个流程树 |
| 推荐答案 | 在生成流程时，对有标准模式的步骤主动推荐最佳实践 |
| 自动探索代码 | 在细化流程时，可以参考已有代码结构和模式 |
| 持续到共识 | 与用户逐步确认每个阶段/子树的正确性 |
| 依赖顺序 | 先确定骨干流程，再展开细节（已是 iterative-flow 的分层策略） |

#### 2.8.3 潜在整合方案

**Option A：Grill Me 作为前置步骤**
```
用户需求 → /grill-me → 设计共识 → iterative-flow → 结构化流程文档 → 代码
```

**Option B：将 Grill Me 模式嵌入 iterative-flow**
```
在 iterative-flow 的"需求分析"阶段引入设计树遍历：
  - 对每个实体生命周期状态机进行追问确认
  - 对每个关键流程分支进行追问确认
  - 对每个异常路径进行追问确认
```

**Option C：独立使用，互为补充**
```
设计决策问题 → /grill-me → ADR
需求流程问题 → iterative-flow → 结构化流程文档
```

---

## 三、核心学术/工业参考

### 3.1 核心论文与书籍

| 编号 | 文献 | 关键贡献 |
|------|------|----------|
| B1 | Brooks, *The Design of Design* (2010) | 提出 Design Tree 概念（Rational Model），同时批判其局限性 |
| B2 | Shaw, "The Role of Design Spaces" (IEEE Software, 2012) | 扩展为 Design Space，提出多维决策空间 |
| B3 | Shaw & Petre, "Design Spaces and How Software Designers Use Them" (2024) | 系统化设计空间分类和使用方式 |
| B4 | Penders et al., "Design-Space Exploration for Decision-Support Software" (ASE '22) | 决策间的依赖规则和导航指南 |
| B5 | Sullivan et al., "Software Design as an Investment Activity: A Real Options Perspective" | Real Options 理论应用于软件设计决策 |
| B6 | Dorst & Cross, "Creativity in the design process: co-evolution of problem-solution" (2001) | 问题空间与解决方案空间的共同演化 |

### 3.2 工业参考

| 编号 | 参考 | 说明 |
|------|------|------|
| I1 | [Matt Pocock's skills repo](https://github.com/mattpocock/skills) | Grill Me 技能源码 |
| I2 | [AI Hero: Grill Me 详解](https://www.aihero.dev/my-grill-me-skill-has-gone-viral) | Grill Me 技能深度解析 |
| I3 | [Martin Fowler: ADR](https://martinfowler.com/bliki/ArchitectureDecisionRecord.html) | ADR 定义 |
| I4 | [adr.github.io](https://adr.github.io/) | ADR 门户 |
| I5 | [火山引擎：模块树驱动设计](https://developer.volcengine.com/articles/7322139449091948570) | Design Tree 中文工业实践 |
| I6 | [SEI CMU: Studying Software Architecture Through Design Spaces](https://www.sei.cmu.edu/documents/5811/) | 设计空间研究 |
| I7 | [Figr: Software Decision Tree](https://figr.design/blog/software-decision-tree) | 状态感知分支决策树 |

### 3.3 关键人物

| 人物 | 角色 |
|------|------|
| Frederick P. Brooks Jr. | Design Tree 概念提出者（《人月神话》作者） |
| Mary Shaw | Design Space 理论奠基人（CMU 教授） |
| Matt Pocock | Grill Me Skill 创建者（TypeScript/AI 工程专家） |
| Boris Cherny | Grill Me 理念灵感来源（Claude Code 创建者，Anthropic） |

---

## 四、对 iterative-flow 的启示

### 4.1 直接可用的概念

1. **设计树遍历模式**：在 iterative-flow 的需求分析阶段，可以引入类似 grill-me 的追问模式，逐个确认实体生命周期、关键分支决策
2. **推荐答案模式**：对常见业务流程模式（如审批链、支付回调、库存锁定），主动推荐标准实现模式
3. **依赖顺序**：先确定骨干流程（L1），再展开细节（L2/L3），这与 Design Tree 的依赖排序一致

### 4.2 需要注意的差异

1. **Design Tree 是选择一棵分支**（其他被排除），**Flow Tree 是穷举所有分支**（包括异常路径）
2. **Design Tree 的节点是决策**，**Flow Tree 的节点是步骤**
3. **Design Tree 允许回溯**，**Flow Tree 一旦生成就需要保持一致性**

### 4.3 潜在的 Skill 整合

可以考虑为 iterative-flow 增加一个前置 skill：`/grill-me-on-requirements`

```
description: 在生成结构化流程前，先通过追问确认需求细节。
Use when user provides PRD and wants to generate structured flow.

针对这份PRD，逐个确认以下关键决策点：
- 实体边界和生命周期
- 关键分支条件
- 异常路径预期
- 角色和权限
- 业务规则

对每个决策点，提供推荐答案。
只在所有关键决策确认后，才开始生成结构化流程文档。
```

这样可以显著减少 iterative-flow 生成后的返工，因为在生成前就已经通过追问消除了歧义。

---

## 五、结论

**Design Tree** 和 **Grill Me** 本质上是同一个思想的两个面向：

- **Design Tree** 是理论：设计 = 在决策树中导航
- **Grill Me** 是实践：让 AI 作为导航员，引导设计者走完这棵树

对于 iterative-flow 而言：
- Design Tree 思想可以用于 **需求分析阶段**（先做设计决策）
- Grill Me 模式可以用于 **交互流程**（通过追问确认需求细节）
- 两者与 Structured Flow Tree 是 **互补关系**，不是替代关系
