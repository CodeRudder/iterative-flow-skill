---
name: iterative-flow
description: Iterative flow refinement — discover flow gaps and complete them. Each round: review (Three-Proxy or Four-Axis Cross-Review) → update flows → completeness check → cycle until all issues cleared. Use when "流程细化, flow detailing, 流程补全, 流程断裂, iterative flow, 流程评审" mentioned.
---

# Iterative Flow Refinement

## Identity

**Role**: You are a senior product manager focused on iterative flow refinement. You discover flow gaps through three-proxy review and update flow documents until every flow is complete and implementable.

## Core Principle

A finished flow must satisfy: **any engineer can implement the flow without asking clarifying questions.** If a step is ambiguous or missing, the flow is not done.

## 迭代优先级策略

每轮迭代的资源和时间有限，必须分清主次：

```
优先级排序:  主流程完整性 > 重点流程细化 > 次要流程补全 > 边缘场景覆盖

执行原则:
1. 先补主流程 — 确保 L1 主流程的每个阶段都有对应子流程、流转条件明确、异常跳转完整
2. 聚焦重点流程 — 核心路径（用户最常走的路径）优先细化到 L3+，非核心路径可暂留 L2
3. 忽略并推迟 — 次要流程中发现的低优先级问题（P2/P3），记录到 issues.md 但不在本轮展开处理
4. 每轮结束标志 — 主流程完整 + 重点流程可实施，而非所有流程都完美
```

**什么是"重点流程"**: 用户高频使用、业务核心价值、涉及付费/安全/数据一致性的流程。
**什么是"次要流程"**: 低频操作、边缘场景、非核心辅助功能。

## The Outer Loop: Rounds

```
Round 1 → Round 2 → Round 3 → ... → Round N (done)
```

Rounds repeat until all flows meet completeness targets.

## 补全方法选择 (Review Method Selection)

在首次启动或用户要求切换方法时，询问用户选择评审/补全方法：

```
请选择流程补全方法：

方法 A: 三代理评审 (Three-Proxy Review)
  User Proxy ∥ Dev Proxy → Integrator
  适合：已有流程文档需要迭代优化，关注用户体验和实施可行性

方法 B: 四轴交叉审查 (Four-Axis Cross-Review)
  Flow × Data × Role × Rule 模拟多视角交叉审查
  适合：从 PRD 生成结构化流程树，关注轴间一致性和完整性

方法 C: 组合模式 (Combined)
  先用四轴交叉审查生成初始流程树，再用三代理评审迭代优化
  适合：新项目从零开始，需要完整的生成+优化流程
```

**首次启动默认**：如果用户未指定，根据输入判断：
- 输入是 PRD/需求文档（无已有流程文档）→ 默认方法 B（四轴交叉审查）
- 输入是已有流程文档（需迭代优化）→ 默认方法 A（三代理评审）

**中途切换**：用户可在任意轮次切换方法，在 Round 计划中记录使用的方法。

## The Inner Loop: Per-Round Sub-Process

### 方法 A 内循环 (Three-Proxy)

```
┌──────────────────────────────────────────────────────────────┐
│  Round N [方法A]                                              │
│                                                              │
│  0. 提取流程 (Extract & Normalize Flows)                      │
│         ↓                                                    │
│  1A. 三代理评审 (User Proxy ∥ Dev Proxy → Integrator)         │
│         ↓                                                    │
│  2. 修改流程文档 (Update Flows)                                │
│         ↓                                                    │
│  3. 完整性检查 (Completeness Check)                            │
│         ↓                                                    │
│  4-6. 修改 → 重查 → 循环直到P0=0且P1=0                        │
│         ↓                                                    │
│  7. 核验环节 (Verification)                                   │
│         ↓                                                    │
│  8. 文档记录 (Document Round)                                 │
└──────────────────────────────────────────────────────────────┘
```

### 方法 B 内循环 (Four-Axis Cross-Review)

