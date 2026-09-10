# A. 话务人力需求层（Erlang / shrinkage）

> 呼叫中心特有。医院排班、餐饮轮班通常没有这一层。

## 1. gstvbatista/mod_turbotab

- 链接：https://github.com/gstvbatista/mod_turbotab
- 语言：Python · 星约 19 · 仍在更新
- 定位：TurboTable 风格的 Erlang B/C/A、排队、多技能、shrinkage 计算 CLI

### 优点
- **最贴近 WFM 人力测算**：Erlang C、occupancy、ASA、多技能人数、shrinkage 都有
- CLI 清晰，适合小白用命令试算，也适合以后被你的后端调用
- 文档把「在线人数」和「要排进班表的人数」区分开了（这一点非常关键）

### 缺点
- 不是排班系统，**不会生成班表**
- 星不多，生态小，要自己验证公式是否符合你们 SLA 定义
- 不负责话务预测（预测要另做）

### 适合你用来
- 作为「需求人数计算器」的参考实现
- MVP 第 2 阶段：输入预测话务 → 输出每时段需求

---

## 2. kpg141260/Erlang

- 链接：https://github.com/kpg141260/Erlang
- 语言：Python · 星很少 · 轻量库

### 优点
- 代码少，**适合读懂 Erlang C 在干什么**
- 上手快，可当教学材料

### 缺点
- 功能远少于 mod_turbotab（多技能、shrinkage 等弱）
- 维护活跃度一般，不宜当长期依赖

### 适合你用来
- 周末读一遍建立直觉；生产计算优先看 mod_turbotab

---

## 小结（A 层）

| 项目 | 推荐指数 | 建议 |
|------|----------|------|
| mod_turbotab | ★★★★★ | 精读 + 可借鉴进你的服务 |
| Erlang (kpg141260) | ★★★ | 入门读物 |
