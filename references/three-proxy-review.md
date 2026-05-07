# 三代理评审 (Three-Proxy Review) — 详细指引

> 本文件是 SKILL.md Step 1A 的详细参考内容，在执行三代理评审时按需加载。

---

## The Three Proxies

| Role | Represents | Core Question | Discovers |
|------|-----------|---------------|-----------|
| User Proxy | 终端用户 | "作为用户走完这个流程，我会在哪里困惑/卡住？" | 交互缺失、感知空白、异常场景下体验断裂 |
| Dev Proxy | 实施工程师 | "拿这份流程去编码，我会在哪里不确定/无法动手？" | 歧义描述、约束缺失、边界条件遗漏 |
| Integrator | 产品负责人 | "所有流程拼在一起，哪里对不上/漏掉了？" | 跨流程矛盾、共享子流程不一致、引用断裂 |

## 各代理探索维度

**User Proxy**: 视觉感知（页面元素/加载状态）→ 操作引导（下一步做什么/可逆性/反馈）→ 异常场景（网络断/输错/失败/意外退出）→ 流程连贯性 → 不同用户角色

**Dev Proxy**: 数据完整性（字段/关联/校验）→ 状态流转（转换条件/不可达状态）→ 业务规则（公式/排序/定时任务）→ 非功能约束（性能/安全）→ 实施歧义（模糊描述/未定义概念/断裂引用）

**Integrator**: 跨流程一致性 → 术语一致性 → 数据一致性 → 流程衔接 → 索引完整性（ID唯一/引用有效/无孤立流程）

## 每条发现必须包含

- User Proxy: 流程步骤(FL-{abbr}-{NN}-{NN}) | 用户角色 | 遇到的问题 | 缺失描述 | 优先级(P0-P3)
- Dev Proxy: 流程/数据(FL-{abbr}-{NN}) | 实施困惑 | 可能的理解方式 | 需补充内容 | 优先级
- Integrator: 检查项 | 关联流程 | 矛盾描述 | 需修正内容 | 优先级

## 执行模式

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

## 代理提示词

各代理的完整提示词模板：

- [User Proxy 提示词](user-proxy-prompt.md)
- [Dev Proxy 提示词](dev-proxy-prompt.md)
- [Integrator 提示词](integrator-prompt.md)