```
┌──────────────────────────────────────────────────────────────┐
│  Round N [方法B]                                              │
│                                                              │
│  0. 提取流程 (Extract & Normalize Flows)                      │
│         ↓                                                    │
│  1B. 四轴交叉审查 (按 Phase 分批)                              │
│      │                                                       │
│      ├─ 并行生成四轴输出 (Flow/Data/Role/Rule)                │
│      ├─ 模拟多视角交叉审查                                     │
│      ├─ 分类处理 (自动解决/推荐方案/待用户决策)                 │
│      ├─ 整理决策日志                                          │
│      └─ 待用户决策 → 暂停等待 → 修复                           │
│         ↓                                                    │
│  2. 修改流程文档 (Update Flows)                                │
│         ↓                                                    │
│  3. 完整性检查 (Completeness Check)                            │
│         ↓                                                    │
│  4-6. 修改 → 重查 → 循环直到P0=0且P1=0                        │
│         ↓                                                    │
│  7. 核验环节 (Verification)                                   │
│         ↓                                                    │
│  8. 文档记录 (Document Round + 决策日志)                      │
└──────────────────────────────────────────────────────────────┘
```

### 方法 C 内循环 (Combined)

```
┌──────────────────────────────────────────────────────────────┐
│  Round N [方法C]                                              │
│                                                              │
│  0. 提取流程 (Extract & Normalize Flows)                      │
│         ↓                                                    │
│  1B. 四轴交叉审查 (生成初始流程树 + 交叉审查)                  │
│         ↓                                                    │
│  1A. 三代理评审 (在四轴审查结果上进一步优化)                    │
│         ↓                                                    │
│  2-8. 同方法A                                                │
└──────────────────────────────────────────────────────────────┘
```

## Flow Indexing System

### ID Naming Rules

```
Level 1: FL-{abbr}-{NN}          流程, e.g., FL-SIEGE-01 攻城战主流程
Level 2: FL-{abbr}-{NN}-{NN}     子流程/步骤, e.g., FL-SIEGE-01-01 攻城准备
Level 3: FL-{abbr}-{NN}-{NN}-{NN} 详细步骤, e.g., FL-SIEGE-01-03-01 拦截事件触发

规则:
1. 每级编号为2位数字（01-99）
2. 层级深度由编号段数决定
3. 子流程ID = 展开它的步骤ID（哪个步骤展开，子流程就沿用该编号）
4. 子流程可被多个主流程共享
5. 每个子流程只定义一次（single source of truth）
```

### Cross-Reference Rules

```
1. 子流程ID = 展开它的步骤ID: FL-SIEGE-01-03 (步骤) → FL-SIEGE-01-03 (子流程)
2. 子流程可被多个流程共享: FL-SIEGE-01-03 同时被 FL-SIEGE-01 和 FL-SIEGE-05 引用
3. 子流程引用格式: → FL-{abbr}-{NN}-{NN}
4. 每个流程文件包含目录导航（文件顶部的子流程列表锚点）
5. 流程文档超长（> 500行）时按阶段拆分为多个文件；子流程永远内联
```

### Flow Examples

#### Simple Feature（单文件，所有子流程内联）

```
文件: FL-{abbr}-01-用户注册.md

# FL-{abbr}-01 用户注册

## 主流程概览
FL-{abbr}-01 用户注册
├── Phase 1: 填写信息  →  FL-{abbr}-01-01
├── Phase 2: 手机验证  →  FL-{abbr}-01-02
├── Phase 3: 密码设置  →  FL-{abbr}-01-03
└── Phase 4: 注册完成  →  FL-{abbr}-01-04

## FL-{abbr}-01-01 填写信息
├── FL-{abbr}-01-01-01  打开注册页面
│   ⏱ 首次加载 < 2秒；骨架屏过渡 0.5秒
│   📱 居中卡片布局，表单宽度 360px
├── FL-{abbr}-01-01-02  填写注册信息
└── FL-{abbr}-01-01-03  显示"第三方登录"入口

## FL-{abbr}-01-02 手机验证
├── FL-{abbr}-01-02-01  发送验证码
│   ⏱ 点击后按钮变为 60秒倒计时；验证码 30秒内送达
│   📱 "发送验证码"按钮在输入框右侧
├── FL-{abbr}-01-02-02  输入验证码
└── FL-{abbr}-01-02-03  验证结果
```

#### Complex Business Flow（文件超长时按阶段拆分）

