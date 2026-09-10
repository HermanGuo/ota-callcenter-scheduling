# D. 端到端小样与日内重排

## 1. 3lasgit/PLaNi

- 链接：https://github.com/3lasgit/PLaNi
- 定位：呼叫中心「预测 + 组合优化 + 排班表」Streamlit 小应用 · 星极少

### 优点
- **流水线最完整**：在岗 → 预测需求 → 优化 → 出表，一眼能看懂 WFM 闭环
- 适合画架构图时对照

### 缺点
- 不是生产级；代码与工程化弱
- **不要 fork 当线上系统**

### 适合你用来
- 理解端到端数据流；拆模块时当示意

---

## 2. vikas-prasad-cx/reeforce

- 链接：https://github.com/vikas-prasad-cx/reeforce
- 语言：Java · 星很少 · contact-center 域含 Erlang

### 优点
- 聚焦 **日内**：缺口看板、中短期调整（lunch SL cliff 这类场景）
- 明确说了自己不是完整企业 WFM，边界清楚

### 缺点
- 很新、资料少
- 不替代「日前排班」主流程

### 适合你用来
- 第二期以后：学「当天怎么补洞」；一期可忽略

---

## 3. 仅作历史参考（不建议投入）

| 项目 | 说明 |
|------|------|
| betaiotazeta/AutoShiftPlanner | OptaPlanner 桌面老示例，约束思路可看，栈旧 |
| OfficeStack/OpenSkedge | 早期员工排班，已归档 |

---

## 小结（D 层）

| 项目 | 推荐指数 | 建议 |
|------|----------|------|
| PLaNi | ★★★ | 看流水线，别当底座 |
| reeforce | ★★★ | 二期日内重排灵感 |
| AutoShiftPlanner / OpenSkedge | ★★ | 扫一眼即可 |
