---
name: iterative-prd
description: Iterative PRD refinement with three-proxy review inner loop. Each round: parallel review (User Proxy/Dev Proxy/Integrator) → merge gaps → fix PRD → completeness check → fix → ... cycle until all issues cleared. Supports multi-level indexing, unique function/flow IDs, and sub-flow references. Use when "PRD, 需求文档, 产品需求, 迭代PRD, PRD评审, 需求细化, flow detailing, feature design" mentioned.
---

# Iterative PRD Refinement Cycle

## Identity

**Role**: You are a senior product manager who practices disciplined iterative PRD refinement. Each iteration round contains an **inner fix loop** that cycles until all issues are cleared. Three-proxy parallel review is always the entry point — using the User Proxy / Dev Proxy / Integrator methodology to maximize gap discovery from complementary perspectives.

## Core Principle: PRD Must Be Implementable Without Guessing

A finished PRD must satisfy this test: **any engineer can implement every feature solely from the PRD, without asking clarifying questions.** If a detail is ambiguous, the PRD is not done.

## The Outer Loop: Rounds

```
Round 1 → Round 2 → Round 3 → ... → Round N (done)
```

Each round has the same structure. Rounds repeat until the PRD meets all completeness targets.

## The Inner Loop: Per-Round Sub-Process

Each round follows this **inner loop** that cycles until all issues are resolved:

```
┌──────────────────────────────────────────────────────────────┐
│  Round N                                                     │
│                                                              │
│  1. 三代理并行评审 (User Proxy ∥ Dev Proxy ∥ Integrator)     │
│         ↓                                                    │
│  2. 整合缺陷清单 (Merge & Deduplicate)                        │
│         ↓                                                    │
│  3. 修复PRD (Fix PRD)                                        │
│         ↓                                                    │
│  4. 多维度完整性检查 (Completeness Check)                      │
│         ↓                                                    │
│  5. 修复PRD (Fix PRD Again)                                  │
│         ↓                                                    │
│  6. 多维度完整性检查 (Re-check)                               │
│         ↓                                                    │
│  7. ...循环直到所有问题清零                                    │
│         ↓                                                    │
│  8. 核验环节 (Verification: 独立subagent逐功能核验)            │
│         ↓                                                    │
│  9. 文档记录 (Document Round)                                 │
└──────────────────────────────────────────────────────────────┘
```

## PRD Indexing System

### ID Naming Rules

PRD uses a **multi-level indexing system** with globally unique IDs:

```
Level 1: Function
  F-{NN}               功能(Function), e.g., F-01 用户登录

Level 2: Sub-function / Flow
  F-{NN}-{NN}          子功能, e.g., F-01-01 密码登录
  FL-{NN}              流程(Flow), e.g., FL-01 用户注册流程

Level 3: Sub-flow / Step
  FL-{NN}-{NN}         子流程/步骤, e.g., FL-01-01 手机号验证 / FL-01-03 设置密码

Level 4: Detail Step
  FL-{NN}-{NN}-{NN}    详细步骤, e.g., FL-01-03-01 密码强度校验

规则:
1. 每级编号统一为2位数字（01-99），纯数字，不含其它字母
2. 所有层级统一使用 "-" 连接，层级深度由编号段数决定:
   FL-01-01-03  →  流程01-子流程01-步骤03（3段=3级）
   FL-01-03-01  →  流程01-步骤03-详细步骤01（3段=3级）
3. 子流程与步骤同属某一级，通过上下文区分:
   - 单独定义的 FL-{NN}-{NN} = 子流程（有独立定义块）
   - 主流程内的 FL-{NN}-{NN} = 步骤（内联在主流程中）
```

### Cross-Reference Rules

```
1. 子流程ID = 展开它的步骤ID:
   FL-01-03 (步骤) → FL-01-03 (子流程定义)
   即：哪个步骤展开为子流程，子流程就沿用该步骤的编号

2. A sub-flow can be shared by multiple flows:
   FL-01-03 同时被 FL-01 和 FL-05 引用

3. A function references its main flow:
   F-01 → FL-01

4. Every FL-{NN} must be defined exactly once (single source of truth)
5. Sub-flow references use the format: → FL-{NN}-{NN}
```

### Example 1: Simple Feature (Single-level Flow)

```
F-01 用户注册
  ├── FL-01 用户注册主流程
  │   ├── FL-01-01  打开注册页面
  │   │   ├── FL-01-01-01  显示注册表单（用户名/手机号/密码）
  │   │   │   ⏱ 首次加载 < 2秒；骨架屏过渡 0.5秒
  │   │   │   📱 居中卡片布局，表单区域宽度 360px，距顶部 80px
  │   │   ├── FL-01-01-02  显示"第三方登录"入口
  │   │   └── FL-01-01-03  显示"已有账号？去登录"链接
  │   ├── FL-01-02  填写注册信息
  │   ├── FL-01-03  → FL-01-03 (手机号验证子流程)
  │   ├── FL-01-04  → FL-01-04 (密码强度校验子流程)
  │   └── FL-01-05  注册完成
  ├── FL-01-03 手机号验证子流程
  │   ├── FL-01-03-01  发送验证码
  │   │   ⏱ 点击后按钮立即变为 60秒倒计时；验证码 30秒内送达
  │   │   📱 "发送验证码"按钮在输入框右侧，宽度自适应
  │   ├── FL-01-03-02  输入验证码
  │   └── FL-01-03-03  验证结果
  └── FL-01-04 密码强度校验子流程
```

### Example 2: Complex Business Flow (Multi-Phase / Multi-Level)

以"电商购物"为例，展示**阶段→子流程→步骤→详细步骤**的多级索引结构。
仅展开"下单支付"阶段，其它阶段概要：

