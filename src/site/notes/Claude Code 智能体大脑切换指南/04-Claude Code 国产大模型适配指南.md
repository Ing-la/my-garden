---
{"dg-publish":true,"permalink":"/claude-code/04-claude-code/","noteIcon":""}
---

# Claude Code 国产大模型适配指南

## 一、核心背景与工程痛点

在 AI 编程工具链中，将顶尖的 AI Agent 宿主（如 Claude Code）与极具性价比的国产大模型（如 DeepSeek、Qwen）结合，是目前开发者群体的热门探索方向。然而，这一过程伴随着“理论可行性”与“工程稳定性”的碰撞。

### 1. 理论可行（接口层）
多数顶级国产大模型都提供了与 OpenAI 或 Anthropic 兼容的 API 端点。理论上只需修改 `BASE_URL` 和 `API_KEY`，就可以让支持自定义 API 域名的客户端跑通国产模型。

### 2. 现实痛点（逻辑层与 Tool Use 层）
在实际长链 Agent 任务中，往往会出现“身体与脑子不适配”的现象：

- **Prompt 强绑定**：Claude Code 内部包含了大量专为 Claude 3.5 Sonnet 优化的复杂系统提示词（如特定的 XML 标签包裹、思考链路控制）。国产模型对此类提示词的理解深度和遵循度不一
- **工具调用（Tool Calling）漂移**：Claude Code 的 Agent 循环重度依赖极其稳定的结构化输出。国产模型在长上下文、多轮迭代中，有时会出现 JSON 尾部多逗号、工具名称拼错、参数结构轻微漂移等问题，极易导致 CLI 陷入“死循环”或直接卡死报错

---

## 二、解决方案：`cc-switch` 核心机制解析

针对上述“脑身不适配”的工程痛点，开源社区推出了 `cc-switch` 类的专用适配工具。它通过在本地构建工业级的“变速箱”，彻底解决了手动 hack 的不稳定性。

### 1. 定位
一个集成了**本地反向代理（Local Reverse Proxy）** 与**多 CLI 统一配置管理**的桌面/终端工具。

### 2. 核心工作原理
- **环境与认证托管**：自动接管环境变量注入，并在后台静默处理 Claude Code 的登录与新手引导（Onboarding）拦截，免去用户手动修改 `.claude.json` 或 `$PROFILE` 的繁琐步骤
- **协议级“实时翻译”**：
  - 当 Claude Code 发出包含复杂 XML 标签和 Anthropic 专属 `tool_use` 格式的请求时，`cc-switch` 会在本地拦截流量，将其动态重组并翻译为国产大模型最擅长的、标准的 OpenAI 格式
  - 当国产大模型返回结果时，`cc-switch` 会对可能存在的格式瑕疵（如多余的逗号、非标准的 JSON）进行矫正，再将其伪装成完美的 Anthropic 原生返回包喂回给 Claude Code
- **无缝热切换**：允许用户在不重启终端或不修改任何配置文件的情况下，通过系统托盘或简单指令在不同脑子（DeepSeek、Qwen、原生 Claude）之间一键切换

### 3. 安装与部署流程（详细步骤）