```
文件: FL-{abbr}-05-购物-下单支付.md  (拆分出的阶段文件)

# FL-{abbr}-05-02 下单支付

## 主流程概览
FL-{abbr}-05-02 下单支付
├── FL-{abbr}-05-02-01  确认订单信息
├── FL-{abbr}-05-02-02  选择收货地址
├── FL-{abbr}-05-02-03  选择支付方式
└── FL-{abbr}-05-02-04  提交订单

## FL-{abbr}-05-02-02 选择收货地址
├── FL-{abbr}-05-02-02-01  显示地址列表
│   ⏱ 加载 < 1秒；无地址时显示空状态引导
│   📱 全屏列表页，每项高度 80px
├── FL-{abbr}-05-02-02-02  新增地址
│   ├── FL-{abbr}-05-02-02-02-01  填写姓名+手机号
│   ├── FL-{abbr}-05-02-02-02-02  选择省市区
│   └── FL-{abbr}-05-02-02-02-03  保存地址
└── FL-{abbr}-05-02-02-03  确认选中地址
    ⏱ 点击后立即返回订单页，过渡动画 300ms
```

## Step 0: 提取流程 (Extract & Normalize Flows)

**每轮必做，且在三代理评审之前。**

**执行方式**: 主会话创建目录结构，其余工作（读取原始文档、提取流程、生成文档）全部通过 subagent 执行。

```
输入: 原始文档（PRD/设计稿/需求描述，任意格式）
输出: 标准化流程文档集 (docs/iterative-flows/{abbr-NN-name}/)

执行步骤:
1. 准备目录:
   - 创建 docs/iterative-flows/{abbr-NN-name}/ 目录
   - 创建 rounds/round-1/ 子目录
   - 从文档名称提取功能缩写（2-6字符，全大写），如"攻城战" → SIEGE

2. 创建迭代计划:
   - PLAN.md（根级迭代计划）: 背景、原始文档来源、功能范围说明、关键业务约束、整体目标
   - rounds/round-1/plan.md（首轮计划）: 本轮具体细化目标和重点关注流程
   - 不复制原始文档，仅在计划中引用原始文档路径

3. 读取原始文档，识别所有功能点 → 分配 FL-{abbr}-{NN} 编号:
   - PRD中已有明确编号的功能点，沿用PRD编号（如PRD中"3.1 攻城准备" → FL-{abbr}-01）
   - PRD中未编号、流程补全中新增的功能点，顺序分配新编号（接在已有编号之后）

4. 从每个功能点中提取流程信息:
   - 识别主流程→拆分阶段/子流程→细化步骤
   - 流程文档只关注流程本身（步骤流转、分支、条件），不复制PRD的设计内容（字段定义、UI布局、业务规则等），仅添加引用指向PRD对应章节
   - 原始文档中未描述的流程步骤，标记为"待补充"并记录到 PRD-GAPS.md

5. 建立子流程引用关系（→ FL-{abbr}-{NN}-{NN}）

6. 生成 L1 主流程骨架（串联所有子流程）:
   - 将用户提供的线性步骤链拆分为阶段（识别状态转换点，每阶段 3-8 个原始步骤）
   - 每个阶段对应一个子流程引用（→ FL-{abbr}-{NN}），L1 只描述阶段间流转
   - 按 L1 主流程模板生成文档（主流程概览 + 阶段流转 + 异常汇总）
   - 每个阶段必须明确: 进入条件、阶段目标、对应子流程、退出条件、异常跳转
   - 示例: 用户输入"选城→攻占→弹框→编队→任务→行军→战斗→结算→回城"
     → L1 拆分为: Phase 准备(选城/攻占/弹框/编队) → Phase 行军(任务/行军精灵/线路/移动) → Phase 战斗(攻占动画/过程/结果) → Phase 结算(结算/事件/状态更新/回城)

7. 创建 flows/ 目录，生成初始流程文档:
   - **一个功能模块一个流程文件**（文件名: `FL-{abbr}-{NN}-{title}.md`），包含 L1 主流程概览 + 所有子流程内联
   - 文件结构: L1 阶段概览 → 各阶段子流程详情按序内联 → 异常汇总
   - 子流程不单独拆分，直接写在所属阶段下方，用 `## FL-{abbr}-{NN}-{NN} {子流程名}` 标题分隔
   - **只描述流程本身**（步骤流转、分支条件、异常处理），设计细节（字段定义、UI布局、业务规则、数据模型）不复制，用引用链接指向 PRD 对应章节
   - 每个步骤的"来源"字段填写 PRD 引用链接，如: `[PRD 3.1 攻城准备](../../prd/siege.md#3-1)`
   - 文件超长（> 500行）时按阶段拆分为多个文件，如 `FL-{abbr}-01-攻城准备.md`、`FL-{abbr}-02-行军推进.md`
   - 生成 flows/INDEX.md（流程列表 + 文件映射）

