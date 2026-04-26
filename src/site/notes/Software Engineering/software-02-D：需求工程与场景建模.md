---
{"dg-publish":true,"permalink":"/software-engineering/software-02-d/","noteIcon":""}
---

# 笔记 D：需求工程与场景建模

> **Architect's Note (架构师按)**： 别以为敏捷开发或者有了大模型就不需要写需求了。如果需求是一团乱麻，AI 只会用光速为你生成一堆毫无用处的垃圾代码。用工程化的方法把模糊的业务意图转化为结构化的场景模型，是你驾驭 AI 替你干活的前提。

## 模块一：需求工程的七个阶段 (The 7 RE Tasks)

需求工程 (Requirements Engineering) 不是一次性写完的长篇大论，而是一个闭环的迭代过程，旨在建立设计与构建的坚实基础。它包含七个边界有时会重叠的核心任务：

1. **起始 (Inception)**：明确业务需求或市场机会，识别利益相关者 (Stakeholders)，建立对问题的初步理解。
2. **启发 (Elicitation)**：挖掘用户的真实目标。这是一个极度困难的过程，因为各方视角 (Viewpoints) 不同，且可能存在冲突。
3. **精化 (Elaboration)**：将启发阶段收集的用户场景转化为细化的需求模型，提取分析类 (Analysis Classes) 并定义其属性与服务。
4. **协商 (Negotiation)**：当需求与现实资源冲突时，平衡功能、性能与成本、时间，达成双赢的妥协。
5. **规格说明 (Specification)**：将需求落实为电子文档、数学模型或用例集，作为后续开发的基础。
6. **验证 (Validation)**：通过正式技术评审 (FTR)，检查需求是否含糊、遗漏、前后矛盾或不可测试。
7. **需求管理 (Requirements Management)**：控制和追踪贯穿整个项目生命周期的需求变更。

## 模块二：需求类型的辨析

- **功能需求 (Functional Requirements)**：系统必须执行的操作和业务逻辑（例如：用户登录、加入购物车）。
- **非功能需求 (Non-functional Requirements, NFRs)**：系统的质量属性 (Quality Attributes) 或约束条件，如可靠性、可扩展性、安全性、性能等。NFR 往往容易被忽视，但缺乏 NFR 的系统注定无法在生产环境存活。

## 模块三：场景建模 (Scenario-based Modeling)

场景建模是从用户（参与者）视角描述系统的主要手段，它是最容易被非技术人员理解的模型。

1. **用例 (Use Cases)**：描述最终用户（扮演特定**参与者 Actor** 角色）在特定情况下与系统交互的“剧本”。一个严密的用例不仅包含主场景 (Primary Scenario)，还必须穷举可能的异常或替代流 (Exceptions/Secondary Scenarios)。
2. **活动图 (Activity Diagram)**：UML 中类似流程图的工具，用于表示特定场景内的交互流或处理细节，它特别擅长处理复杂的系统行为和并发活动 (Concurrent flows)。
3. **泳道图 (Swimlane Diagram)**：活动图的变体，它通过垂直划分“泳道”，清晰地界定了哪个参与者 (Actor) 或分析类 (Analysis Class) 负责执行哪一个特定的操作 (Responsibilities)。

---

## 核心思维维度

### [Core Concept]

需求建模的本质是消除信息不对称，通过结构化抽象将系统的“熵值”（混乱度）在敲下第一行代码前降到最低。

### [Engineering Mindset]

- **权衡逻辑**：这是在“过早进入细节 (Premature Detail)”与“需求模糊 (Ambiguity)”之间的 Trade-off。过早陷入细节会导致“分析瘫痪 (Analysis Paralysis)”和巨大的变更沉没成本；但如果不做细致的异常分支思考，模糊的需求将带来灾难。
- **隐形成本的倍率效应**：软件工程中存在**缺陷放大 (Defect Amplification)** 现象。根据行业数据，在编码阶段修复一个缺陷的成本如果是 $977，在系统测试阶段发现并修复它的成本将飙升至 $7,136（约 8 倍），如果遗漏到发布后（维护阶段），修复成本将高达 $14,102（约 15 倍）。不做需求验证，就是给后期的自己和公司挖坑。