#### 步骤 1：获取 cc-switch 客户端
- **GitHub 仓库**：访问 [cc-switch 官方仓库](https://github.com/cc-switch/cc-switch) 获取最新版本
- **下载链接**：
  - Windows：下载 `.msi` 或 `.exe` 安装包 [最新发布页面](https://github.com/cc-switch/cc-switch/releases/latest)
  - macOS：下载 `.dmg` 安装包
  - Linux：下载 `.AppImage` 或使用包管理器安装
- **备用下载**：如果 GitHub 访问困难，可通过 [GitHub Proxy](https://ghproxy.com/) 加速下载

#### 步骤 2：安装与初始配置
1. **运行安装程序**：双击下载的安装包，按照向导完成安装
2. **启动 cc-switch**：安装完成后，在系统托盘（Windows）或菜单栏（macOS）找到 cc-switch 图标
3. **基本设置**：
   - 点击系统托盘图标 → "Settings"
   - 确保 "Auto-start with system" 已启用
   - 检查 "Proxy Port"（默认通常为 8080）

#### 步骤 3：添加模型供应商（Provider）
1. **打开供应商管理**：点击系统托盘图标 → "Manage Providers"
2. **添加新供应商**：点击 "Add Provider" 按钮
3. **配置供应商信息**：
   - **Name**：输入供应商名称（如 "DeepSeek"）
   - **Base URL**：填写 API 地址（如 `https://api.deepseek.com/v1`）
   - **API Format**：选择 "OpenAI"（cc-switch 会自动转换为 Anthropic 格式）
4. **模型映射配置**：
   - **Sonnet / Haiku / 主模型**：填写模型 ID（如 `deepseek-chat`）
   - **Opus / 推理模型**：填写推理模型 ID（如 `deepseek-reasoner`）
5. **保存配置**：点击 "Save" 完成供应商添加

#### 步骤 4：配置 Claude Code 使用 cc-switch

根据实际使用经验，cc-switch 通常会自动处理 Claude Code 的配置，用户可能无需手动设置环境变量。以下是两种可能的配置方式：

##### 方式一：自动配置（推荐）
1. **启动 cc-switch**：确保 cc-switch 在后台运行
2. **启动 Claude Code**：直接运行 `claude` 命令
3. **自动检测**：cc-switch 会自动拦截 Claude Code 的请求并重定向到配置的模型

##### 方式二：手动配置（备用方案）
如果自动配置不生效，可以尝试手动设置环境变量：

1. **获取代理信息**：
   - 在 cc-switch 界面中查看代理地址（通常为 `http://localhost:8080`）
   - 获取 cc-switch 生成的虚拟 API Key（如果需要）

2. **设置环境变量**（可选）：
   ```bash
   # Windows PowerShell
   $env:ANTHROPIC_BASE_URL="http://localhost:8080"  # cc-switch 代理地址
   # 如果 cc-switch 需要虚拟 Key
   $env:ANTHROPIC_AUTH_TOKEN="cc-switch提供的虚拟Key"
   
   # Linux/macOS
   export ANTHROPIC_BASE_URL="http://localhost:8080"
   export ANTHROPIC_AUTH_TOKEN="cc-switch提供的虚拟Key"
   ```

3. **验证连接**：运行 `claude --version` 确认 Claude Code 能正常启动

**重要提示**：根据用户反馈，大多数情况下只需安装并运行 cc-switch，无需手动设置环境变量。如果 Claude Code 无法连接，再尝试手动配置。

#### 步骤 5：测试与切换
1. **测试模型响应**：运行简单命令如 `claude "Hello"` 检查是否正常响应
2. **切换模型**：在 cc-switch 系统托盘菜单中直接选择不同的供应商
3. **热切换验证**：切换后无需重启终端，直接测试新模型

#### 注意事项
- **端口冲突**：如果 8080 端口被占用，可在 cc-switch 设置中修改端口号
- **防火墙设置**：确保防火墙允许 cc-switch 和 Claude Code 的网络通信
- **自动配置优先**：优先尝试 cc-switch 的自动配置，大多数情况下无需手动设置环境变量


## 三、操作红线与最佳实践

1. **禁止留空**：在 `cc-switch` 中配置供应商时，必须填满 Sonnet/Haiku/Opus 映射，否则 CLI 握手会报错
2. **管理员运行**：在 Windows 上修改 `claude.exe` 路径或环境变量时，PowerShell 必须具备管理员权限
3. **独立管理**：不要在同一个厂家配置里反复切换映射。正确的做法是：**每一个大脑对应一个独立的 Provider 卡片**

---

## 四、选型决策树（工程实用建议）

- **若追求绝对的确定性与大项目重构**：建议使用 Aider、Cursor、Continue 等。这类工具从底层设计之初就是“多模型兼容”哲学，具备更成熟的跨模型 Fallback（容错）机制
- **若追求极致的性价比与轻量级任务**：使用 `cc-switch` + Claude Code + 国产官方 API。享受最顶级的 Agent 交互体验和低廉的 Token 成本，但需接受极少数复杂长链任务中可能出现的抽风
- **若专注于极速修 Bug 或对成本不敏感**：坚持使用 Claude Code 原生身体 + Claude 3.5 Sonnet 原生大脑，不进行任何协议层干预

---

## 五、附录：Cursor + DeepSeek 深度集成（2026 官方标准版）

> **⚠️ 重要更新**：由于 Cursor 官方在 2026 年对模型配置系统进行了重大调整，以下传统配置方法可能已不再适用。建议优先使用 `cc-switch` 方案实现 Claude Code 的国产模型适配。本附录仅作为历史参考。

### 1. 核心配置步骤（传统方法）
1. **进入模型设置**：Cursor 右上角齿轮图标 → Settings → Models
2. **配置 OpenAI 协议通道**：
   - **API Key 填写**：展开 `OpenAI API Key`，填入 DeepSeek Key
   - **Base URL 劫持**：开启 `Override OpenAI Base URL`，填入 `https://api.deepseek.com/v1`
3. **激活 DeepSeek 大脑**：在 `Add Custom Model` 区域手动添加：
   - **`deepseek-chat`**：主力模型，用于日常对话和 Composer 代码修改
   - **`deepseek-reasoner`**：推理模型（R1 系列），用于攻克逻辑死角

### 2. 价格参考（2026 市场价）
- **原生 Claude 3.5 Sonnet**：约 20-100 元人民币 / 百万 Token
- **DeepSeek V3.2 / V4**：约 **0.2 - 1.5 元人民币** / 百万 Token
- **结论**：用 1/50 的价格，实现 95% 的编程效果

### 3. 最佳实践建议
> **“田忌赛马”式开发方案：**
> - **日常搬砖**：用 **`deepseek-chat`**
> - **逻辑死磕**：换 **`deepseek-reasoner`**
> - **极致审美/UI**：偶尔切回原版 **`Claude 3.5 Sonnet`**

---

## 六、下一步
掌握国产大模型适配后，可深入学习 [进阶配置与多脑并存](./05-Claude%20Code%20进阶配置与多脑并存.md) 实现模型资源的最优调度。