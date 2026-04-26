---
{"dg-publish":true,"permalink":"/software-engineering/software-engineering-moc/","noteIcon":""}
---

![Software Engineering cover.png](/img/user/Software%20Engineering/Software%20Engineering%20cover.png)
# 🏗️ 软件工程：实践者的研究方法 · 知识地图

> 本文档是 **《软件工程：实践者的研究方法（第 9 版）》** 的知识索引地图（Map of Content），系统梳理了该著作的核心框架与知识脉络，涵盖从软件过程、建模方法、质量保障到项目管理的完整体系，并融入了 AI 时代下的工程实践映射。

---

## 🙏 致谢与致敬

本知识体系源自以下经典著作：

**《Software Engineering: A Practitioner's Approach, 9th Edition》**
**《软件工程：实践者的研究方法（第 9 版）》**

- **作者：** Roger S. Pressman 博士 & Bruce R. Maxim 教授
- **出版社：** McGraw-Hill Education
- **出版年份：** 2019 年（第 9 版）

这本书是软件工程领域最具影响力的经典教材之一，三十余年来持续引领全球软件工程师的思维范式。Pressman 博士以深厚的工程功底和清晰的系统思维，构建了一个贯穿"过程 — 建模 — 质量 — 管理 — 进阶"的完整知识体系。第 9 版由 Bruce R. Maxim 教授共同更新，融入了敏捷开发、安全工程、DevOps 等现代实践，使这部经典在 AI 时代依然焕发着强大的生命力。

本知识体系在整理过程中，同步映射了 AI 辅助编程（如 Cursor、GitHub Copilot）和 LLM Agent 等前沿技术对传统软件工程的冲击与重塑——这既是对经典的致敬，也是对未来的回应。

> *"软件工程的目标不是构建软件，而是构建能够持续交付价值的系统。"* —— Roger S. Pressman

---

## 📑 知识体系结构

本知识体系由 **1 份知识地图（本文档）** 和 **16 份核心知识笔记** 组成，遵循原书的五大篇章结构，按"过程 → 建模 → 质量 → 管理 → 进阶"的逻辑递进。

### 核心文档导航

| 篇章 | 核心内容 |
|------|---------|
| **Part 1：软件过程** | 软件本质、过程模型、敏捷开发与 AI 时代的反馈闭环 |
| **Part 2：建模** | 需求工程、分析建模、体系结构设计、组件级与人机交互设计 |
| **Part 3：质量与安全** | 质量保证、测试策略、安全工程与防御性设计 |
| **Part 4：项目管理** | 量化度量、估算排期、风险管理与软件演进 |
| **Part 5：高级课题** | 过程改进与 CMMI、新兴趋势、职业伦理与终身成长 |

---

## 🗺️ 知识地图总览

### Part 1：软件过程（Software Process）—— 工程基石

**核心文档：**
- [[Software Engineering/software-01-A：软件工程的底层逻辑与分层体系\|software-01-A：软件工程的底层逻辑与分层体系]]
- [[Software Engineering/software-01-B：过程模型对比与演进策略\|software-01-B：过程模型对比与演进策略]]
- [[Software Engineering/software-01-C：敏捷开发与人本思维\|software-01-C：敏捷开发与人本思维]]

**关键脉络：**

| 节点 | 核心洞察 |
|------|---------|
| 软件的本质 | 软件退化论（Deterioration vs. Wear-out）—— 变更加速退化的根本原因 |
| 分层技术模型 | 质量焦点 → 过程 → 方法 → 工具，四层递进不可僭越 |
| 过程框架 | 沟通 → 策划 → 建模 → 构建 → 部署 + 普适性活动 |
| 过程模型权衡 | 瀑布 / 增量 / 原型 / 螺旋 —— 架构师的决策情境表 |
| 敏捷与 Scrum | 敏捷宣言 2.0、Backlog、Sprint、每日站会、回顾 |
| AI-Era 映射 | 构建层被压缩，架构师价值向上游（需求/建模）和全局（质量/配置）转移 |

---

### Part 2：建模（Modeling）—— 系统之魂

**核心文档：**
- [[Software Engineering/software-02-D：需求工程与场景建模\|software-02-D：需求工程与场景建模]]
- [[Software Engineering/software-02-E：分析建模：类、流动与行为\|software-02-E：分析建模：类、流动与行为]]
- [[Software Engineering/software-02-F：设计工程与体系结构\|software-02-F：设计工程与体系结构]]
- [[Software Engineering/software-02-G：从微观组件到人机交互的艺术\|software-02-G：从微观组件到人机交互的艺术]]

**关键脉络：**

| 节点 | 核心洞察 |
|------|---------|
| 需求工程 | 7 项需求任务： elicit → analyze → specify → validate → manage |
| 用例建模 | Actor + 场景 + 用例关系，向 System Prompt 的转化映射 |
| 分析建模 | CRC 建模、状态机、序列图 —— 多 Agent 系统的角色定义蓝图 |
| 体系结构 | 五大风格（数据流、调用返回、独立组件、虚拟机、数据仓）+ 架构权衡 |
| 组件级设计 | 内聚与耦合、信息隐蔽、契约式设计（Design by Contract） |
| 人机交互 | UI 黄金三法则（用户操纵、减轻记忆、一致性）、展示逻辑与业务逻辑分离 |
| AI-Era 映射 | 分析模型 → Agent 思维链定义；体系结构 → Multi-Agent 编排拓扑 |

---

### Part 3：质量与安全（Quality & Security）—— 工程师的尊严

