# OTA 呼叫中心一线排班 · 开源参考与零基础起步

面向：**OTA（在线旅游）呼叫中心一线坐席排班系统**，从 0 搭建时的参考仓库分类、优缺点，以及完全开发小白的起步路线。

> 说明：GitHub 上几乎没有能直接当「成品 WFM」用的完整开源系统。更现实的做法是按模块借鉴，而不是 fork 一个仓库改完就上线。

## 文档导航

| 文档 | 内容 |
|------|------|
| [references/00-overview.md](references/00-overview.md) | 四层架构总览（该抄什么、不该抄什么） |
| [references/01-demand-staffing.md](references/01-demand-staffing.md) | A. 话务人力需求（Erlang / shrinkage） |
| [references/02-auto-scheduling.md](references/02-auto-scheduling.md) | B. 自动排班求解器 |
| [references/03-product-shell.md](references/03-product-shell.md) | C. 排班产品壳（后台 / 日历） |
| [references/04-end-to-end.md](references/04-end-to-end.md) | D. 端到端小样与日内重排 |
| [references/comparison.md](references/comparison.md) | 总对比表（推荐指数） |
| [getting-started/for-complete-beginners.md](getting-started/for-complete-beginners.md) | 零基础：你到底怎么开始 |
| [getting-started/mvp-roadmap.md](getting-started/mvp-roadmap.md) | 最小可用产品（MVP）分阶段路线 |

## 一句话结论

1. **先学业务，再写代码**：先搞懂「每 30 分钟要多少人」→「谁上哪个班」→「怎么发布给一线」。
2. **别一上来做自动求解**：先做「手工排班后台」，再接 Erlang 算需求，最后才上 Timefold/OR-Tools。
3. **优先参考组合**：`mod_turbotab`（算人） + `Timefold employee-scheduling`（求解） + `StaffScheduler`（产品壳）。

## 更新记录

- 2026-09-11：初版，整理自公开 GitHub 检索与 README 阅读。