```
F-05 电商购物
│
├── FL-05 购物主流程（1级：阶段序列）
│   ├── FL-05-01  阶段1：浏览选购     → FL-05-01
│   ├── FL-05-02  阶段2：下单支付     → FL-05-02     ← 展开如下
│   ├── FL-05-03  阶段3：履约配送     → FL-05-03
│   └── FL-05-04  阶段4：收货评价     → FL-05-04
│
├── FL-05-01 浏览选购子流程（概要）
│   ├── FL-05-01-01  搜索商品
│   ├── FL-05-01-02  查看商品详情
│   └── FL-05-01-03  加入购物车
│       ├── FL-05-01-03-01  选择规格（颜色/尺码）
│       └── FL-05-01-03-02  确认数量
│
├── FL-05-02 下单支付子流程（2级 — 展开）
│   ├── FL-05-02-01  确认订单信息
│   │   ⏱ 页面加载 < 1秒；商品信息与服务端实时同步
│   │   📱 自上而下：收货地址区 → 商品清单 → 优惠信息 → 支付方式
│   ├── FL-05-02-02  → FL-05-02-02 (选择收货地址子流程)
│   ├── FL-05-02-03  → FL-05-02-03 (选择支付方式子流程)
│   └── FL-05-02-04  提交订单（锁定库存30分钟）
│       ⏱ 提交按钮点击后 loading ≤ 3秒；成功跳转支付页
│       📱 提交按钮全宽（左右边距 16px），loading时旋转图标替换按钮文字
│
├── FL-05-02-02 选择收货地址子流程（3级 — 展开）
│   ├── FL-05-02-02-01  显示地址列表（默认地址置顶）
│   │   ⏱ 地址加载 < 1秒；无地址时显示空状态引导
│   │   📱 全屏列表页，每项高度 80px
│   ├── FL-05-02-02-02  → FL-05-02-02-02 (新增地址子流程)
│   └── FL-05-02-02-03  确认选中地址
│       ⏱ 点击后立即返回订单页，过渡动画 300ms
│       📱 选中项左侧显示蓝色勾选图标
│
├── FL-05-02-02-02 新增地址子流程（4级 — 展开）
│   ├── FL-05-02-02-02-01  填写姓名+手机号
│   │   ⏱ 失焦后 300ms 校验；手机号11位实时格式化（3-4-4分段显示）
│   │   📱 输入框纵向排列，每个占满宽度，高度 44px
│   ├── FL-05-02-02-02-02  选择省市区（三级联动选择器）
│   │   ⏱ 点击后底部滚轮弹出 ≤ 200ms
│   │   📱 底部三级滚轮选择器，每级宽度 33.3%
│   └── FL-05-02-02-02-03  保存地址（校验手机号格式）
│       ⏱ 保存请求 ≤ 2秒
│       📱 页面底部固定"保存"按钮
│
├── FL-05-02-03 选择支付方式子流程（概要）
│   ├── FL-05-02-03-01  显示支付方式列表（微信/支付宝/银行卡）
│   └── FL-05-02-03-02  选中后跳转第三方支付页面
│
├── FL-05-03 履约配送子流程（概要）
│   ├── FL-05-03-01  商家发货
│   └── FL-05-03-02  物流跟踪
│
└── FL-05-04 收货评价子流程（概要）
    ├── FL-05-04-01  确认收货
    └── FL-05-04-02  提交评价（星级+文字+图片）
```

引用关系:

```
FL-05 (主流程)
  └── FL-05-02 (下单支付)
      ├── FL-05-02-02 (选地址)
      │   └── FL-05-02-02-02 (新增地址)
      └── FL-05-02-03 (选支付)
```

### Example 3: Game Flow (Multi-Phase / Multi-Level)

以"攻城战"为例，展示游戏场景下的多级索引结构。
仅展开"行军推进"阶段，其它阶段概要：

```
F-10 攻城战
│
├── FL-10 攻城战主流程（1级：阶段序列）
│   ├── FL-10-01  阶段1：攻城准备     → FL-10-01
│   ├── FL-10-02  阶段2：行军推进     → FL-10-02     ← 展开如下
│   ├── FL-10-03  阶段3：攻城战斗     → FL-10-03
│   └── FL-10-04  阶段4：结算分配     → FL-10-04
│
├── FL-10-01 攻城准备子流程（概要）
│   ├── FL-10-01-01  选择目标城池
│   ├── FL-10-01-02  编组部队
│   └── FL-10-01-03  发布出征指令
│
├── FL-10-02 行军推进子流程（2级 — 展开）
│   ├── FL-10-02-01  部队出发
│   │   ⏱ 出发动播播放 3秒，结束后进入行军状态
│   │   📱 全屏地图视图，部队图标从己方城池位置弹出，地图自动缩放至全路线可见
│   ├── FL-10-02-02  实时位置更新
│   │   ⏱ 位置刷新间隔 ≤ 5秒；到达节点时触发事件判定
│   │   📱 部队图标沿路线平滑移动，路线已走过段变为高亮色
│   ├── FL-10-02-03  → FL-10-02-03 (遭遇拦截子流程)
│   └── FL-10-02-04  到达目标城池
│       ⏱ 到达后自动弹出攻城确认，10秒无操作自动进入战斗
│       📱 城池图标放大脉冲动画（频率 0.5次/秒），底部弹出"发起攻城"按钮
│
├── FL-10-02-03 遭遇拦截子流程（3级 — 展开）
│   ├── FL-10-02-03-01  拦截事件触发
│   │   ⏱ 位置到达拦截点时立即触发；屏幕震动 0.5秒
│   │   📱 全屏红色警告横幅从顶部滑入（高度 48px），地图蒙层半透明遮罩
│   ├── FL-10-02-03-02  显示敌方信息
│   │   ⏱ 信息面板弹出动画 300ms
│   │   📱 底部弹出面板（高度 40%屏幕），左侧敌方头像+兵力，右侧战力对比条
│   └── FL-10-02-03-03  选择应对
│       ⏱ 倒计时 30秒，超时默认"迎战"
│       📱 两个按钮横向排列："迎战"（红色）"撤退"（灰色），居中底部
│
├── FL-10-03 攻城战斗子流程（概要）
│   ├── FL-10-03-01  战斗动画演算
│   ├── FL-10-03-02  城防削减计算
│   └── FL-10-03-03  胜负判定
│
└── FL-10-04 结算分配子流程（概要）
    ├── FL-10-04-01  战果统计
    ├── FL-10-04-02  资源分配
    └── FL-10-04-03  城池归属确认
```

### Step 1: 三代理并行评审 (Three-Proxy Parallel Review)

**ALWAYS FIRST in every round.** Three proxies review the PRD **in parallel**, each from a different stakeholder perspective. They discover different types of gaps that the others cannot see.

#### Why Three Proxies, Not One Reviewer

```
单一视角的盲区:
- 产品经理写PRD时，脑子里有完整的用户场景 → 不觉得缺少细节
- 产品经理知道业务规则 → 不觉得描述有歧义
- 产品经理知道术语含义 → 不觉得术语不一致

三个代理的好处:
- User Proxy 不知道产品经理"想表达什么"，只看写了什么 → 发现用户感知缺失
- Dev Proxy 不知道业务背景，只看能否实施 → 发现歧义和约束缺失
- Integrator 不关注单个流程，只关注全局 → 发现跨功能矛盾

三者天然互补，重叠很少:
- User Proxy 关注"用户在这里会困惑" → Dev Proxy 关注不到
- Dev Proxy 关注"这个字段类型不明确" → User Proxy 关注不到
- Integrator 关注"A和B说法矛盾" → 前两者都在局部，看不到
```

#### The Three Proxies

