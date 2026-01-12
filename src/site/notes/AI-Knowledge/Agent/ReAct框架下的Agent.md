---
{"dg-publish":true,"permalink":"/AI-Knowledge/Agent/ReAct框架下的Agent/","noteIcon":""}
---

# 知识文档：深入理解 ReAct 框架 Agent

## 1. 什么是 ReAct 框架？
**ReAct** 是 **Reasoning (推理)** 与 **Acting (行动)** 的缩写，由 Google 研究人员于 2022 年提出。

在传统的 Prompt 工程中，LLM 要么只进行逻辑推理（如 Chain of Thought），要么只进行工具调用（如 Action Generation）。ReAct 的精髓在于**将两者结合**：LLM 在执行任务时，会交替生成“推理轨迹”和“特定任务的行动”。

> **核心公式：** ReAct = Chain of Thought (CoT) + Action Plan
---

## 2. ReAct 的运行逻辑 (The Loop)

ReAct Agent 的工作流程是一个闭环状态机，通常包含以下四个步骤的循环：

1.  **Thought (思考)**：LLM 描述当前情况，计划下一步该做什么，并决定调用哪个工具。
2.  **Action (行动)**：LLM 输出一个预定义的指令（如 `call_search("Python")`），主程序捕获该指令。
3.  **Observation (观察)**：主程序执行工具，并将结果（API 返回值、数据库内容等）反馈给 LLM。
4.  **Final Answer (最终回复)**：当 LLM 判断信息足够时，跳出循环，给出人类可读的答案。
---

## 3. 设计核心：Prompt 与 脚本的协作

要让 ReAct 跑通，必须实现 **Prompt 设计**与**主程序逻辑**的深度解耦与配合。

### A. Prompt 层：构建“思维框架”
Prompt 必须强制 LLM 遵守固定的输出格式。一个典型的 ReAct 系统提示词（System Prompt）结构如下：

> **示例 Prompt 结构：**
> - **Available Tools:** 列出所有可用工具及其参数说明。
> - **Format Instructions:** 强制要求输出格式：
>   - `Thought: <推理过程>`
>   - `Action: <工具名>[参数]`
>   - `Observation: <工具返回的结果>` (这部分由程序填充)
>   - `... (重复)`
>   - `Final Answer: <最终结论>`

### B. 程序层：构建“执行引擎”
主程序脚本（Python/Node.js）承担着“手脚”的功能，逻辑如下：

1.  **解析逻辑**：使用正则表达式或 JSON 解析器，提取 LLM 输出中 `Action:` 之后的内容。
2.  **中断机制**：一旦解析到 `Action`，立即停止 LLM 的继续生成，转而执行本地代码。
3.  **上下文拼接**：将执行结果以 `Observation: XXX` 的格式拼接回对话历史。

---

## 4. 为什么 ReAct 优于纯推理或纯动作？

| 维度 | 纯推理 (CoT) | 纯动作 (Tool Use) | ReAct 框架 |
| :--- | :--- | :--- | :--- |
| **准确性** | 容易产生事实性幻觉 | 缺乏逻辑，难以处理复杂意图 | 通过观察实时修正逻辑 |
| **透明度** | 过程可见，但无法交互 | 过程不可见，仅有结果 | 整个思维链条清晰可见 |
| **容错率** | 一步错步步错 | 无法通过逻辑判断错误 | 发现 Observation 不符合预期时能自我修正 |

---

## 5. 极简伪代码实现 (Python 逻辑演示)

```python
def run_agent(user_query):
    # 1. 初始 Prompt 包含工具描述和任务
    context = system_prompt + user_query
    
    while True:
        # 2. 调用 LLM (设定在遇到 "Observation:" 时停止生成)
        response = llm.generate(context, stop_sequences=["Observation:"])
        context += response
        
        # 3. 判断是否完成
        if "Final Answer:" in response:
            return extract_final_answer(response)
        
        # 4. 提取 Action 并执行
        action_call = extract_action(response)
        observation = execute_tool(action_call)
        
        # 5. 将结果喂回 LLM
        context += f"\nObservation: {observation}\n"
     
```

---

##   6. 避坑指南与优化建议

- **防止死循环**：务必在脚本中设置 `max_iterations`（例如 5 次），防止 Agent 陷入无休止的搜索。
    
- **处理解析失败**：如果 LLM 输出格式不对，脚本应返回一个 Observation 告诉 LLM：“你的格式有误，请重新按照 Action: [tool_name] 的格式输出”。
    
- **状态同步**：确保 LLM 知道它是在一个多轮对话中，每一轮 Observation 都是它“亲眼所见”的实时事实。
    

---

**结语：** ReAct 的精髓在于**“透明的思考”**与**“受控的行动”**。它是目前构建生产级 Agent（如 LangChain、AutoGPT）最基础也最有效的底层思维模型。