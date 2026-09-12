# 开源排班与 OR-Tools 调查报告 v0.1

> 主笔调研：auto scheudling · 成文润色：Writing Bot  
> 日期：2026-09-12 · 试点基线：HKCG Chat（中国侧）  
> 对齐：`hard_constraints_pilot_v0.2` · `project_plan_onepager_v1` · `architecture_swimlanes_v0`  
> 仓库路径：`docs/research_opensource_scheduling_or_tools_v0.md`  
> 本地 SSOT：`18_Auto_Scheduling\docs\research_opensource_scheduling_or_tools_v0.md`  
> 修订说明：相对调研初稿仅调整结构与表述；技术结论未改。

---

## 0. 摘要

1. **开源现状**：没有可直接替代本项目「双泳道 + Shift Pool」的完整开源 WFM。可用资产分四层（人力测算、约束求解、产品壳、端到端小样），须拼装，不可整仓 fork。  
2. **OR-Tools（CP-SAT）适合作为 WFM 泳道 Schedule Solver 的主候选**：在硬约束下生成 **Shift Pool（需求班次供给）**，并压低相对 `required_hc` 的拟合误差（产品主口径 WMAPE）。  
3. **竞标、换班、自助改班、Manager override 不宜进入求解主路径**：属 Service 泳道，由规则引擎与流程消化。  
4. **若采用 OR-Tools**：决策变量须改为「开哪些班型槽位 / 开多少」；编码 L1–L7 与 H1/H2/H6/H7；餐休扣净人力（H7）；并用独立合规校验器验收「Hard 违反数 = 0」。  
5. **后续工作**：薄 MVP（启发式基线 + CP-SAT）、数据契约、WMAPE 双报、与 Service 竞标窗对接；Timefold 作 Java 备选对照，不阻塞 Phase 1。

---

## 1. 相关开源项目：优缺点

评价维度：是否贴近呼叫中心 / workforce 思路、约束表达力、与「只出 Shift Pool、不到人」架构的契合度、工程可维护性、许可与活跃度。

### 1.1 人力需求层（算「要多少人」，不生成班表）