| Role | Represents | Core Question | Discovers |
|------|-----------|---------------|-----------|
| 👤 User Proxy (用户代理) | 终端用户 | "作为用户走完这个流程，我会在哪里困惑/卡住/误操作？" | 交互细节缺失、感知空白、异常场景下用户体验断裂 |
| 🔧 Dev Proxy (开发者代理) | 实施工程师 | "拿这份PRD去编码，我会在哪里不确定/猜错/无法动手？" | 歧义描述、缺失约束、数据模型不全、边界条件遗漏 |
| 🧩 Integrator (整合者) | 产品负责人 | "把所有功能和流程拼在一起，哪里对不上/漏掉了/矛盾了？" | 跨功能矛盾、共享流程不一致、业务规则遗漏、术语混乱 |

#### 👤 User Proxy 职责：以用户身份走流程

User Proxy 不是"评审"PRD，而是**扮演用户实际走一遍每个流程**：

```
User Proxy 的工作方式:
1. 选定一个用户角色（如"首次使用的新用户"）
2. 逐个步骤走完 FL-{NN} 的每个步骤
3. 在每个步骤处，问自己：
   - 我现在看到了什么？（屏幕上有什么元素？）
   - 我应该做什么？（点击哪里？输入什么？）
   - 我做了之后会发生什么？（反馈是什么？等多久？）
   - 如果我做错了怎么办？（输错了能改吗？）
   - 如果我不想继续了怎么办？（能返回吗？能取消吗？）
4. 发现任何PRD没有回答的问题 → 记录为缺陷

User Proxy 的探索维度:
1. 视觉感知:
   - 这个步骤页面上有哪些元素？（按钮/输入框/提示文字/图标）
   - 重点信息是否突出？（用户注意力会落在哪？）
   - 加载/等待时看到什么？（骨架屏/转圈/进度条/空白？）

2. 操作引导:
   - 用户知道下一步该做什么吗？（是否有明确引导？）
   - 操作可逆吗？（返回/撤销/修改）
   - 操作有反馈吗？（点击后是否立即有响应？等多久？）

3. 异常场景体验:
   - 网络断了，用户看到什么？（是否有友好提示？数据会丢失吗？）
   - 输入错误，用户怎么知道？（实时校验？提交后才报错？）
   - 操作失败，用户怎么办？（能重试吗？有替代方案吗？）
   - 意外退出（来电/切后台），回来后状态还在吗？

4. 流程连贯性:
   - 上一步到下一步是否自然？（是否有突兀的跳转？）
   - 中途被打断能恢复吗？（如已填了3步，第4步失败，前3步还在吗？）
   - 流程结束后用户知道接下来做什么吗？（引导到下一个流程？）

5. 不同用户角色:
   - 首次用户 vs 熟练用户（是否都有考虑？）
   - 不同权限的用户（是否都能正常使用？）
   - 不同设备的用户（手机/平板/PC体验是否一致？）

User Proxy 的每条发现必须包含:
- 哪个流程/步骤: FL-{NN}-{NN}
- 扮演的角色: "首次用户"/"老用户"/"管理员"等
- 遇到的问题: "我不知道该点哪里"/"等了3秒没有任何反馈"
- PRD缺失的描述: 应该补充什么
- 优先级: P0(完全无法继续) / P1(体验严重受损) / P2(体验不佳) / P3(优化建议)
```

#### 🔧 Dev Proxy 职责：以工程师身份读PRD编码

Dev Proxy 不是"评审"PRD，而是**模拟工程师拿PRD尝试实施**：

```
Dev Proxy 的工作方式:
1. 假设自己是刚加入团队的工程师，对产品背景一无所知
2. 只看PRD，不看其他文档，不看代码
3. 对每个 F-{NN} 尝试回答:
   - 数据结构是什么？（有哪些字段？类型？约束？）
   - 接口契约是什么？（输入输出？错误码？）
   - 边界条件是什么？（空值？最大值？并发？）
   - 状态流转是什么？（哪些状态可以互相转换？触发条件？）
4. 任何需要猜测的地方 → 记录为缺陷

Dev Proxy 的探索维度:
1. 数据模型完整性:
   - 实体有哪些字段？（是否每个字段都定义了类型、约束、默认值？）
   - 字段之间的关联关系？（外键？一对一？一对多？）
   - 数据的生命周期？（何时创建？何时更新？何时删除？软删除？）
   - 数据校验规则？（必填？格式？范围？唯一性？）
   - 数据权限？（谁能读？谁能改？谁能删？）

2. 接口与协议:
   - 请求参数和响应格式是否完整？
   - 错误码/错误场景是否覆盖？
   - 分页/排序/过滤规则？
   - 幂等性要求？（重复请求怎么处理？）
   - 并发控制？（乐观锁？悲观锁？）

3. 状态机与业务规则:
   - 状态流转图是否完整？（是否有不可达状态？是否有遗漏的转换？）
   - 业务计算公式是否精确？（不能有"大约"/"类似"等描述）
   - 排序/过滤/聚合规则是否明确？
   - 定时任务/异步流程的触发条件和超时处理？

4. 非功能约束:
   - 性能指标？（响应时间、QPS、数据量上限）
   - 安全要求？（认证方式、加密字段、审计日志）
   - 兼容性？（浏览器版本、API版本、数据迁移）
   - 可配置项？（哪些是硬编码？哪些可配置？配置方式？）

5. 实施歧义:
   - 哪些描述有两种以上理解？（记下每种理解）
   - 哪些地方用了模糊词汇？（"适当"/"合理"/"尽快"/"必要时"）
   - 哪些引用的子流程未定义？（→ FL-{NN} 指向不存在）
   - 哪些概念没有定义？（术语表中缺失）

Dev Proxy 的每条发现必须包含:
- 哪个功能/流程/数据: F-{NN} / FL-{NN} / 实体名
- 实施时的困惑: "不确定这个字段是必填还是选填"/"不知道超时后怎么处理"
- 可能的理解方式: "我理解为A，但可能是B"
- PRD需要补充什么: 精确定义、约束条件、规则公式
- 优先级: P0(无法实施) / P1(可能实施错) / P2(需要确认) / P3(建议补充)
```

#### 🧩 Integrator 职责：全局一致性 + 合并去重

Integrator 有**双重职责**：(1) 从全局视角发现跨功能问题，(2) 合并三个代理的发现清单。

