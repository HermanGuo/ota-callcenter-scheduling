# C. 排班产品壳（后台 / 日历 / 发布）

没有自动求解，也能先做「能用的排班后台」。**零基础请从这一层开始写自己的系统。**

## 1. lucaosti/StaffScheduler

- 链接：https://github.com/lucaosti/StaffScheduler
- 语言：TypeScript（Node + React）+ 可选 Python OR-Tools · 星少但结构清楚

### 优点
- **现代全栈**，班次模板、发布排班、赋值校验文档较全
- 可选自动生成，产品壳和求解边界相对清晰
- 适合当「自建蓝本」看目录怎么拆

### 缺点
- 星少、社区小，遇到坑要自己查
- 并非呼叫中心区间覆盖模型，要改造
- 对小白仍有前后端分离门槛

### 适合你用来
- 对照学习：模块怎么拆、API 大概长什么样
- 有一点基础后，可参考其领域模型

---

## 2. SirChri/employee-shift-scheduler

- 链接：https://github.com/SirChri/employee-shift-scheduler
- 语言：React + Spring Boot + PostgreSQL · 约 77★

### 优点
- 典型企业后台形态：人、班、日历
- 技术栈经典，网上教程多（尤其 Java/Spring 路线）
- 适合学 CRUD、权限、数据库表设计

### 缺点
- 自动优化弱，偏手工排班管理系统
- Spring 生态对完全小白偏重
- 不是 WFM（无 Erlang 需求层）

### 适合你用来
- Java 路线的「管理系统」参考；或只看数据表怎么设计

---

## 3. Eric-Schubert/Shiftplan

- 链接：https://github.com/Eric-Schubert/Shiftplan
- 语言：TypeScript · Docker · 星很少

### 优点
- 轮班模板、节假日、审计、Docker 自托管——**运营向功能完整**
- 适合学「周排班 / 轮转」而不是复杂优化

### 缺点
- 几乎没有呼叫中心 SLA / 区间人力概念
- 项目新、星少

### 适合你用来
- 抄轮转、节假日、审计思路

---

## 小结（C 层）

| 项目 | 推荐指数 | 建议 |
|------|----------|------|
| StaffScheduler | ★★★★ | 现代自建蓝本 |
| employee-shift-scheduler | ★★★★ | 传统后台 / 表结构参考 |
| Shiftplan | ★★★ | 轮转与运营功能参考 |

**重要：** 你自己的第一版，建议 **自己写一个极简版产品壳**（见 getting-started），开源只当对照，不要一上来大改别人仓库。
