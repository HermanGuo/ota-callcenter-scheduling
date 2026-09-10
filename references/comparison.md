# 总对比表（OTA 呼叫中心视角）

评分是「对你从 0 搭建 OTA 一线排班的参考价值」，不是单纯看星标。

| 仓库 | 层级 | 推荐 | 优点一句话 | 缺点一句话 | 现在要精读吗 |
|------|------|------|------------|------------|--------------|
| mod_turbotab | A 需求 | ★★★★★ | 最像真 WFM 人力测算 | 不会排班 | 第 2 阶段精读 |
| Timefold employee-scheduling | B 求解 | ★★★★★ | 工业级约束求解 | Java 门槛高 | 第 3 阶段再碰 |
| roster-wizard | B 求解 | ★★★★ | Python 可跑自动排班 | 场景偏医院 | 想走 Python 时精读 |
| shift_schedule | B 教材 | ★★★★ | 中文讲清建模 | 非产品 | 写约束清单时读 |
| aivot | B 思路 | ★★★★ | 规则 + 冲突解释 | 新、许可 GPL | 学产品思路 |
| StaffScheduler | C 产品壳 | ★★★★ | 现代全栈蓝本 | 星少、非 CC | 有基础后对照 |
| employee-shift-scheduler | C 产品壳 | ★★★★ | 经典后台形态 | 无 WFM 测算 | 看表结构 |
| Shiftplan | C 运营 | ★★★ | 轮转/审计 | 无 SLA 区间 | 可选 |
| PLaNi | D 流水线 | ★★★ | 端到端示意 | 非生产 | 画架构时看 |
| reeforce | D 日内 | ★★★ | 日内缺口 | 太新 | 二期 |
| Erlang (kpg) | A 入门 | ★★★ | 短小好懂 | 功能少 | 可选入门 |
| AutoShiftPlanner | 历史 | ★★ | 老 Opta 示例 | 过时 | 不投入 |
| OpenSkedge | 历史 | ★★ | 早期产品 | 已归档 | 不投入 |

## 推荐精读组合（按目标）

1. **只想尽快做出能用的排班工具（推荐）**  
   自己写极简后台 → 对照 StaffScheduler / employee-shift-scheduler 的表结构 → 以后再接 mod_turbotab。

2. **想先搞懂呼叫中心「要多少人」**  
   mod_turbotab +（可选）kpg Erlang。

3. **想先搞懂自动排班算法**  
   shift_schedule（概念）→ roster-wizard（Python 跑通）→ Timefold（长期）。

4. **想做接近线上的完整架构**  
   A: mod_turbotab · B: Timefold 或 OR-Tools · C: 自研壳 · D: 以后再看 reeforce。