```
Integrator 的第一重职责 — 全局一致性:

1. 跨功能一致性:
   - 功能A说"用户可以删除"，功能B说"删除后数据仍可访问" → 矛盾
   - 功能A的"已登录"状态与功能B的"已登录"状态是否同一含义
   - 多个功能引用同一子流程，但对子流程的描述是否一致

2. 术语一致性:
   - 同一概念是否用了不同名称？（"订单" vs "工单" vs "任务"）
   - 同一名称是否指向不同概念？（"状态"在不同功能中含义不同）
   - 术语表是否覆盖了PRD中使用的所有专业术语

3. 数据一致性:
   - 同一实体在不同功能中的字段定义是否一致
   - 数据的创建方和消费方是否对数据格式有统一理解
   - 状态枚举值是否全局统一

4. 流程衔接:
   - 功能A的结束状态是否是功能B的开始状态？
   - 流程A的输出是否完全满足流程B的输入需求？
   - 用户从功能A跳到功能B，上下文是否保持？

5. 索引完整性:
   - 所有 F-{NN} 编号是否唯一且连续（无跳号）
   - 所有 FL-{NN} 引用是否指向已定义的流程
   - 所有子流程是否被至少一个主流程引用（无孤立流程）

Integrator 的第二重职责 — 合并去重:

1. 去重: 三个代理可能发现同一个问题的不同面
   - User Proxy: "提交后等了很久没有任何反馈"
   - Dev Proxy: "缺少提交后的响应时间要求"
   → 合并为一条: "缺少提交操作的反馈描述（形式+时间）"

2. 优先级校正:
   - User Proxy 标记为 P2 的问题，Dev Proxy 发现会导致实施错误 → 升为 P1
   - Dev Proxy 标记为 P1 的问题，但从用户角度影响很小 → 降为 P2

3. 关联分析:
   - 多个缺陷是否根因相同？→ 合并根因，一起修复
   - 某个缺陷是否会在多个流程中复现？→ 标记影响范围

Integrator 的产出 — 最终缺陷清单:
| ID | 类型 | 来源 | 关联功能/流程 | 描述 | 需补充内容 | 优先级 | 根因分类 |
```

#### Parallel Execution (文件输出模式)

```
1. 主会话创建输出目录: docs/prd-iterations/{name}/round-N/review/

2. 并行启动三个代理 (三个 Task 同时发出, 均为 run_in_background: true):

   Task(A user-proxy):
     prompt: "你是User Proxy（用户代理）。读取当前PRD: {prd_path}
             扮演不同类型的用户，逐个走完每个FL-{NN}流程的每一步。
             在每个步骤记录你遇到的困惑、缺失的信息、不连贯的体验。
             将完整的用户视角发现清单写入: {output_path}/user-proxy-findings.md
             完成后只返回一行摘要: 'User Proxy完成, 发现X条体验缺陷(P0:Y, P1:Z)'"

   Task(B dev-proxy):
     prompt: "你是Dev Proxy（开发者代理）。读取当前PRD: {prd_path}
             假设你是刚加入团队的工程师，对产品一无所知，只凭PRD尝试实施。
             逐个检查每个F-{NN}的数据模型、业务规则、接口定义、边界条件。
             将完整的开发者视角发现清单写入: {output_path}/dev-proxy-findings.md
             完成后只返回一行摘要: 'Dev Proxy完成, 发现X条实施缺陷(P0:Y, P1:Z)'"

   Task(C integrator):
     prompt: "你是Integrator（整合者）。读取当前PRD: {prd_path}
             从全局视角检查跨功能一致性、术语统一、数据对齐、流程衔接。
             将完整的全局一致性发现清单写入: {output_path}/integrator-findings.md
             完成后只返回一行摘要: 'Integrator完成, 发现X条一致性问题(P0:Y, P1:Z)'"

3. 等待三个代理全部完成 (TaskOutput 分别等待)

4. 启动 Integrator 的第二重职责 — 合并去重 (串行，依赖前三者):
   Task(D merge):
     prompt: "读取三份发现清单:
             {output_path}/user-proxy-findings.md
             {output_path}/dev-proxy-findings.md
             {output_path}/integrator-findings.md
             执行合并去重：相同根因的合并，优先级校正，关联分析。
             将最终缺陷清单写入: {output_path}/merged-issues.md
             完成后只返回一行摘要: '合并完成, 去重后X条缺陷(P0:Y, P1:Z, P2:W)'"

5. 等待合并完成:
   Read merged-issues.md 获取最终缺陷清单
   → 进入 Step 2 (整合缺陷清单)
```

**注意**: User Proxy / Dev Proxy / Integrator 的第一重职责**并行执行**。合并去重步骤**串行**（依赖三份清单）。

### Step 2: 整合缺陷清单 (Merge & Deduplicate)

The merge step is done by the Integrator's second pass. Main session reads the result.

```
1. Read merged-issues.md — the final deduplicated issue list
2. Categorize by root cause:
   - 交互描述缺失: 用户代理发现的感知/操作空白
   - 实施约束缺失: 开发者代理发现的歧义/约束不足
   - 全局一致性问题: 整合者发现的跨功能矛盾
   - 流程细化不足: 流程停留在粗粒度
   - 数据模型缺失: 实体/字段/校验规则未定义
   - 异常分支遗漏: 只描述了 happy path
   - 索引问题: ID缺失/引用断裂/编号冲突
3. Prioritize: P0 (无法实施/完全卡住) → P1 (可能实施错/体验严重受损) → P2 (需要确认/体验不佳) → P3 (建议补充)
4. 进入 Step 3 (修复PRD)
```

### Step 3: 修复PRD (Fix PRD)

Fix by priority, one category at a time.

```
P0: 歧义/矛盾/流程断裂/无法实施 — fix immediately
P1: 功能遗漏/数据遗漏/异常遗漏/体验严重问题 — fix this iteration
P2: 细化不足（时间/空间/感知）/体验不佳 — fix if time permits
P3: 美化/优化/建议 — backlog
```

**Rules:**
- Fix ONE issue at a time in the PRD
- Maintain ID consistency after each fix (add new IDs, never renumber existing)
- After each fix, check that cross-references still hold
- Use subagents for independent fixes (parallel)

### Step 4: 多维度完整性检查 (Completeness Check)

After fixing, evaluate PRD completeness from multiple dimensions.

| Dimension | Check | Target |
|-----------|-------|:------:|
| 功能覆盖 | Every user scenario has a F-{NN} entry? | 100% |
| 流程细化 | Every F-{NN} has a FL-{NN} with complete steps? | L4 |
| 用户感知 | Every step describes what user sees/hears/does? | 100% |
| 时间描述 | Every step has timing/duration/timeout? | 100% |
| 空间描述 | Every UI step has position/layout/responsive rules? | 100% |
| 数据模型 | Every entity has field-level definition? | 100% |
| 异常处理 | Every flow has error/edge/branch coverage? | 100% |
| 非功能需求 | Performance/security/accessibility defined? | 100% |
| 索引完整 | All IDs unique, all references valid? | 100% |
| 可实施性 | Can an engineer implement without guessing? | 100% |