### [AI-Era Mapping]

- **传统用例向 Agent 任务流的演变**：在 AI 时代，书中的 Actor（参与者）可以直接映射为不同职能的 AI Agents。Swimlane（泳道图）完美对应了多 Agent 协作系统（Multi-Agent System）中，Manager Agent 与 Worker Agents 的任务交接过程。
- **给 Cursor 写 Prompt 的降维打击**：大多数人写 Prompt 是口语化的“帮我写个登录页面”。如果你借用需求工程的思想来写 `requirements.md`，你会定义好 _Preconditions (前置条件)_、_Trigger (触发事件)_、_Primary Flow (主操作流)_ 以及最重要的 _Exceptions (异常处理边界)_。结合明确的 NFRs（如响应时间、使用的 UI 库限制），Cursor 生成的代码不仅能跑，而且具备极高的鲁棒性。

### [Memory Trigger]

- **比喻**：需求工程和建模就像**拍电影的前期筹备**。起始和启发是“拉投资和定题材”，精化是“写剧本”（Use Cases），活动图和泳道图是导演画的“分镜头脚本”（指明了机位、演员走位、道具配合）。没有分镜头脚本，直接让演员（AI 编程助手）自由发挥，最后剪辑出来的一定是逻辑不通的烂片。

---

## 特别模块：架构师的 System Prompt 转化模板

书中的标准 Use Case 包含：目标 (Goal)、前置条件 (Precondition)、触发器 (Trigger)、主场景 (Scenario) 和异常 (Exceptions)。作为 AI 时代的架构师，我们可以将这种结构完美转化为给 AI Agent（或 Cursor）执行的 System Prompt 指令：

```
# [Agent Role / System Prompt]: 订单结算专家

## 1. Goal (目标)
你现在的角色是处理电商订单结算的核心引擎。请根据以下结构化的场景描述，生成/执行相关的业务逻辑代码。

## 2. Preconditions (前置条件/上下文)
- 用户已登录，且具有有效的 JWT Token。
- 购物车 (Cart) 对象不为空，且包含至少一件校验过库存的商品。
- 系统处于“等待结算”状态 (State: Pending_Checkout)。

## 3. Trigger (触发器)
- 接收到 API 请求: `POST /api/v1/checkout`

## 4. Scenario (主干任务流 - 类似 Activity Diagram)
请按以下步骤严格执行/生成代码逻辑：
Step 1. 验证用户 JWT Token 的时效性。
Step 2. 锁定购物车中商品的数据库库存 (排他锁)。
Step 3. 计算订单总价（遍历商品单价 * 数量）。
Step 4. 检查是否有适用的优惠券 (Coupon)，如有则扣减相应金额。
Step 5. 生成支付流水号，调用第三方支付网关。

## 5. Exceptions (异常处理/边界护栏)
如果发生以下情况，请严格按照约束执行 (Guardrails)：
- [Exception 1]: 如果库存锁定失败 (库存不足)，立即释放已锁定的其他商品，返回 HTTP 409 (Conflict)，并提示“部分商品被抢空”。
- [Exception 2]: 如果第三方支付网关超时 (> 2秒)，重试一次；若仍失败，标记订单为 `Payment_Pending`，返回 HTTP 504。

## 6. NFRs (非功能性需求/架构约束)
- [性能]: 整个结算接口响应时间必须 < 500ms。
- [安全]: 支付金额字段必须使用 `Decimal` 类型，严禁使用浮点数避免精度丢失。
- [日志]: 所有失败的步骤必须记录详细的 TraceID 到 Elasticsearch。
```

> 通过上述模板，你实质上是在用严谨的需求规格说明 (Requirements Specification) 来约束不可预测的大模型。将这份笔记运用到日常的 Cursor 交互中，你会发现 AI 的输出质量将产生质的飞跃。

---
下一章：[[Software Engineering/software-02-E：分析建模：类、流动与行为\|software-02-E：分析建模：类、流动与行为]]
首页：[[Software Engineering/Software Engineering MOC\|Software Engineering MOC]]