**核心文档：**
- [[Software Engineering/software-03-H：质量保证与评审艺术\|software-03-H：质量保证与评审艺术]]
- [[Software Engineering/software-03-I：测试策略与实战战术\|software-03-I：测试策略与实战战术]]
- [[Software Engineering/software-03-J：安全工程与防御性设计\|software-03-J：安全工程与防御性设计]]

**关键脉络：**

| 节点 | 核心洞察 |
|------|---------|
| 质量保证 | V&V 辨析（Validation vs. Verification）、评审的成本效应曲线 |
| SQA 体系 | 普适性活动视角下的质量基础设施 |
| 测试策略 | V 模型映射：单元 → 集成 → 系统 → 验收，逐层递进 |
| 白盒与黑盒 | 逻辑覆盖（语句/分支/路径）、等价类划分、边界值分析 |
| 安全工程 | CIA 三要素（机密性、完整性、可用性）、威胁建模（STRIDE） |
| 安全左移 | Shift-Left 实践：在需求/设计阶段引入安全考量 |
| AI-Era 映射 | AI 生成代码的评审新课题 —— 谁为幻觉引发的安全漏洞负责？ |

---

### Part 4：项目管理（Managing Software Projects）—— 掌控节奏

**核心文档：**
- [[Software Engineering/software-04-K：管理要素与量化度量\|software-04-K：管理要素与量化度量]]
- [[Software Engineering/software-04-L：估算、排期与进度掌控\|software-04-L：估算、排期与进度掌控]]
- [[Software Engineering/software-04-M：风险管理与软件演进\|software-04-M：风险管理与软件演进]]

**关键脉络：**

| 节点 | 核心洞察 |
|------|---------|
| 4P 要素 | People + Product + Process + Project —— 管理者的四维决策空间 |
| 软件度量 | 面向规模（LOC）、面向功能（FP）、面向对象（Use-Case）度量 |
| 估算方法 | FP 估算法、LOC 转换、经验模型（COCOMO II） |
| 进度控制 | WBS、关键路径法（CPM）、挣值分析（Earned Value） |
| 风险管理 | RMMM 计划（识别 → 分析 → 应对 → 监控） |
| 软件演进 | 技术债务、逆向工程、重构、再工程策略 |
| AI-Era 映射 | AI 辅助估算与风险评估、自动化 WBS 生成、AI 驱动的配置管理 |

---

### Part 5：高级课题（Advanced Topics）—— 视野与格局

**核心文档：**
- [[Software Engineering/software-05-N：过程改进与能力成熟度 (SPI & CMMI)\|software-05-N：过程改进与能力成熟度 (SPI & CMMI)]]
- [[Software Engineering/software-05-O：新兴趋势与 AI 增强型工程\|software-05-O：新兴趋势与 AI 增强型工程]]
- [[Software Engineering/software-05-P：职业伦理与架构师的终身成长\|software-05-P：职业伦理与架构师的终身成长]]

**关键脉络：**

| 节点 | 核心洞察 |
|------|---------|
| SPI 框架 | 评估 → 教育 → 创建 → 试点 → 部署 → 持续，消除对英雄主义的依赖 |
| CMMI 五级 | 初始级 → 可管理级 → 已定义级 → 量化管理级 → 优化级 |
| 新兴趋势 | 云原生、微服务、百万亿级系统、开放世界软件（Open-World Software） |
| AI 增强工程 | LLM 辅助需求分析、AI 驱动的测试生成、自我修复系统 |
| 职业伦理 | ACM/IEEE 八项原则：公众 > 客户 > 产品 > 判断 > 管理 > 职业 > 同事 > 自我 |
| 架构师成长 | 从 Coder 到 Architect 的认知跃迁 —— 领导力、沟通、终身学习 |
| AI-Era 映射 | AI 伦理困境（算法偏见、数据隐私）、AI 时代的职业责任边界 |

---

## 📚 使用建议

**推荐阅读路径：** Part 1 → Part 2 → Part 3 → Part 4 → Part 5

1. **理解全局**：先阅读本 MOC 掌握全貌
2. **地基先行**：从 Part 1 理解软件工程底层逻辑和过程模型选择
3. **建模为核心**：学习 Part 2 掌握从需求到设计的完整转化链路
4. **质量为底线**：通过 Part 3 建立质量保障与安全防御意识
5. **管理为杠杆**：学习 Part 4 掌握估算、排期与风险控制
6. **视野为天花板**：阅读 Part 5 理解过程改进、AI 趋势与职业伦理
7. **查阅原文**：如需原始完整内容，可参阅原著《Software Engineering: A Practitioner's Approach, 9th Edition》

---

## 🎯 本知识体系特色

- ✅ **忠于原著**：完整保留原书的五大部分核心框架与关键概念，不遗漏有价值的内容
- ✅ **AI 时代映射**：每篇笔记均包含 AI-Era Mapping 模块，将经典理论映射到 AI 辅助编程与 LLM Agent 时代
- ✅ **逻辑分层**：按"过程 → 建模 → 质量 → 管理 → 进阶"递进式组织，符合认知规律
- ✅ **清晰导航**：MOC 文档提供全局视角，笔记间通过 `[[]]` 互相关联
- ✅ **认知深度**：每份笔记均包含核心概念、工程思维（权衡逻辑 + 隐形成本）、AI 映射与记忆触发器
- ✅ **实践驱动**：强调从 Coder 到 Architect 的认知跃迁，聚焦工程决策力的培养

---

*基于《Software Engineering: A Practitioner's Approach, 9th Edition》整理*
*原作者：Roger S. Pressman 博士 & Bruce R. Maxim 教授 (McGraw-Hill Education)*
*整理日期：2026年4月24日*
*笔记数量：16 份核心知识笔记 + 1 份知识地图*

---
[[Home\|Home]]