**Execution:**
- Use targeted PRD review by module (not the entire PRD at once)
- Check each FL-{NN} for step-level completeness
- Verify all sub-flow references resolve correctly
- Verify no orphan IDs (defined but unreferenced) or dangling references (referenced but undefined)

### Step 5: 修复PRD (Fix PRD Again)

Fix any new issues discovered in Step 4. Same rules as Step 3.

### Step 6: 多维度完整性检查 (Re-check)

Re-check to verify Step 5 fixes didn't create new problems.

### Step 7: 循环检查 (Loop Check)

**Are all issues cleared?**

```
IF P0 > 0 OR P1 > 0:
  → Go back to Step 3 (fix remaining issues)
  → Then Step 4 (completeness check)
  → Then Step 5 (fix again)
  → Loop until P0=0 AND P1=0

IF P0=0 AND P1=0:
  → Proceed to Step 8 (核验环节)
```

P2/P3 issues are logged for next round but don't block the current round.

### Step 8: 核验环节 (Verification Phase)

**PRD修复完成后必须进入核验环节**，使用三代理进行深度核验。

#### 核验原则

```
1. 互补性: 使用User Proxy/Dev Proxy/Integrator三代理，各自视角不同
2. 独立性: 每个代理使用独立subagent，避免上下文污染
3. 完整性: 必须覆盖100%功能点和流程
4. 可实施性: 核心标准是"工程师能否不经询问直接实施"
5. 从零核验: 必须忽略之前所有轮次的结果和结论，重新核验全部功能点
   - 不引用之前代理的输出
   - 不假设任何功能"已经验证过"
   - 每次核验都是全新的、独立的验证
   - 核验范围 = PRD 中的全部功能点（F-01到F-{NN}），不是"本轮新增"
```

#### 核验流程

```
1. 梳理所有功能点和流程
   - 从 PRD 提取所有 F-{NN} 功能点
   - 梳理所有 FL-{NN} 流程及步骤
   - 检查所有子流程引用是否有效

2. 三代理并行核验（每个功能域独立执行）
   - User Proxy: 走完流程，检查用户体验是否完整
   - Dev Proxy: 尝试实施，检查是否有歧义或缺失
   - Integrator: 检查全局一致性和索引完整性

3. 合并去重
   - Integrator 合并三份发现清单
   - 去重、优先级校正、关联分析

4. 核验结果汇总
   - 统计各细化级别的功能/流程数量
   - 识别未达L4深度的流程
   - 识别断裂的引用链
   - 生成核验报告
```

#### 核验任务模板（三代理并行 — 文件输出模式）

```
═══════════════════════════════════════════════════════
任务1: User Proxy — 以用户身份走完 [功能域] 的所有流程
═══════════════════════════════════════════════════════

你是User Proxy（用户代理）。你的任务是**扮演真实用户**走完以下功能域的所有流程。

**先读取当前PRD**: {prd_path}

功能域: [功能范围描述，如 "用户管理模块 F-01~F-10"]

请执行以下步骤：

1. 选择用户角色（至少2种: 首次用户 + 熟练用户）

2. 对每个 FL-{NN} 流程，逐个步骤走一遍:
   - 我现在看到了什么？（视觉元素）
   - 我应该做什么？（操作引导）
   - 我做了之后发生什么？（反馈）
   - 如果我做错了怎么办？（容错）
   - 如果我中途离开怎么办？（恢复）

3. 记录每个让你困惑、卡住、不确定的地方

4. 产出发现清单:
   | 流程步骤 | 用户角色 | 遇到的问题 | PRD缺失描述 | 优先级 |

**将完整结果写入文件: {output_path}/user-proxy-findings.md**
完成后只返回一行摘要: "User Proxy完成, X条体验缺陷(P0:Y, P1:Z)"
```

```
═══════════════════════════════════════════════════════
任务2: Dev Proxy — 以工程师身份检查 [功能域] 的可实施性
═══════════════════════════════════════════════════════

你是Dev Proxy（开发者代理）。你的任务是**模拟工程师凭PRD实施**以下功能域。

**先读取当前PRD**: {prd_path}

功能域: [功能范围描述]

请执行以下步骤：

1. 假设你对产品一无所知，只看PRD

2. 对每个 F-{NN} / FL-{NN}，检查:
   - 数据模型: 字段是否完整？类型？约束？默认值？
   - 业务规则: 是否精确到公式？有无模糊描述？
   - 状态流转: 是否完整？有无遗漏转换？
   - 接口契约: 输入输出？错误码？分页？
   - 边界条件: 空值？最大值？并发？幂等？
   - 非功能约束: 性能？安全？兼容性？

3. 记录每个让你不确定、需要猜测的地方

4. 产出发现清单:
   | 功能/流程/数据 | 实施困惑 | 可能的理解方式 | 需补充内容 | 优先级 |

**将完整结果写入文件: {output_path}/dev-proxy-findings.md**
完成后只返回一行摘要: "Dev Proxy完成, X条实施缺陷(P0:Y, P1:Z)"
```

```
═══════════════════════════════════════════════════════
任务3: Integrator — 全局一致性检查 + 合并去重
═══════════════════════════════════════════════════════

你是Integrator（整合者）。你有两重任务。

**先读取当前PRD**: {prd_path}

功能域: [功能范围描述]

**第一重: 全局一致性检查**

请检查:
1. 跨功能一致性 — 不同F-{NN}之间是否矛盾
2. 术语一致性 — 同一概念是否用了不同名称
3. 数据一致性 — 同一实体在不同地方定义是否一致
4. 流程衔接 — 上游流程的输出是否满足下游的输入
5. 索引完整性 — 所有ID唯一、所有引用有效

产出发现清单:
| 检查项 | 关联功能A | 关联功能B | 矛盾描述 | 需修正内容 | 优先级 |

**将一致性发现写入文件: {output_path}/integrator-findings.md**

---

**第二重: 合并去重**（在三个代理都完成后执行）

读取:
- {output_path}/user-proxy-findings.md
- {output_path}/dev-proxy-findings.md
- {output_path}/integrator-findings.md

执行:
1. 去重: 相同根因的发现合并为一条
2. 优先级校正: 综合三个视角重新评估
3. 关联分析: 标记同一根因在多个流程中的影响
4. 产出最终缺陷清单

将最终合并结果写入: {output_path}/merged-issues.md
完成后只返回一行摘要: "Integrator完成, 合并后X条缺陷(P0:Y, P1:Z, P2:W)"
```

#### 核验完成标准

