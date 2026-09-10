# B. 自动排班求解层

把「每时段要多少人」变成「张三周一早班、李四夜班…」。

## 1. TimefoldAI/timefold-solver + timefold-quickstarts

- 求解器：https://github.com/TimefoldAI/timefold-solver （约 1.8k★）
- 快速开始：https://github.com/TimefoldAI/timefold-quickstarts （约 577★）
- 关键用例目录：`use-cases/employee-scheduling`
- 语言：Java（主）· 工业级 · 活跃

### 优点
- **业界开源里最强的一档**：约束建模、硬/软约束、可扩展性都成熟
- quickstart 里有员工排班现成例子，能跑通「约束 → 解」
- OptaPlanner 已归档，后续生态看 Timefold

### 缺点
- **Java 栈**，对完全小白门槛高（概念多：实体、约束流、求解器配置）
- 这是「引擎」，不是带换班审批的完整产品
- 学习曲线陡，不适合作为人生第一个编程项目

### 适合你用来
- 中后期（你会一点后端之后）当自动排班引擎
- 现在：只需要知道「有这一层」，先别深啃源码

---

## 2. galojix/roster-wizard

- 链接：https://github.com/galojix/roster-wizard
- 语言：Python / Django + OR-Tools · 约 62★ · 仍有更新

### 优点
- **Python 友好**，比 Java 求解器更好上手
- 有技能搭配、员工诉求、班次顺序等规则，接近真实排班
- Web 可跑，能看到「规则 → 班表」闭环

### 缺点
- 偏医院/护士场景，需改造成呼叫中心「区间覆盖」
- 规模与工程化程度不如 Timefold
- 文档/社区体量一般

### 适合你用来
- 想用 Python 做自动排班时的第一份可运行参考

---

## 3. weiran-aitech/shift_schedule

- 链接：https://github.com/weiran-aitech/shift_schedule
- 语言：Python · 约 48★ · 中文说明友好

### 优点
- **中文**讲清员工/护士排班如何用约束规划建模
- 适合建立「什么是硬约束/软约束」的概念

### 缺点
- 更偏研究/建模，不是完整产品
- 不能直接当 OTA 呼叫中心系统用

### 适合你用来
- 学习资料；写业务约束清单时对照

---

## 4. angesanze/aivot

- 链接：https://github.com/angesanze/aivot
- 语言：Django + React + OR-Tools · 星很少 · 概念新

### 优点
- **规则目录**设计清晰；排不出时能解释冲突（对运营极重要）
- 有引导式流程：人 → 班次 → 规则 → 出表

### 缺点
- 太新、星少，稳定性与文档需自己验证
- GPL-3.0，商用二次开发要注意许可

### 适合你用来
- 抄「规则可配置 + 冲突解释」产品思路，不必整仓 fork

---

## 小结（B 层）

| 项目 | 推荐指数 | 建议 |
|------|----------|------|
| Timefold quickstarts | ★★★★★ | 中长期引擎首选（Java） |
| roster-wizard | ★★★★ | Python 路线首选可跑 demo |
| shift_schedule | ★★★★ | 中文建模教材 |
| aivot | ★★★★ | 学规则与冲突解释 |
