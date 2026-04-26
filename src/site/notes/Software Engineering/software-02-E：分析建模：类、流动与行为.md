---
{"dg-publish":true,"permalink":"/software-engineering/software-02-e/","noteIcon":""}
---

# 笔记 E：分析建模：类、流动与行为

> **Architect's Note (架构师按)**： 分析建模的本质是**降维与解耦**。面对复杂的业务系统，如果我们直接让 AI 开始写代码，最终一定会得到一个高度耦合的“泥石流（Big ball of mud）”系统。通过类（静态结构）、流动（数据与控制的穿梭）和行为（动态响应）的拆解，我们实质上是在为 AI 划定清晰的**系统边界和认知上下文**。

## 模块一：基于类的建模 (Class-based Modeling) 与 CRC 思维

基于类的建模是通过将业务场景中的实体抽象为对象，来定义系统的静态结构。通常可以通过对用例文本进行“语法解析（Grammatical parse）”来识别类：名词往往是类（Classes），动词则是操作（Operations）。

- **[Core Concept]**：提取问题域的词汇表，通过定义属性（Attributes）和操作（Operations）来明确系统“有什么”和“能做什么”。
- **[Engineering Mindset]**：
    - **权衡逻辑**：这是“过度设计（Over-engineering）”与“结构简陋”的博弈。如果万物皆类，系统会因为碎片化而难以维护；如果不做抽象，所有逻辑堆在一个函数里，系统将瞬间腐化。
    - **为什么用 CRC 卡片而不是直接写代码？**：CRC（Class-Responsibility-Collaborator，类-责任-协作者）模型通过简单的索引卡片，左边写责任，右边写协作者。在卡片层面进行推演，工程师可以完全摆脱编程语言的语法干扰，纯粹地思考**关注点分离 (Separation of Concerns)**。如果一张卡片写满了责任，或者协作者过多，它在视觉上就会报警（高耦合），这比写完几千行代码后再重构要廉价得多。
- **[AI-Era Mapping]**：
    - **拒绝“上帝类 (God Class)”**：在使用 Cursor 时，大模型有一种“过度表现”的倾向，喜欢在一个文件里把所有业务逻辑写完。利用 CRC 思维，你可以在 Prompt 中严格约束：_“你的职责（Responsibility）仅仅是 X，当你需要 Y 时，严禁自己实现，必须调用协作者（Collaborator）Z。”_
    - **Multi-Agent 的底层逻辑**：CRC 是设计 Multi-Agent 系统的完美范式。一个 Class 就是一个 Agent，Responsibility 是它的 System Prompt 和专属 Tools，而 Collaborator 就是它被允许路由（Router）唤醒的其他 Agents。
- **[Memory Trigger]**：
    - **比喻**：**米其林餐厅后厨**。主厨（类）的责任是调味（Responsibility），他的协作者是切配工（Collaborator）。主厨绝不应该自己去洗菜，CRC 卡片就是后厨的岗位职责说明书。

## 模块二：流动建模 (Flow-oriented Modeling)

流动建模描述了数据和控制指令如何在系统的各个处理元素之间穿梭。它通常通过活动图（Activity Diagram）来表现并发与控制流，或通过管道-过滤器（Pipe-and-Filter）架构来表现数据的转换传递。

- **[Core Concept]**：将系统视为一系列独立的处理节点（Filters）和传输通道（Pipes），关注输入数据如何被一步步转换为输出数据。
- **[Engineering Mindset]**：
    - **权衡逻辑**：控制流与数据流分离的 Trade-off。过滤器之间必须保持独立性，它们不需要知道上下游的具体实现。如果不遵循这一原则，导致数据处理节点互相依赖内部状态，就会引发灾难性的级联故障。
- **[AI-Era Mapping]**：
    - 在构建大模型应用（如 RAG 系统）时，流动建模思维至关重要。我们可以将数据流拆解为：文档解析 -> 向量化 -> 检索 -> 过滤 -> LLM 生成。每一个节点都可以作为一个独立的函数交给 Cursor 生成，并且明确每个节点的 Input 和 Output 格式。