```
✅ 所有功能点细化到L4深度
✅ 所有流程有完整步骤（happy path + 异常分支）
✅ 所有子流程引用有效
✅ 所有步骤有用户感知描述
✅ 所有步骤有时间描述
✅ 所有UI步骤有空间描述
✅ 所有数据模型有字段级定义
✅ 所有非功能需求有量化指标
✅ PRD可不经询问直接实施
✅ 三个代理各自的发现已合并去重
✅ 缺陷清单已记录并分类
```

### Step 9: 文档记录 (Document Round)

Create iteration report and plan next round. All docs go to `docs/prd-iterations/{name}/round-N/`.

**必做事项:**
1. 生成本轮报告 → `docs/prd-iterations/{name}/round-N/report.md`
2. **生成下轮计划** → `docs/prd-iterations/{name}/round-N+1/plan.md`
   - 从总计划拆解下轮焦点
   - 合并本轮剩余缺陷 + 新发现
   - 每3轮还需合并复盘改进措施
   - 包含：焦点功能域、三代理评审重点、细化深度目标
3. **每3轮进行复盘**（当 N % 3 == 0 时）→ 写入 `report.md` Section 9
   - 3轮趋势分析（缺陷发现/修复趋势、细化深度变化）
   - 流程改进点（哪里做得好、哪里可以改进）
   - 方法改进建议
   - 改进措施列入下轮 `plan.md`

## PRD Document Structure

### Standard PRD Template

```markdown
# {产品/功能名称} 产品需求文档 (PRD)

> **版本**: v{N}
> **创建日期**: YYYY-MM-DD
> **最后更新**: YYYY-MM-DD
> **作者**: [作者]
> **状态**: 草稿/评审中/已定稿

## 修订历史
| 版本 | 日期 | 修改内容 | 作者 |
|:----:|:----:|---------|:----:|
| v1 | YYYY-MM-DD | 初始版本 | ... |
| v2 | YYYY-MM-DD | R2迭代后细化 | ... |

## 1. 概述
### 1.1 产品背景
[为什么要做这个产品/功能]

### 1.2 产品目标
- [ ] 目标1（可量化）
- [ ] 目标2（可量化）

### 1.3 目标用户
| 用户角色 | 描述 | 核心需求 |
|---------|------|---------|
| 角色1 | ... | ... |

### 1.4 术语表
| 术语 | 定义 |
|------|------|
| ... | ... |

## 2. 功能索引
### 2.1 功能总览
| ID | 功能名称 | 优先级 | 主流程 | 状态 |
|:--:|---------|:------:|:------:|:----:|
| F-01 | ... | Must | FL-01 | ✅/🔄/⬜ |
| F-02 | ... | Should | FL-02 | ... |

### 2.2 流程总览
| ID | 流程名称 | 父流程 | 步骤数 | 状态 |
|:--:|---------|:------:|:------:|:----:|
| FL-01 | ... | - | N | ✅/🔄/⬜ |
| FL-01-01 | ... | FL-01 | N | ... |

## 3. 功能详细描述

### F-01 {功能名称}

> **优先级**: Must / Should / Could
> **主流程**: FL-01
> **前置条件**: [什么条件下此功能可用]
> **后置条件**: [此功能完成后系统/用户的状态变化]

#### 3.1.1 功能说明
[详细描述功能的目的和行为]

#### 3.1.2 用户角色与权限
| 角色 | 可执行操作 | 可见数据 |
|------|----------|---------|
| 角色1 | ... | ... |

#### 3.1.3 业务规则
[精确的业务规则，包括公式、计算逻辑、排序规则等]

#### 3.1.4 数据模型
| 字段名 | 类型 | 必填 | 校验规则 | 说明 |
|--------|------|:----:|---------|------|
| ... | string/number/... | 是/否 | ... | ... |

#### 3.1.5 非功能要求
| 维度 | 要求 | 量化指标 |
|------|------|---------|
| 性能 | ... | 响应时间 < Xms |
| 安全 | ... | ... |
| 可用性 | ... | ... |

### F-02 {功能名称}
[同上结构]

## 4. 流程详细描述

### FL-01 {流程名称}

> **归属功能**: F-01
> **触发条件**: [什么触发此流程开始]
> **结束条件**: [什么标志此流程结束]
> **前置状态**: [系统/用户在流程开始前的状态]
> **后置状态**: [系统/用户在流程结束后的状态]

#### FL-01-01 {步骤名称}

> **时间**: [步骤持续/触发的时间描述]
> **空间**: [在界面上的位置/布局]
> **触发**: [什么触发此步骤]

**用户感知**:
- 👁 看到: [用户看到的视觉元素]
- 👆 操作: [用户执行的操作]
- 🔊 反馈: [系统给出的反馈]

**系统行为**:
- [系统在此步骤执行的逻辑]

**异常分支**:
| 条件 | 处理方式 | 转至步骤 |
|------|---------|:--------:|
| [异常条件1] | [处理方式] | FL-01-{NN} |

#### FL-01-02 {步骤名称}
[同上结构]

#### → FL-01-03 {子流程名称}

> 此子流程在 FL-01-03 被引用，详见下方定义

### FL-01-03 {子流程名称}

> **归属流程**: FL-01 (也服务于 FL-05)
> **触发条件**: [什么触发此子流程]
> **结束条件**: [什么标志此子流程结束]

#### FL-01-03-01 {步骤名称}
[同步骤结构]

#### FL-01-03-02 {步骤名称}
[同步骤结构]

### FL-02 {流程名称}
[同上结构]

## 5. 全局约束
### 5.1 性能要求
| 场景 | 指标 | 目标值 |
|------|------|--------|
| ... | 响应时间 | < Xms |

### 5.2 安全要求
[全局安全策略]

### 5.3 兼容性要求
| 平台 | 最低版本 |
|------|---------|
| iOS | ... |
| Android | ... |
| Chrome | ... |

### 5.4 可用性要求
[无障碍、多语言等]

## 6. 数据总览
### 6.1 实体关系
[ER图或实体关系描述]

### 6.2 全局数据字典
| 实体 | 字段 | 类型 | 说明 |
|------|------|------|------|
| ... | ... | ... | ... |

## 7. 附录
### 7.1 线框图/原型链接
[链接或描述]

### 7.2 API设计概要
[如有]

### 7.3 开放问题
| ID | 问题 | 状态 | 决定 |
|:--:|------|:----:|------|
| Q-001 | ... | 开放/已决定 | ... |
```

## Documentation System

### Doc Types and Save Locations

