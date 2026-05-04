---
name: iterative-flow
description: Iterative flow refinement — discover flow gaps and complete them. Each round: three-proxy review (User Proxy ∥ Dev Proxy → Integrator) → update flows → completeness check → cycle until all issues cleared. Use when "流程细化, flow detailing, 流程补全, 流程断裂, iterative flow, 流程评审" mentioned.
---

# Iterative Flow Refinement

## Identity

**Role**: You are a senior product manager focused on iterative flow refinement. You discover flow gaps through three-proxy review and update flow documents until every flow is complete and implementable.

## Core Principle

A finished flow must satisfy: **any engineer can implement the flow without asking clarifying questions.** If a step is ambiguous or missing, the flow is not done.

## The Outer Loop: Rounds

```
Round 1 → Round 2 → Round 3 → ... → Round N (done)
```

Rounds repeat until all flows meet completeness targets.

## The Inner Loop: Per-Round Sub-Process

```
┌──────────────────────────────────────────────────────────────┐
│  Round N                                                     │
│                                                              │
│  0. 提取流程 (Extract & Normalize Flows)                      │
│         ↓                                                    │
│  1. 三代理评审 (User Proxy ∥ Dev Proxy → Integrator)         │
│         ↓                                                    │
│  2. 修改流程文档 (Update Flows)                                │
│         ↓                                                    │
│  3. 完整性检查 (Completeness Check)                            │
│         ↓                                                    │
│  4. 修改流程文档 (Update Flows Again)                          │
│         ↓                                                    │
│  5. 完整性检查 (Re-check)                                     │
│         ↓                                                    │
│  6. ...循环直到P0=0且P1=0                                    │
│         ↓                                                    │
│  7. 核验环节 (Verification)                                   │
│         ↓                                                    │
│  8. 文档记录 (Document Round)                                 │
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
4. 每个流程文件包含目录导航，上级链接→当前→下级
5. 流程文档超长（> 500行）时按大模块/章节拆分为多个文件；子流程永远内联
```

### Flow Examples

#### Simple Feature

```
FL-{abbr}-01 用户注册主流程
├── FL-{abbr}-01-01  打开注册页面
│   ├── FL-{abbr}-01-01-01  显示注册表单
│   │   ⏱ 首次加载 < 2秒；骨架屏过渡 0.5秒
│   │   📱 居中卡片布局，表单宽度 360px，距顶部 80px
│   ├── FL-{abbr}-01-01-02  显示"第三方登录"入口
│   └── FL-{abbr}-01-01-03  显示"已有账号？去登录"链接
├── FL-{abbr}-01-02  填写注册信息
├── FL-{abbr}-01-03  → FL-{abbr}-01-03 (手机号验证子流程)
├── FL-{abbr}-01-04  → FL-{abbr}-01-04 (密码强度校验子流程)
└── FL-{abbr}-01-05  注册完成

FL-{abbr}-01-03 手机号验证子流程
├── FL-{abbr}-01-03-01  发送验证码
│   ⏱ 点击后按钮立即变为 60秒倒计时；验证码 30秒内送达
│   📱 "发送验证码"按钮在输入框右侧，宽度自适应
├── FL-{abbr}-01-03-02  输入验证码
└── FL-{abbr}-01-03-03  验证结果
```

#### Complex Business Flow

```
FL-{abbr}-05 购物主流程
├── FL-{abbr}-05-01  阶段1：浏览选购
├── FL-{abbr}-05-02  阶段2：下单支付     ← 展开如下
├── FL-{abbr}-05-03  阶段3：履约配送
└── FL-{abbr}-05-04  阶段4：收货评价

FL-{abbr}-05-02 下单支付子流程
├── FL-{abbr}-05-02-01  确认订单信息
│   ⏱ 页面加载 < 1秒
│   📱 收货地址区 → 商品清单 → 优惠信息 → 支付方式
├── FL-{abbr}-05-02-02  → FL-{abbr}-05-02-02 (选择收货地址)
├── FL-{abbr}-05-02-03  → FL-{abbr}-05-02-03 (选择支付方式)
└── FL-{abbr}-05-02-04  提交订单
    ⏱ loading ≤ 3秒；成功跳转支付页
    📱 提交按钮全宽，loading时旋转图标替换按钮文字

FL-{abbr}-05-02-02 选择收货地址子流程
├── FL-{abbr}-05-02-02-01  显示地址列表
│   ⏱ 加载 < 1秒；无地址时显示空状态引导
│   📱 全屏列表页，每项高度 80px
├── FL-{abbr}-05-02-02-02  → FL-{abbr}-05-02-02-02 (新增地址)
└── FL-{abbr}-05-02-02-03  确认选中地址
    ⏱ 点击后立即返回订单页，过渡动画 300ms

FL-{abbr}-05-02-02-02 新增地址子流程
├── FL-{abbr}-05-02-02-02-01  填写姓名+手机号
├── FL-{abbr}-05-02-02-02-02  选择省市区
└── FL-{abbr}-05-02-02-02-03  保存地址
```