- **[Memory Trigger]**：
    - **比喻**：**城市自来水厂（管道与过滤器）**。从水库取水（数据源），经过沉淀池、过滤池、消毒池（独立的处理节点），最后通过水管（Pipes）送到用户家中。

## 模块三：行为建模 (Behavioral Modeling)

行为模型（Behavioral Model）揭示了系统如何响应内部或外部的事件（Events）或刺激。

- **[Core Concept]**：通过**状态图 (State Diagram)** 描述对象生命周期内的状态转换，通过**序列图 (Sequence Diagram)** 描述多个对象之间随着时间推移的消息交互序列。
- **[Engineering Mindset]**：
    - **权衡逻辑**：显式状态机与隐式状态机的博弈。状态图强制工程师定义事件（Trigger）、守卫条件（Guard, 转换必须满足的布尔条件）和动作（Action）。如果不画状态图，隐形成本是大量的 `if-else` 或 `switch` 语句散落在代码各处，导致极其难以复现的幽灵 Bug。
- **[AI-Era Mapping]**：
    - 大模型在处理复杂且长周期的业务流程（如电商订单流转：未支付 -> 已支付 -> 备货中 -> 发货）时，极易产生“幻觉”，跳过某个前置条件（Guard）。
    - 将 UML 状态图的逻辑转化为 Markdown 文本作为 Prompt 喂给 AI，并要求其使用显式的状态机库（如 XState），是保证 AI 生成代码业务健壮性的最强手段。
- **[Memory Trigger]**：
    - **比喻**：**序列图是一场交响乐演出的乐谱**（明确了什么时候该哪个乐器发声）；**状态图是汽车的自动变速箱**（只有踩下刹车（Event/Guard），才能从 D 挡（State）切换到 R 挡（State））。

---

## 特别模块：面向 AI 的 CRC 协作描述模板

为了避免 Cursor 生成高耦合的“上帝类”，我们可以将传统的 CRC 卡片理念转化为以下 **Markdown Prompt 模板**，直接用于 AI 辅助编程：

```
# [Class Definition]: PaymentProcessor (支付处理器)

## 1. Class Purpose (类目标)
本类的唯一目标是处理用户订单的支付状态流转。请严格遵循“单一职责原则 (SRP)”。

## 2. Responsibilities (自身职责 - 必须自己实现)
- 接收前端传来的支付请求与金额。
- 校验支付金额是否大于 0。
- 更新本地数据库中的支付流水状态 (Pending -> Success/Failed)。

## 3. Collaborators (协作者 - 严禁自己实现，必须调用以下协作者)
在实现上述职责时，如果遇到以下需求，**请勿在当前类中编写具体逻辑**，而是通过依赖注入 (DI) 调用相应的协作者接口：
- **[Collaborator 1]: `RiskControlService`**
  - **交互时机**：在发起第三方支付前。
  - **目的**：传递 UserID，检查是否为风险交易。
- **[Collaborator 2]: `StripeGateway`**
  - **交互时机**：金额校验通过且风控通过后。
  - **目的**：将金额和币种发给第三方网关，获取支付结果。

## 4. State Transitions (行为与状态约束 - 参照 State Diagram)
执行支付时，状态必须遵循以下流转（携带 Guard 守卫条件）：
- `INIT` -> (Event: request_pay) -> `VALIDATING`
- `VALIDATING` -> (Guard: risk_score < 90) -> `PROCESSING`
- `VALIDATING` -> (Guard: risk_score >= 90) -> `BLOCKED`

## 要求：
请基于以上 CRC 职责边界和状态转换逻辑，为我生成 `PaymentProcessor` 的代码。
```

> **架构师寄语**：通过这张“AI 版的 CRC 卡片”，你实际上是在告诉大模型：“别自作聪明把风控和第三方支付的逻辑全塞进这个类里。” 这是保证 AI 生成代码优雅、可测试、低耦合的绝佳实践。

---
下一章：[[Software Engineering/software-02-F：设计工程与体系结构\|software-02-F：设计工程与体系结构]]
首页：[[Software Engineering/Software Engineering MOC\|Software Engineering MOC]]