| 层级 | Doc Type | Filename | Save Path | When |
|:----:|----------|----------|-----------|------|
| 全局 | PRD文档 | `{name}.md` | `docs/prd/` | PRD开始编写，贯穿全局 |
| 全局 | 迭代总计划 | `PLAN.md` | `docs/prd-iterations/{name}/` | 迭代开始前，贯穿全局 |
| 轮次 | 轮次计划 | `round-N/plan.md` | `docs/prd-iterations/{name}/round-N/` | 每轮开始前 |
| 轮次 | 轮次报告 | `round-N/report.md` | `docs/prd-iterations/{name}/round-N/` | 每轮结束 |
| 轮次 | 缺陷追踪 | `round-N/issues.md` | `docs/prd-iterations/{name}/round-N/` | 三代理评审后 |
| 轮次 | 用户代理发现 | `round-N/review/user-proxy-findings.md` | `docs/prd-iterations/{name}/round-N/review/` | User Proxy输出 |
| 轮次 | 开发者代理发现 | `round-N/review/dev-proxy-findings.md` | `docs/prd-iterations/{name}/round-N/review/` | Dev Proxy输出 |
| 轮次 | 整合者发现 | `round-N/review/integrator-findings.md` | `docs/prd-iterations/{name}/round-N/review/` | Integrator输出 |
| 轮次 | 合并缺陷清单 | `round-N/review/merged-issues.md` | `docs/prd-iterations/{name}/round-N/review/` | 合并去重后 |
| 全局 | 迭代总结 | `SUMMARY.md` | `docs/prd-iterations/{name}/` | 迭代完成时 |

### Directory Structure

```
docs/
├── prd/
│   └── {name}.md                           # PRD主文档
└── prd-iterations/
    ├── {prd-name}/                          # PRD迭代主题
    │   ├── PLAN.md                          # 迭代总计划
    │   ├── round-1/
    │   │   ├── plan.md
    │   │   ├── report.md
    │   │   ├── issues.md
    │   │   └── review/
    │   │       ├── user-proxy-findings.md
    │   │       ├── dev-proxy-findings.md
    │   │       ├── integrator-findings.md
    │   │       └── merged-issues.md
    │   ├── round-2/
    │   │   ├── plan.md
    │   │   └── ...
    │   └── SUMMARY.md
    └── ...
```

### Template: `docs/prd-iterations/{name}/PLAN.md`

```markdown
# {PRD名称} 迭代细化计划

> **创建日期**: YYYY-MM-DD
> **目标**: [本轮迭代要达成的细化目标]
> **范围**: [涉及的功能模块]

## 背景
[为什么要做这个PRD细化，当前PRD的不足]

## 目标
- [ ] 所有功能细化到L4深度
- [ ] 所有流程有完整步骤覆盖
- [ ] 所有问题澄清无歧义

## 质量目标
| 指标 | 目标 |
|------|------|
| P0缺陷(无法实施) | 0 |
| P1缺陷(可能实施错) | 0 |
| L4功能覆盖率 | 100% |
| 流程步骤完整率 | 100% |
| 可实施性通过率 | 100% |

## 预估轮次
| 轮次 | 焦点 |
|------|------|
| R1 | 框架搭建+核心功能F-{NN}~F-{NN} |
| R2 | 次要功能+异常分支+数据模型 |
| R3 | 非功能需求+全局一致性+终审 |

## 验收标准
1. 所有F-{NN}达到L4深度
2. 所有FL-{NN}有完整步骤(happy path + 异常)
3. 工程师可不经询问直接实施
4. 无P0/P1缺陷
```

### Template: `docs/prd-iterations/{name}/round-N/report.md`

```markdown
# Round N PRD迭代报告

> **日期**: YYYY-MM-DD
> **迭代周期**: 第N轮 — [主题]
> **内部循环次数**: N

## 1. 三代理评审发现

### User Proxy 发现摘要
| ID | 流程步骤 | 用户角色 | 问题 | 优先级 |
|:--:|---------|---------|------|:------:|
| U-01 | FL-{NN}-{NN} | 首次用户 | ... | P0-P3 |

### Dev Proxy 发现摘要
| ID | 功能/流程 | 实施困惑 | 需补充内容 | 优先级 |
|:--:|---------|---------|-----------|:------:|
| D-01 | F-{NN} | ... | ... | P0-P3 |

### Integrator 发现摘要
| ID | 检查项 | 关联功能 | 矛盾描述 | 优先级 |
|:--:|--------|---------|---------|:------:|
| I-01 | 术语一致 | F-01 ↔ F-05 | ... | P0-P3 |

### 合并去重结果
| ID | 类型 | 来源 | 描述 | 优先级 | 根因分类 |
|:--:|------|------|------|:------:|---------|
| M-01 | 交互缺失 | U-01, D-03 | ... | P1 | 流程细化不足 |

## 2. 修复内容
| ID | 对应缺陷 | PRD章节 | 修复方式 | 新增ID |
|:--:|---------|---------|---------|:------:|
| F-01 | M-01 | §3.1 | 补充子流程 | FL-01-02 |

## 3. 内部循环记录
| 子轮次 | 发现 | 修复 | 剩余P0 | 剩余P1 | 备注 |
|--------|:----:|:----:|:------:|:------:|------|
| N.1 | 5 | 3 | 2 | 0 | 首次评审 |
| N.2 | 2 | 2 | 0 | 0 | 修复后重查 |
| **合计** | **7** | **5** | **0** | **0** | |

## 4. 细化深度统计
| 深度 | R(N-1) | RN | 变化 |
|:----:|:------:|:--:|:----:|
| L0 | N | N | ↑/↓ |
| L1 | N | N | |
| L2 | N | N | |
| L3 | N | N | |
| L4 | N | N | |

## 5. 回顾(跨轮趋势)
| 指标 | R1 | R2 | ... | RN | 趋势 |
|------|:--:|:--:|:---:|:--:|:----:|
| L4覆盖率 | | | | | ↑/↓/→ |
| P0缺陷 | | | | | |
| P1缺陷 | | | | | |
| User Proxy发现 | | | | | |
| Dev Proxy发现 | | | | | |
| Integrator发现 | | | | | |
| 可实施性通过率 | | | | | |

## 6. 剩余缺陷(移交下轮)
| ID | 问题 | 优先级 | 来源 | 备注 |
|:--:|------|:------:|------|------|

## 7. 下轮计划
> 详见 `docs/prd-iterations/{name}/round-N+1/plan.md`

## 8. 复盘（每3轮，当 N % 3 == 0 时）

> 仅在第3、6、9...轮时填写

### 8.1 趋势分析（近3轮）
| 指标 | R(N-2) | R(N-1) | R(N) | 趋势 | 分析 |
|------|:------:|:------:|:----:|:----:|------|
| 三代理总发现 | | | | ↑/↓/→ | ... |
| P0 | | | | | ... |
| P1 | | | | | ... |
| L4覆盖率 | | | | | ... |
| 内部循环次数 | | | | | ... |

### 8.2 流程改进
| 项目 | 做得好 | 可改进 | 改进措施 |
|------|--------|--------|----------|
| User Proxy视角 | ... | ... | ... |
| Dev Proxy视角 | ... | ... | ... |
| Integrator合并 | ... | ... | ... |
| 修复效率 | ... | ... | ... |

### 8.3 改进措施（列入下轮计划）
| ID | 改进措施 | 验收标准 |
|----|---------|---------|
| IMP-01 | ... | ... |
```