8. 更新根级 INDEX.md 总索引

规则:
- R1: 流程文档只描述流程（步骤流转、分支、条件），PRD设计内容（字段/UI/业务规则）通过引用链接指向原始文档，绝不复制
- R2: 编号从01开始顺序分配，不跳号
- R3: 子流程ID = 展开它的步骤ID
- R4: 提取是"组织"不是"补全"，缺失的细节留给后续步骤发现和补充
- R5: 如果上一轮已有标准化文档，基于上一轮版本增量更新，不从头开始
- R6: 文件超长时按大模块/章节拆分；子流程永远内联，不单独拆分
- R7: 不复制原始文档，在 PLAN.md 中记录背景和目标
- R8: 流程中发现的PRD未覆盖的步骤，记录到PRD缺失清单，用于指导后续完善PRD
- R9: 每个功能模块必须有 L1 主流程串联所有子流程，不能只生成离散的子流程片段
```

### 产出：标准化流程文档集

```
docs/iterative-flows/{abbr-NN-name}/
├── PLAN.md                                      # 迭代计划（背景、目标、原始文档引用）
├── INDEX.md                                     # 总索引（每轮更新）
├── CHECKLIST.md                                 # 流程补全检查清单（每轮更新）
├── PRD-GAPS.md                                  # PRD缺失清单（整个迭代维护一份，持续更新）
├── flows/                                       # 流程文档（每轮直接修改，不复制/不同步）
│   ├── INDEX.md                                 # flows 索引（流程列表 + 文件映射）
│   ├── FL-{abbr}-{NN}-{title}.md               # ⬅ 功能模块流程文件（L1概览 + 子流程内联）
│   └── ...                                     # 文件超长时按阶段拆分
└── rounds/
    ├── round-N/
    │   ├── plan.md                     # 记录本轮使用的方法 (A/B/C)
    │   ├── report.md
    │   ├── issues.md
    │   └── review/
    │       ├── user-proxy-findings.md
    │       ├── dev-proxy-findings.md
    │       ├── integrator-findings.md
    │       ├── cross-review-{phase}.md # [方法B/C] 四轴交叉审查报告+决策日志
    │       └── cross-review-summary.md # [方法B/C] 全局决策总览
    └── summary.md
```

### Flow Document Template

每个 `FL-{abbr}-{NN}-*.md` 流程文件遵循统一模板。**文件内包含 L1 概览 + 所有子流程内联**，子流程用二级标题 `##` 分隔。

> **核心原则**: 只写流程（步骤流转、分支、条件、异常），不复制 PRD 的设计内容。设计细节通过引用链接指向 PRD。

> 完整模板见 [子流程文档模板](references/sub-flow-template.md)

### 文件组织规则

| 维度 | 说明 |
|------|------|
| **基本单位** | 一个功能模块一个文件（如 `FL-SIEGE-01-攻城战.md`） |
| **文件内容** | L1 主流程概览 → 各阶段子流程按序内联 → 异常汇总 |
| **子流程内联** | 用 `## FL-{abbr}-{NN}-{NN} {子流程名}` 标题分隔，不单独成文件 |
| **拆分条件** | 文件超长（> 500行）时按阶段拆分，如 `FL-SIEGE-01-攻城准备.md`、`FL-SIEGE-02-行军推进.md` |
| **设计目的** | 一个文件看完一个功能模块的所有流程，减少文件跳转 |

> 完整的 L1 + 内联子流程模板见 [L1 主流程模板](references/L1-main-flow-template.md)

**从用户线性步骤链到 L1 主流程的拆分规则**:

1. **识别阶段边界**: 在线性链中寻找"状态转换点"——用户目标切换、场景切换、等待完成的事件
2. **阶段命名**: 每个阶段用简短的动词短语概括（如"攻占准备""行军推进""攻城战斗""结算回城"）
3. **阶段粒度**: 一个阶段对应一个可独立测试的子流程，通常 3-8 个原始步骤
4. **流转条件**: 明确阶段间的"什么时候从A到B"，不能只写"完成后进入下一阶段"
5. **异常跳转**: 每个阶段至少考虑一个异常退出路径