| 项目 | 优点 | 缺点 | 对本项目 |
|------|------|------|----------|
| [gstvbatista/mod_turbotab](https://github.com/gstvbatista/mod_turbotab) | Erlang C/A、shrinkage、多技能测算；区分「在线人数」与「应排人数」 | 不生成班表；非完整产品 | **Phase 1 前后可参考**；当前 HKCG 主路径为量÷CPH→`required_hc`，Erlang 非门禁 |
| [kpg141260/Erlang](https://github.com/kpg141260/Erlang) | 短小，易读懂 Erlang C | 功能少、维护弱 | 教学用；不进生产依赖 |

### 1.2 约束求解层（自动生成班次 / 到人）

| 项目 | 优点 | 缺点 | 对本项目 |
|------|------|------|----------|
| **Google OR-Tools CP-SAT**（库，非单一业务仓） | Python/C++ 可用；硬约束表达强；限时求近优；呼叫中心/护士排班案例多 | 须自建领域模型与数据管道；无现成「Shift Pool + WMAPE」产品 | **WFM Solver 首选库** |
| [TimefoldAI/timefold-solver](https://github.com/TimefoldAI/timefold-solver) + [quickstarts employee-scheduling](https://github.com/TimefoldAI/timefold-quickstarts) | 工业级；员工排班用例完整；硬/软约束体系成熟 | Java 为主；示例多为**到人 assignment**，与 Shift Pool 主路径不一致 | Phase 1 可作对照；不阻塞 Python/OR-Tools 路线 |
| [galojix/roster-wizard](https://github.com/galojix/roster-wizard) | Django + OR-Tools；技能/班序规则可跑通 | 偏医院到人；非 CC Shift Pool | 学「规则→求解」接线 |
| [weiran-aitech/shift_schedule](https://github.com/weiran-aitech/shift_schedule) | 中文讲清硬/软约束建模 | 研究向，非产品 | 写约束目录时对照 |
| [angesanze/aivot](https://github.com/angesanze/aivot) | 规则目录 + 不可解时解释冲突 | 新、星少；GPL-3.0 | **学冲突解释**；许可需法务审慎 |
| [betaiotazeta/AutoShiftPlanner](https://github.com/betaiotazeta/AutoShiftPlanner) | 经典 OptaPlanner 约束示例 | 栈旧、桌面端 | 历史参考，不投入 |

### 1.3 产品壳 / 运营（后台、日历、轮转）

| 项目 | 优点 | 缺点 | 对本项目 |
|------|------|------|----------|
| [lucaosti/StaffScheduler](https://github.com/lucaosti/StaffScheduler) | 班次模板、发布、可选自动生成；TS 全栈清晰 | 非 CC；星少 | Service/后台对照，非 Solver |
| [SirChri/employee-shift-scheduler](https://github.com/SirChri/employee-shift-scheduler) | React + Spring Boot 典型后台 | 无 WFM 测算与 Shift Pool | 表结构参考 |
| [Eric-Schubert/Shiftplan](https://github.com/Eric-Schubert/Shiftplan) | 轮转、节假日、审计、Docker | 无 SLA 区间拟合 | 运营功能灵感 |

### 1.4 呼叫中心 / 日内 / 端到端小样

| 项目 | 优点 | 缺点 | 对本项目 |
|------|------|------|----------|
| [3lasgit/PLaNi](https://github.com/3lasgit/PLaNi) | 预测→优化→排班表流水线直观 | 非生产级 | 仅作叙事示意 |
| [vikas-prasad-cx/reeforce](https://github.com/vikas-prasad-cx/reeforce) | 联系中心日内缺口 / Erlang capacity | 新、非日前 Shift Pool | Phase 2+ 日内灵感 |
| [OfficeStack/OpenSkedge](https://github.com/OfficeStack/OpenSkedge) | 早期员工排班产品 | 已归档 | 不投入 |

### 1.5 分层结论

| 层级 | 建议 |
|------|------|
| 需求 HC | 试点按 **量 ÷ CPH → required_hc**；Erlang 库可选增强，不作 Phase 1 阻塞 |
| 求解 | **OR-Tools CP-SAT** 生成 Shift Pool；Heuristic 作秒级基线 |
| 到人与自助 | **不**用开源「一键到人排班」当主路径；走 Service：竞标 / 换班 / 自助 / override |
| 完整开源 WFM | **不存在**可直接替换本双泳道架构的仓 |

---

## 2. Google OR-Tools：典型输入输出，以及是否适合 Auto Scheduling

### 2.1 库角色（与本架构用语对齐）

| 名称 | 含义 |
|------|------|
| OR-Tools | Google 开源优化套件 |
| CP-SAT | 其中的约束规划求解器；本项目 Schedule Solver 的推荐实现之一 |
| Heuristic | 贪心填缺口；快、可复现；作对照基线 |
| Schedule Solver | 产品名：可跑 Heuristic、CP-SAT，或择优 |

### 2.2 典型「员工排班」教科书模型（多数开源示例）

多数开源示例默认「求解器直接到人」，与本项目 Shift Pool 主路径不一致。

**常见输入：**

- 规划期（如 7 / 14 / 28 天）
- 员工集合、技能 / 资格、不可用日
- 班型或每日班次槽位
- 覆盖需求（每班或每时段最少人数）
- 硬约束（休息、连续上班、间隔等）与软约束权重

**常见输出：**

- 到人 assignment：`employee × day → shift | OFF`
- 或布尔矩阵「谁上哪个班」
- 目标函数值；不可行时的冲突信息（视建模而定）

### 2.3 本项目应采用的 OR-Tools 形态（推荐）

对齐双泳道：WFM **只产出可发布的需求班次供给**。

**输入（WFM / Shift Pool）：**

| 输入 | 说明 |
|------|------|
| `required_hc[t]` | 各 interval（建议 15/30 min）净需求人力 |
| 班型库 | 如 S0800/S0900/…；`span=9h`，`unpaid_meal=1h`，`net=8h` |
| 运营窗 | 班型起止须落在覆盖窗（H6） |
| 硬约束开关 | L1–L7、H1、H2、H7（及制度闸门 L6） |
| 求解限时 | 如 30–120s；超时返回当前最优可行解 |
| Soft 权重 | under/over，或直接代理 WMAPE；S2–S4 等 |

**决策变量（推荐）：**

- 每个「日期 × 班型」的开班数量（整数），或有限个可发布槽位的 0/1
- Phase 1 **不以** `员工 × 班` 为主变量（到人在 Service）

**输出：**

| 输出 | 说明 |
|------|------|
| Shift Pool | 发布用需求班次列表（日、班型、起止、skill_pool=HKCG、数量/槽位 ID） |
| 展开净 HC 曲线 | 扣 meal/break 后的 \(n_t\)（H7） |
| 拟合指标 | WMAPE（主）+ Coverage（辅，双报） |
| 求解状态 | Optimal / Feasible / Unknown / Infeasible |
| 审计包 | 班型库版本、约束版本、限时、随机种子（若有） |

**合规校验器（求解后必跑，可独立于 OR-Tools）：**

- 对 Shift Pool（及若存在的对照到人结果）逐条跑 L1–L7、H1/H2/H6/H7
- 成功定义：**Hard 违反数 = 0**

### 2.4 适合性判断（对照 HKCG 硬约束与双泳道）

| 诉求 | OR-Tools CP-SAT | 说明 |
|------|-----------------|------|
| 双泳道：WFM 只出 Shift Pool | **适合** | 变量设计为开班供给，而不是到人 |
| Shift Pool 拟合 `required_hc` | **适合** | 目标可代理为最小化加权 \|n−d\|（WMAPE 分子） |
| L1 单日净工时 ≤ 8h | **适合** | 班型已是 9h−1h；变量侧禁止非库班型即可（与 H1） |
| L2 周净工时 ≤ 40h | **部分适用** | 「未到人」时约束的是班次供给结构对潜在履约的可行性；完整到人周工时在 Service 履约后二次校验 |
| L3 每周至少休息 1 日 | **到人后强约束** | Pool 阶段可保留「每人可认领负荷」的松弛上界；**正式 L3 在 assignment/竞标结果上校验** |
| L4 延长工时 | **适合（若启用延长班型）** | 无延长班型则自然满足 |
| L5 节假日 | **适合** | 节假日默认不开班或强制假日标记 |
| L6 标准工时闸门 | **流程适合** | 配置层禁止综合工时放宽；非求解器内部算子 |
| L7 班间隔 ≥ 11h | **到人后强约束** | Pool 阶段通过班型组合避免「必然无法满足 11h」的供给；**认领/换班时硬校验** |
| H1 班型库 | **适合** | 变量域 = 班型库 |
| H2 每人每天 ≤ 1 班 | **Service / 到人** | Pool 不直接到人；竞标与换班强制 |
| H5 渠道资格 | **Service** | Chat 资格在认领侧校验 |
| H7 meal 扣净人力 | **适合且必须** | 展开 \(n_t\) 必须扣 unpaid meal |
| 竞标 / 换班 / 自助 / override | **不适合进求解主路径** | 与架构 SSOT 一致：流程 + 规则引擎 |

**总判：**

- **推荐采用 OR-Tools CP-SAT 作为 WFM Schedule Solver。**
- **前提：** 按 Shift Pool 建模，并配套独立合规校验；L3/L7/H2/H5 等「到人态」硬约束在 Service 履约链闭环。
- **不推荐：** 用 OR-Tools 一次性替代 Service 自助产品；不推荐照搬开源「护士到人排班」示例作为 HKCG 主路径。

---

## 3. 若采用 OR-Tools，我们要做的改造

相对「官网员工排班示例 / roster-wizard 类到人模型」，改造清单如下。

### 3.1 模型改造（必须）

1. **决策变量从到人改为开班供给**（日期 × 班型数量，或槽位 0/1）。
2. **覆盖从「每班人数」改为 interval 级净 HC**：开班展开为 \(n_t\)，并 **H7 扣餐休**。
3. **目标对齐产品**：最小化 WMAPE 代理（或加权 under/over）；Coverage 仅双报辅助。
4. **硬约束分层落地：**
   - Pool 阶段强制：H1、H6、H7、L1（班型净工时）、L5/L6（配置）、避免必然破坏 L7 的班型对。
   - Assignment/竞标阶段强制：L2/L3/L7、H2、H5，以及换班后水位。
5. **禁止**把个人偏好、休整月、IM+ 拆组规则打进 Hard（反模式；走 Service）。

### 3.2 工程改造（必须）

1. **数据契约**：`required_hc`、班型库、skill_pool=`HKCG`、发布 Shift Pool schema（与 `data_contract_v0` 对齐）。
2. **双引擎**：Heuristic 基线 + CP-SAT；同输入可比对 WMAPE。
3. **合规校验器**：与求解器解耦；CI/发布门禁 Hard 违反数 = 0。
4. **版本钉扎**：约束文件版本、班型库版本写入每次求解审计。
5. **vendor 目录**：`vendor/or-tools`（或本地 `third_party/google-or-tools`）存放许可证、版本钉、最小可运行示例；**不**把业务规则写进 vendor。

### 3.3 与 Service 的接口改造（必须）

1. 发布 Shift Pool → 竞标窗可见槽位。
2. 认领 / 换班 / 自助调用同一套合规与水位规则（L3/L7/H2/H5 等）。
3. Override 留痕进 RCA；不回写「伪造 Hard 已满足」。

### 3.4 可选改造（后期）

1. 多技能 / 多队列 `required_hc`。
2. 不可解时最小冲突集解释（可借鉴 aivot 思路）。
3. 日内重算（reeforce 类）与日前 Pool 分离。
4. Timefold 对照实验（同一数据契约）。

---

## 4. 后续工作

| 优先级 | 工作项 | 说明 |
|--------|--------|------|
| P0 | 锁定本报告结论与改造清单 | Projects Manager 纳入计划；Writing Bot 润色定稿 |
| P0 | 书面确认包（HKCG 范围 + 硬约束） | Herman 发出；Agent 不代发私信 |
| P0 | Shift Pool 数据契约与样例数据 | 假数据可跑通求解→发布 |
| P1 | Heuristic 基线可验收 | 秒级；双报 WMAPE + Coverage |
| P1 | OR-Tools CP-SAT 薄 MVP | 限时求解；Hard=0；审计包 |
| P1 | 合规校验器单测 | L1–L7、H1/H2/H6/H7 用例 |
| P2 | 与 Service 竞标/换班对接 | 员工自助选班次系统主责；WFM 只保障 Pool 质量 |
| P2 | 正式需求口径与真花名册 | 替换 interim |
| P3 | 冲突解释 / 日内 / LLM 仅解释 | 不替代求解与竞标 |

**当前明确不做：** 为各组私有规则无限拆排班组；照搬 EUCG 规则；用 LLM 直接产出可采纳班表。

---

## 5. 结论（给决策）

1. **开源**：用于学习与对照，不能替代本双泳道产品。
2. **OR-Tools CP-SAT**：**适合**作为 HKCG 试点 WFM 侧生成 Shift Pool 的求解引擎；前提是按第 3 章完成「到人示例 → 开班供给」改造，并与合规校验、Service 泳道拆分。
3. **标准工时 L1–L7**：制度与班型层（L1/L5/L6/H1/H7）进 Pool 求解；**L3/L7/H2 等以人为主体的规则在履约链强制**，避免「Pool 看似合规、到人后大面积违规」。
4. **下一步工程**：Heuristic + CP-SAT 薄 MVP → 硬约束门禁 → 再接竞标窗。

---

## 6. 参考路径

| 材料 | 路径 |
|------|------|
| 硬约束 v0.2 | `18_Auto_Scheduling\docs\hard_constraints_pilot_v0.md` |
| 计划一页纸 | `18_Auto_Scheduling\docs\project_plan_onepager_v1.md` |
| 双泳道架构 | `18_Auto_Scheduling\docs\architecture_swimlanes_v0.md` |
| 仓库内开源分类（初版） | `references/comparison.md` |
| 本报告 | `docs/research_opensource_scheduling_or_tools_v0.md` |

---

*v0.1：在调研事实与技术判断基础上完成成文润色；结论未改，除非 Project Lead 变更架构。*