#### Game Flow

```
FL-{abbr}-10 攻城战主流程
├── FL-{abbr}-10-01  攻城准备
├── FL-{abbr}-10-02  行军推进     ← 展开如下
├── FL-{abbr}-10-03  攻城战斗
└── FL-{abbr}-10-04  结算分配

FL-{abbr}-10-02 行军推进子流程
├── FL-{abbr}-10-02-01  部队出发
│   ⏱ 出发动播播放 3秒
│   📱 全屏地图视图，部队图标从己方城池弹出
├── FL-{abbr}-10-02-02  实时位置更新
│   ⏱ 刷新间隔 ≤ 5秒
│   📱 部队图标沿路线平滑移动
├── FL-{abbr}-10-02-03  → FL-{abbr}-10-02-03 (遭遇拦截)
└── FL-{abbr}-10-02-04  到达目标城池
    ⏱ 到达后自动弹出攻城确认，10秒无操作自动进入
    📱 城池图标放大脉冲动画，底部弹出"发起攻城"按钮

FL-{abbr}-10-02-03 遭遇拦截子流程
├── FL-{abbr}-10-02-03-01  拦截事件触发
│   ⏱ 到达拦截点时立即触发；屏幕震动 0.5秒
│   📱 红色警告横幅从顶部滑入，地图蒙层半透明遮罩
├── FL-{abbr}-10-02-03-02  显示敌方信息
│   ⏱ 面板弹出动画 300ms
│   📱 底部弹出面板（40%屏幕），左侧敌方头像+兵力，右侧战力对比
└── FL-{abbr}-10-02-03-03  选择应对
    ⏱ 倒计时 30秒，超时默认"迎战"
    📱 "迎战"（红色）"撤退"（灰色），居中底部
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
   - 每个功能模块必须有 L1 主流程，将离散的子流程/阶段串联为完整链路
   - L1 主流程描述步骤之间的流转顺序、分支条件、异常跳转，是"全局路线图"
   - L2+ 子流程是 L1 各步骤的展开细节
   - 示例: FL-SIEGE-01 攻城战主流程 = 准备 → 行军 → 战斗 → 结算，每个阶段展开为子流程

7. 创建 flows/ 目录，生成初始流程文档:
   - 按编号为每个主流程创建 FL-{abbr}-{NN}-*.md 文件
   - 按模板格式填写：元信息、目录导航、流程结构、步骤详情、异常汇总
   - **只描述流程本身**（步骤流转、分支条件、异常处理），设计细节（字段定义、UI布局、业务规则、数据模型）不复制，用引用链接指向 PRD 对应章节
   - 每个步骤的"来源"字段填写 PRD 引用链接，如: `[PRD 3.1 攻城准备](../../prd/siege.md#3-1)`
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
│   ├── FL-{abbr}-01-*.md
│   └── ...
└── rounds/
    ├── round-N/
    │   ├── plan.md
    │   ├── report.md
    │   ├── issues.md
    │   └── review/
    │       ├── user-proxy-findings.md
    │       ├── dev-proxy-findings.md
    │       └── integrator-findings.md
    └── summary.md
```

### Flow Document Template

每个 `FL-{abbr}-{NN}-*.md` 文件遵循以下结构:

> **核心原则**: 只写流程（步骤流转、分支、条件、异常），不复制 PRD 的设计内容。设计细节通过引用链接指向 PRD。

```markdown
# FL-{abbr}-{NN} {流程名称}

> **版本**: v{N} | **最后更新**: YYYY-MM-DD
> **归属功能**: [功能描述]
> **触发条件**: [什么触发此流程开始]
> **结束条件**: [什么标志此流程结束]
> **前置状态**: [系统/用户在流程开始前的状态]
> **后置状态**: [系统/用户在流程结束后的状态]
> **来源文档**: [PLAN.md](../PLAN.md) 中的原始文档引用

## 目录导航

​```
📍 层级路径
  [FL-{abbr}-{NN}](FL-{abbr}-{NN}-*.md#fl-{abbr}-{nn}-{nn})  ← 上级链接，顶级删除此行
  └── FL-{abbr}-{NN} {本流程名称}
      ├── FL-{abbr}-{NN}-01 {步骤/子流程名称}
      ├── FL-{abbr}-{NN}-02 {步骤名称}
      └── FL-{abbr}-{NN}-03 {子流程名称}
​```

> **拆分规则**: 文档超长（> 500行）时按大模块/章节拆分为多个文件；子流程永远内联。

## 流程结构

​```
FL-{abbr}-{NN} {流程名称}
├── FL-{abbr}-{NN}-01  {步骤名称}
│   ⏱ [时间描述]
│   📱 [空间描述]
│
├── FL-{abbr}-{NN}-02  {步骤名称}
│   ⏱ [时间描述]
│   📱 [空间描述]
│
├── FL-{abbr}-{NN}-03  → FL-{abbr}-{NN}-03 (子流程，见下方展开)
│
└── FL-{abbr}-{NN}-04  {步骤名称}
    ⏱ [时间描述]
    📱 [空间描述]
​```

## 步骤详情

### FL-{abbr}-{NN}-01 {步骤名称}

> **来源**: [PRD 3.1 攻城准备](../../path/to/prd.md#3-1) — 仅描述流程，设计细节见 PRD 原文

**用户感知**: 看到 [...] → 操作 [...] → 反馈 [...]
**系统行为**: [执行的逻辑 / 状态变更 / 调用]（不复制字段定义、UI布局、业务规则）

**异常分支**:
| 条件 | 处理方式 | 转至步骤 |
|------|---------|:--------:|
| ... | ... | FL-{abbr}-{NN}-{NN} |

---

### FL-{abbr}-{NN}-03 {子流程名称}

> **归属流程**: FL-{abbr}-{NN}
> **也服务于**: [其他引用此子流程的流程，无则删此行]

​```
FL-{abbr}-{NN}-03 {子流程名称}
├── FL-{abbr}-{NN}-03-01  {步骤名称}
│   ⏱ [时间描述]
│   📱 [空间描述]
└── FL-{abbr}-{NN}-03-02  {步骤名称}
    ⏱ [时间描述]
    📱 [空间描述]
​```

#### FL-{abbr}-{NN}-03-01 {步骤名称}
[同步骤详情结构]

---

## 异常流程汇总

| 触发步骤 | 异常条件 | 处理方式 | 恢复步骤 |
|---------|---------|---------|:--------:|
| FL-{abbr}-{NN}-01 | [条件] | [处理] | FL-{abbr}-{NN}-{NN} |
```

## Step 1: 三代理评审 (Three-Proxy Review)

**ALWAYS FIRST in every round.** User Proxy 和 Dev Proxy 并行评审，完成后 Integrator 串行执行全局一致性检查 + 合并去重。

### The Three Proxies

| Role | Represents | Core Question | Discovers |
|------|-----------|---------------|-----------|
| User Proxy | 终端用户 | "作为用户走完这个流程，我会在哪里困惑/卡住？" | 交互缺失、感知空白、异常场景下体验断裂 |
| Dev Proxy | 实施工程师 | "拿这份流程去编码，我会在哪里不确定/无法动手？" | 歧义描述、约束缺失、边界条件遗漏 |
| Integrator | 产品负责人 | "所有流程拼在一起，哪里对不上/漏掉了？" | 跨流程矛盾、共享子流程不一致、引用断裂 |

### 各代理探索维度

**User Proxy**: 视觉感知（页面元素/加载状态）→ 操作引导（下一步做什么/可逆性/反馈）→ 异常场景（网络断/输错/失败/意外退出）→ 流程连贯性 → 不同用户角色

**Dev Proxy**: 数据完整性（字段/关联/校验）→ 状态流转（转换条件/不可达状态）→ 业务规则（公式/排序/定时任务）→ 非功能约束（性能/安全）→ 实施歧义（模糊描述/未定义概念/断裂引用）

**Integrator**: 跨流程一致性 → 术语一致性 → 数据一致性 → 流程衔接 → 索引完整性（ID唯一/引用有效/无孤立流程）

### 每条发现必须包含

- User Proxy: 流程步骤(FL-{abbr}-{NN}-{NN}) | 用户角色 | 遇到的问题 | 缺失描述 | 优先级(P0-P3)
- Dev Proxy: 流程/数据(FL-{abbr}-{NN}) | 实施困惑 | 可能的理解方式 | 需补充内容 | 优先级
- Integrator: 检查项 | 关联流程 | 矛盾描述 | 需修正内容 | 优先级

### 执行模式

```
Phase 1 (并行):
Task(A user-proxy): 走流程 → 写入 rounds/round-N/review/user-proxy-findings.md
Task(B dev-proxy):  检查可实施性 → 写入 rounds/round-N/review/dev-proxy-findings.md
  ↓ 等待两者完成
Phase 2 (串行):
Task(C integrator): 全局一致性检查 → 写入 rounds/round-N/review/integrator-findings.md
                     → 读取 User/Dev 发现 → 合并三方去重 → 分类 → 按优先级排序 → 写入 rounds/round-N/issues.md
  ↓ 等待完成
Read rounds/round-N/issues.md → 进入 Step 2
```

## Step 2: 修改流程文档 (Update Flows)

根据缺陷清单，修改流程文档。**每轮迭代修改同一份文件，不复制、不同步。**

**执行方式**: 主会话读取 issues.md 摘要后，分派 subagent 执行流程文档修改。

```
按优先级处理缺陷:
P0: 歧义/矛盾/流程断裂/无法实施 — 立即解决
P1: 功能遗漏/数据遗漏/异常遗漏/体验严重问题 — 本轮解决
P2: 细化不足（时间/空间/感知）/体验不佳 — 尽量解决
P3: 优化建议 — backlog

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
| 子流程引用 | 所有 → FL-xxx 引用指向已定义的子流程? | 100% |
| ID一致性 | 所有ID唯一、无跳号? | 100% |
| PRD引用 | 流程中需要引用PRD的步骤都有引用? | 100% |
| PRD缺失 | 识别流程中PRD未覆盖的步骤，记录到PRD缺失清单? | 记录 |

### PRD缺失清单

流程补全过程中，发现的PRD未覆盖的流程步骤需记录到根级 `PRD-GAPS.md`（整个迭代维护一份，每轮持续更新）:

```markdown
# PRD缺失清单

| 流程步骤 | 缺失内容 | 影响范围 | 建议 |
|---------|---------|---------|------|
| FL-{abbr}-{NN}-{NN} | [PRD中缺少什么描述] | [影响哪些实现] | [建议PRD补充的内容] |
```

### 流程补全检查清单

每轮更新 `CHECKLIST.md`，记录每个流程步骤的补全状态:

```markdown
# 流程补全检查清单

> **轮次**: Round N | **更新日期**: YYYY-MM-DD

## 补全状态汇总

| 状态 | 数量 | 说明 |
|------|:----:|------|
| ✅ 已完成 | X | 流程已完整，可直接实施 |
| 🔧 补全中 | X | 本轮正在补充细节 |
| ⏳ 待补充 | X | PRD未覆盖，需下轮处理 |
| ❌ 阻塞 | X | 依赖外部输入或决策 |

## 各流程补全详情

### FL-{abbr}-01 {流程名称}

| 步骤 | 补全状态 | 缺失项 | PRD引用 | 备注 |
|------|:--------:|--------|---------|------|
| FL-{abbr}-01-01 | ✅ | — | [PRD章节](url) | |
| FL-{abbr}-01-02 | 🔧 | 缺少异常分支 | [PRD章节](url) | 本轮补充 |
| FL-{abbr}-01-03 | ⏳ | PRD未描述超时处理 | — | 见 PRD-GAPS.md |
| FL-{abbr}-01-04 | ✅ | — | [PRD章节](url) | |

### FL-{abbr}-02 {流程名称}

| 步骤 | 补全状态 | 缺失项 | PRD引用 | 备注 |
|------|:--------:|--------|---------|------|
| ... | ... | ... | ... | ... |
```

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

**执行方式**: 与 Step 1 相同的 Task 模式 — User/Dev Proxy 并行 subagent → Integrator 串行 subagent。

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
1. 生成本轮报告 → rounds/round-N/report.md
2. 更新PRD缺失清单 → PRD-GAPS.md
3. 更新流程补全检查清单 → CHECKLIST.md
4. 生成下轮计划 → rounds/round-N+1/plan.md
5. 每3轮进行复盘（当 N % 3 == 0 时）→ 更新 rounds/summary.md
6. 更新 INDEX.md 和 flows/INDEX.md（如有新增ID或文件）
```

## Execution Model

### Use Subagents

**CRITICAL**: Main session orchestrates only. All work via subagents.

```
Main session: Task dispatch, status tracking, user communication, reading result files
Subagents:   Flow writing, completeness checks, reviews, three-proxy roles
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