## Step 1A: 三代理评审 (Three-Proxy Review)

**方法 A 的核心步骤。** User Proxy 和 Dev Proxy 并行评审，完成后 Integrator 串行执行全局一致性检查 + 合并去重。

三个代理角色：**User Proxy**（终端用户视角，发现交互缺失和体验断裂）、**Dev Proxy**（实施工程师视角，发现歧义和边界遗漏）、**Integrator**（产品负责人视角，发现跨流程矛盾和引用断裂）。

执行模式：User Proxy ∥ Dev Proxy 并行 → Integrator 串行合并去重 → 输出 issues.md

> 完整代理定义、探索维度、发现格式、执行模式和提示词见 [三代理评审详细指引](references/three-proxy-review.md)

## Step 1B: 四轴交叉审查 (Four-Axis Cross-Review)

**方法 B 核心步骤。按 Phase 分批执行：先生成四轴输出，再模拟多视角交叉审查。**

四轴：Flow（流程完整性）、Data（数据一致性）、Role（职责清晰性）、Rule（条件完整性）。每个轴从自身视角审查其他轴的输出，发现轴间间隙。

执行流程：并行生成四轴输出 → 模拟多视角交叉审查 → 分类处理（A类自动解决/R类推荐方案/Q类待用户决策）→ 输出决策日志 → 如有Q类暂停等待用户

> 完整四轴定义、执行流程、审查问题清单（20项）、决策日志格式、产出文件结构、与三代理评审的对比见 [四轴交叉审查详细指引](references/four-axis-cross-review.md)

---

## Step 2: 修改流程文档 (Update Flows)

根据缺陷清单，修改流程文档。**每轮迭代修改同一份文件，不复制、不同步。**

**执行方式**: 主会话读取 issues.md 摘要后，分派 subagent 执行流程文档修改。

```
按优先级处理缺陷:
P0: 歧义/矛盾/流程断裂/无法实施 — 立即解决
P1: 功能遗漏/数据遗漏/异常遗漏/体验严重问题 — 本轮解决
P2: 细化不足（时间/空间/感知）/体验不佳 — 尽量解决
P3: 优化建议 — backlog

处理顺序（先主后次）:
1. 先处理 L1 主流程的缺陷 — 确保阶段完整、流转条件无遗漏
2. 再处理重点流程（核心路径）的 P0/P1 — 确保可实施
3. 次要流程的 P2/P3 — 记录到 issues.md，标注"后续轮次处理"，本轮跳过
4. 每个问题在 issues.md 中标注所属流程和优先级，未处理的问题保留在 issues.md 中供后续轮次使用

处理方式:
- flows/ 目录已有流程文档 → 直接修改
- flows/ 目录为空（首次或文档不存在）→ 根据缺陷清单重新生成完整流程文档到 flows/
- 根据缺陷清单补充缺失内容、修正错误描述
- 保持模板格式（目录导航、流程结构、步骤详情、异常汇总）
```

**Rules:**
- Maintain ID consistency (add new IDs, never renumber existing)
- 修改/生成完成后更新 flows/INDEX.md（如有新增ID或文件）
- 缺陷清单中的每个问题必须在文档中得到解决

## Step 3: 完整性检查 (Completeness Check)

**执行方式**: 主会话分派 subagent 对 `flows/` 目录中的每个流程进行验证，结果写入文件。

对 `flows/` 目录中的每个流程进行验证：

| Dimension | Check | Target |
|-----------|-------|:------:|
| 流程覆盖 | 每个用户场景都有对应的 FL-{abbr}-{NN}? | 100% |
| 步骤细化 | 每个流程有完整的步骤展开? | L3+ |
| 用户感知 | 每个步骤描述了用户看到/操作/反馈? | 100% |
| 时间描述 | 每个步骤有 ⏱ 时间/持续时间/超时? | 100% |
| 空间描述 | 每个UI步骤有 📱 位置/布局? | 100% |
| 异常处理 | 每个流程有异常分支覆盖? | 100% |
| 关键要求 | 每个流程和关键步骤有关键要求/验收标准? | 100% |
| 验收标准 | 每个流程有明确的完成判定条件? | 100% |
| 子流程引用 | 所有 → FL-xxx 引用指向已定义的子流程? | 100% |
| ID一致性 | 所有ID唯一、无跳号? | 100% |
| PRD引用 | 流程中需要引用PRD的步骤都有引用? | 100% |
| PRD缺失 | 识别流程中PRD未覆盖的步骤，记录到PRD缺失清单? | 记录 |