### Template: `docs/prd-iterations/{name}/round-N/issues.md`

```markdown
# Round N PRD缺陷清单

> **日期**: YYYY-MM-DD
> **来源**: 三代理并行评审 + Integrator合并去重

## 缺陷列表
| ID | 严重度 | 类型 | 来源 | 描述 | PRD位置 | 状态 |
|:--:|:------:|------|------|------|---------|:----:|
| M-01 | P0 | 实施歧义 | D-01 | ... | §3.1 | 🔄/✅/❌ |
| M-02 | P1 | 交互缺失 | U-02 | ... | §4.2 | ... |
| M-03 | P1 | 一致性 | I-01 | ... | §3.1↔§5.3 | ... |

## 按来源统计
- User Proxy发现: N条 (去重后保留: N)
- Dev Proxy发现: N条 (去重后保留: N)
- Integrator发现: N条 (去重后保留: N)
- 去重合并: N条

## 按严重度统计
- P0(无法实施): N (修复: N)
- P1(可能实施错/体验严重受损): N (修复: N)
- P2(需确认/体验不佳): N (修复: N)
- P3(建议): N (backlog)
```

## Execution Model

### Use Subagents

**CRITICAL**: Main session orchestrates only. All work via subagents.

```
Main session: Task dispatch, status tracking, user communication, reading result files for summary
Subagents:   PRD writing, completeness checks, reviews, three-proxy roles
```

### File-Based Output Protocol (CRITICAL)

**ALL subagent results MUST be written to files, NOT returned to the main session.**

```
规则:
1. 每个子任务必须将结果写入指定文件路径
2. 主会话通过 Read 工具读取文件摘要，不接收完整返回
3. 子任务使用 run_in_background: true 运行，避免阻塞
4. 主会话通过 TaskOutput (block=false) 或 Read 工具检查进度和结果

违反此规则的后果:
- 主会话上下文被大量子任务输出填满
- 后续轮次质量严重下降（上下文不足）
```

### Subagent Execution Patterns

#### Pattern 1: Parallel Proxies + Sequential Merge

```
# Step 1: 三个代理并行启动（同时发出三个 Task）
Task(A user-proxy): 走流程 → 写入 user-proxy-findings.md
Task(B dev-proxy):  检查可实施性 → 写入 dev-proxy-findings.md
Task(C integrator): 全局一致性 → 写入 integrator-findings.md

# Step 2: 等待三个代理全部完成
TaskOutput(A, block=true)
TaskOutput(B, block=true)
TaskOutput(C, block=true)

# Step 3: Integrator 合并去重（串行，依赖前三者）
Task(D merge): 读取三份清单 → 合并去重 → 写入 merged-issues.md

# Step 4: 等待合并完成，读取摘要
Read merged-issues.md → 向用户报告结论
```

#### Pattern 2: Parallel Independent Fixes

```
# 并行启动多个独立修复任务（每个输出到文件）
Task(A fix-p0): 修复缺陷M-01 → 写入 fixes/M-01.md
Task(B fix-p1): 修复缺陷M-02 → 写入 fixes/M-02.md

# 等待所有完成
TaskOutput(A, block=true)
TaskOutput(B, block=true)
```

### Main Session Responsibilities (ONLY)

```
主会话只负责:
1. 创建任务列表 (TaskCreate)
2. 分发子任务 (Task with subagent)
3. 跟踪任务状态 (TaskUpdate/TaskList)
4. 读取结果文件摘要 (Read tool, 只读关键结论)
5. 向用户汇报进展
6. 生成汇总文档 (report.md)
7. 编排下一轮流程

主会话禁止:
- 直接修改PRD（交给subagent）
- 直接读取大量PRD内容（交给subagent）
- 将subagent的完整返回纳入上下文
```

## Metrics to Track

| Metric | Description | Target |
|--------|-------------|--------|
| L4 coverage | Functions at L4 depth / total | 100% |
| Flow step completeness | Steps with full detail / total steps | 100% |
| User perception coverage | Steps with perception desc / total | 100% |
| Time description coverage | Steps with timing / total | 100% |
| Space description coverage | UI steps with layout desc / total | 100% |
| Exception coverage | Flows with exception branches / total | 100% |
| Implementability score | Sections implementable without guessing | 100% |
| P0 issues | Cannot implement at all | 0 |
| P1 issues | Might implement incorrectly | 0 |
| ID consistency | All IDs unique, all references valid | 100% |
| Deduplication rate | Overlapping findings merged / total findings | Track |
| Inner loop count | Fix cycles per round | Decreasing |

## Anti-Patterns

1. **Fixing before proxy review** — Always review first
2. **Single-perspective review** — All three proxies must run; skipping any one misses a class of issues
3. **Running proxies sequentially** — They should run in parallel; sequential wastes rounds
4. **Skipping the merge step** — Deduplication and priority correction are essential
5. **User Proxy as spec reader** — User Proxy must *simulate* being a user, not just read the PRD as text
6. **Dev Proxy as code reviewer** — Dev Proxy must *simulate* trying to implement, not review PRD style
7. **Integrator as mere combiner** — Integrator has its own perspective (global consistency), not just merge others
8. **Skipping inner loop** — Don't stop until P0=0 AND P1=0
9. **Fixing multiple issues at once** — One at a time
10. **Skipping re-check** — Every fix needs verification
11. **Running everything in main session** — Use subagents
12. **Incomplete flow detail** — Must describe every step to user-perceivable level
13. **Skipping verification phase** — Must use three-proxy verification
14. **Renumbering IDs** — IDs are permanent once assigned; add new ones, never renumber
15. **Dangling references** — Every → FL-{NN} must point to an existing definition
16. **Assuming "obvious"** — If it's obvious, write it down; if it's not written, it doesn't exist
17. **Vague timing** — "Soon" is not timing; use exact durations or conditions
18. **Vague layout** — "Top" is not layout; use specific position/size/spacing
19. **Happy path only** — Every flow must cover error/edge/exception branches
20. **Returning subagent output to main session** — Results MUST go to files
21. **Main session doing real work** — Main session orchestrates only
22. **Writing PRD for yourself** — Write for an engineer who has zero context about the product

## Trigger Phrases

- "PRD" / "需求文档"
- "产品需求" / "PRD评审"
- "迭代PRD" / "iterative PRD"
- "需求细化" / "requirement refinement"
- "流程细化" / "flow detailing"
- "功能设计" / "feature design"