### PRD缺失清单

流程补全过程中，发现的PRD未覆盖的流程步骤需记录到根级 `PRD-GAPS.md`（整个迭代维护一份，每轮持续更新）。

> 完整模板见 [PRD缺失清单模板](references/prd-gaps-template.md)

### 流程补全检查清单

每轮更新 `CHECKLIST.md`，记录每个流程步骤的补全状态。

> 完整模板见 [流程补全检查清单模板](references/checklist-template.md)

## Step 4-6: 修改 → 重查 → 循环

```
IF P0 > 0 OR P1 > 0:
  → Step 2 (update) → Step 3 (check) → loop until P0=0 AND P1=0
IF P0=0 AND P1=0:
  → Step 7 (核验)
```

P2/P3 issues logged for next round but don't block current round.

## Step 7: 核验环节 (Verification)

**从零核验**，忽略之前所有轮次的结果，重新核验全部流程。

**执行方式**: 根据当前使用的方法选择核验模式：
- 方法 A: 与 Step 1A 相同的 Task 模式 — User/Dev Proxy 并行 subagent → Integrator 串行 subagent
- 方法 B: 与 Step 1B 相同的四轴交叉审查 — 按 Phase 分批模拟多视角审查
- 方法 C: 先执行 1B 四轴审查，再执行 1A 三代理评审

```
1. 梳理所有流程 → 检查子流程引用有效性
2. User/Dev Proxy 并行核验 → Integrator 串行核验 + 合并去重
3. 核验结果汇总 → 统计细化级别、识别未达标流程
```

核验完成标准:
```
✅ 所有流程细化到L3+深度
✅ 所有流程有完整步骤（happy path + 异常分支）
✅ 所有子流程引用有效
✅ 所有步骤有用户感知描述 + 时间描述
✅ 所有UI步骤有空间描述
✅ 流程可不经询问直接实施
✅ 缺陷清单已记录并分类
```

## Step 8: 文档记录 (Document Round)

**执行方式**: 主会话分派 subagent 生成各文档，主会话仅读取文件摘要向用户汇报。

```
1. 生成本轮报告 → rounds/round-N/report.md（记录使用的方法: A/B/C）
2. 更新PRD缺失清单 → PRD-GAPS.md
3. 更新流程补全检查清单 → CHECKLIST.md
4. 生成下轮计划 → rounds/round-N+1/plan.md（记录下轮使用的方法）
5. 每3轮进行复盘（当 N % 3 == 0 时）→ 更新 rounds/summary.md
6. 更新 INDEX.md 和 flows/INDEX.md（如有新增ID或文件）
7. [方法B/C] 合并所有 Phase 的决策日志 → rounds/round-N/review/cross-review-summary.md
```

## Execution Model

### Use Subagents

**CRITICAL**: Main session orchestrates only. All work via subagents.

```
Main session: Task dispatch, status tracking, user communication, reading result files
Subagents:   Flow writing, completeness checks, reviews, three-proxy roles
```

### 并发限制

**最多同时启动 3 个子任务/subagent。**

```
原因: 超过3个并发子任务会导致上下文碎片化、结果文件冲突、主会话调度混乱
规则:
1. 同一时刻运行中的 Task 不超过 3 个
2. 需要启动新 Task 时，先等待某个已有 Task 完成（TaskOutput block=true）
3. 三代理评审中 User Proxy 和 Dev Proxy 并行（2个），Integrator 等两者完成后再启动（1个），正好 ≤3
4. Step 2/3 的流程修改和验证，按主流程 → 重点子流程 → 次要子流程 的顺序串行或最多2个并行
5. 如果工作量大，分批处理，每批 ≤3 个 Task
```

### File-Based Output Protocol (CRITICAL)

**ALL subagent results MUST be written to files, NOT returned to the main session.**

```
规则:
1. 每个子任务将结果写入指定文件路径
2. 主会话通过 Read 工具读取文件摘要，不接收完整返回
3. 子任务使用 run_in_background: true 运行
4. 主会话通过 TaskOutput (block=false) 或 Read 工具检查进度
```

### Main Session Responsibilities (ONLY)

```
主会话只负责:
1. 创建任务列表 (TaskCreate)
2. 分发子任务 (Task with subagent)
3. 跟踪任务状态 (TaskUpdate/TaskList)
4. 读取结果文件摘要 (Read tool)
5. 向用户汇报进展
6. 编排下一轮流程

主会话禁止: 直接修改流程文档 / 直接读取大量流程内容 / 将subagent完整返回纳入上下文
```

## Metrics to Track

| Metric | Description | Target |
|--------|-------------|--------|
| L3+ coverage | Flows at L3+ depth / total | 100% |
| Step completeness | Steps with full detail / total | 100% |
| User perception | Steps with perception desc / total | 100% |
| Time description | Steps with ⏱ timing / total | 100% |
| Space description | Steps with 📱 layout / total | 100% |
| Exception coverage | Flows with exception branches / total | 100% |
| P0 issues | Cannot implement | 0 |
| P1 issues | Might implement wrong | 0 |
| ID consistency | All IDs unique, references valid | 100% |
| Key requirements | Flows/steps with 关键要求 / total | 100% |
| Acceptance criteria | Flows/steps with 验收标准 / total | 100% |

## Anti-Patterns

1. **Fixing before review** — Always review first
2. **Single-perspective review** — All three proxies must run
3. **Running User/Dev sequentially** — They should run in parallel
4. **Skipping Integrator merge** — Integrator must merge all three proxies' findings, deduplicate, and prioritize
5. **User Proxy as spec reader** — Must simulate being a user
6. **Dev Proxy as reviewer** — Must simulate trying to implement
7. **Integrator as mere combiner** — Has own perspective (global consistency)
8. **Skipping inner loop** — Don't stop until P0=0 AND P1=0
9. **Fixing issues without prioritization** — Always process P0 first, then P1
10. **Skipping re-check** — Every fix needs verification
11. **Renumbering IDs** — IDs are permanent once assigned
12. **Dangling references** — Every → FL-xxx must point to existing definition
13. **Assuming "obvious"** — If not written, it doesn't exist
14. **Happy path only** — Every flow must cover error/edge/exception branches
15. **Returning subagent output to main session** — Results go to files
16. **Main session doing real work** — Main session orchestrates only

## Trigger Phrases

- "流程细化" / "flow detailing"
- "流程补全" / "流程断裂"
- "迭代流程" / "iterative flow"
- "流程评审" / "flow review"

## References

所有模板和提示词已抽取到 `references/` 目录，正文中通过引用链接指向。

| 文件 | 用途 | 正文引用位置 |
|------|------|-------------|
| [L1-main-flow-template.md](references/L1-main-flow-template.md) | L1 主流程文档模板 | L1 主流程模板节 |
| [sub-flow-template.md](references/sub-flow-template.md) | L2+ 子流程文档模板（含时间/空间要点） | Flow Document Template 节 |
| [time-space-guidelines.md](references/time-space-guidelines.md) | 时间/空间描述要点指引 | 子流程模板内引用 |
| [prd-gaps-template.md](references/prd-gaps-template.md) | PRD 缺失清单模板 | PRD缺失清单节 |
| [checklist-template.md](references/checklist-template.md) | 流程补全检查清单模板 | 流程补全检查清单节 |
| [three-proxy-review.md](references/three-proxy-review.md) | 三代理评审详细指引（代理定义、探索维度、执行模式） | Step 1A 三代理评审 |
| [four-axis-cross-review.md](references/four-axis-cross-review.md) | 四轴交叉审查详细指引（四轴定义、审查清单、决策日志格式） | Step 1B 四轴交叉审查 |
| [user-proxy-prompt.md](references/user-proxy-prompt.md) | User Proxy 评审提示词 | 三代理评审指引内引用 |
| [dev-proxy-prompt.md](references/dev-proxy-prompt.md) | Dev Proxy 评审提示词 | 三代理评审指引内引用 |
| [integrator-prompt.md](references/integrator-prompt.md) | Integrator 评审提示词 | 三代理评审指引内引用 |
| [multi-perspective-review-design.md](docs/multi-perspective-review-design.md) | 四轴交叉审查设计文档（问题清单、Prompt模板、决策日志格式） | 四轴交叉审查指引内引用 |
