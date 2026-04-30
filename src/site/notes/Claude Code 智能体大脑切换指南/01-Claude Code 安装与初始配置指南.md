---
{"dg-publish":true,"permalink":"/claude-code/01-claude-code/","noteIcon":""}
---

# Claude Code 安装与初始配置指南

## 一、前置环境准备

### 1. PowerShell 安装

Claude Code 的安装脚本和日常操作依赖 PowerShell。如果你的系统尚未安装，请按以下步骤操作：

#### 检测是否已安装
打开任意命令行（cmd），输入：
```powershell
powershell -Command "$PSVersionTable.PSVersion"
```
- 如果显示版本号（如 `Major Minor Build Revision`），说明已安装，跳过此节
- 如果提示"不是内部或外部命令"，则继续以下安装步骤

#### 下载与安装
1. **访问官方下载页**：[PowerShell GitHub Releases](https://github.com/PowerShell/PowerShell/releases/latest)
2. **选择对应安装包**：
   - Windows 10/11 64位：下载 `PowerShell-<版本号>-win-x64.msi`
   - Windows 10/11 32位：下载 `PowerShell-<版本号>-win-x86.msi`
   - Windows 7/Server：下载 `PowerShell-<版本号>-win-<架构>.msi`（注意 Windows 7 需安装 WMF 5.1 前置补丁）
3. **运行安装程序**：双击 `.msi` 文件，一路点击"Next"，建议勾选"Add to PATH"
4. **重启终端**：安装完成后，重新打开命令行

#### 验证安装
```powershell
powershell -Command "$PSVersionTable.PSVersion"
```
正常应显示类似以下输出（版本号以实际为准）：
```
Major  Minor  Build  Revision
-----  -----  -----  --------
7      4       xxxx  xxxx
```

#### 常见问题
- **Windows 7 安装失败**：需先安装 [WMF 5.1](https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/wmf/setup/install-configure) 和 KB 补丁
- **安装后仍找不到命令**：尝试注销后重新登录，或者手动将 `C:\Program Files\PowerShell\7\` 添加到系统 PATH

---

### 2. Git 安装

Claude Code 在文件编辑、代码生成和项目管理中重度依赖 Git。如果你的系统尚未安装，请按以下步骤操作：

#### 检测是否已安装
打开 PowerShell，输入：
```powershell
git --version
```
- 如果显示版本号（如 `git version 2.45.0.windows.1`），说明已安装，跳过此节
- 如果提示"无法将'git'识别为 cmdlet"，则继续以下安装步骤

#### 下载与安装
1. **访问官方下载页**：[https://git-scm.com/download/win](https://git-scm.com/download/win)
   - 系统会自动推荐对应架构的版本（64位或32位）
2. **运行安装程序**：双击下载的 `.exe` 文件，关键配置项建议如下：
   - **Select Components**：默认勾选即可，建议勾选"Add a Git Bash Profile to Windows Terminal"（如有）
   - **Choosing the default editor**：选择你习惯的编辑器（如 VS Code、Notepad++ 等）
   - **Adjusting your PATH environment**：选择 **"Git from the command line and also from 3rd-party software"**（确保在 PowerShell 中可直接使用 git 命令）
   - **Choosing HTTPS transport backend**：选 "Use the native Windows Secure Channel library"
   - **Configuring the line ending conversions**：选 **"Checkout as-is, commit as-is"**（避免跨平台换行符问题）
   - **其他选项**：保持默认即可
3. **完成安装**：点击"Install"，等待完成

#### 验证安装
```powershell
git --version
```
正常应显示：
```
git version 2.45.0.windows.1
```

#### 初次使用配置（必做）
安装后需配置用户名和邮箱（用于 Git 提交记录）：
```powershell
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

#### 常见问题
- **安装后 PowerShell 找不到 git**：检查 C:\Program Files\Git\cmd 是否在系统 PATH 中，或重启终端
- **需要代理**：在 PowerShell 中设置 `git config --global http.proxy http://127.0.0.1:7897`

---

## 二、安装 Claude Code

### 1. 卸载旧版本（如适用）
官方已停止对 npm 版本的更新支持，全面转向原生二进制版本。若之前通过 npm 安装，请先卸载：

```powershell
npm uninstall -g @anthropic-ai/claude-code
```

### 2. 安装原生版本（Windows 示例）
使用官方安装脚本：

```powershell
irm https://claude.ai/install.ps1 | iex
```

### 3. 解决安装网络瓶颈
由于安装脚本需从海外服务器拉取二进制包，直连可能卡顿或失败。可在当前 PowerShell 窗口临时设置代理：

```powershell
$env:HTTP_PROXY="http://127.0.0.1:7897"
$env:HTTPS_PROXY="http://127.0.0.1:7897"
```
（将 `7897` 替换为本地代理软件实际监听端口）

**原理**：`$env:` 前缀表示临时环境变量，仅当前窗口生效，关闭后自动销毁。此配置将网络请求重定向至本地代理，实现精准加速。

### 4. 验证安装
重新打开终端，运行：
```powershell
claude doctor
```
确保所有检查通过，无路径冲突。

---

## 三、认证绕过与 API Key 配置

### 1. 底层逻辑：为什么能绕过登录？
Claude Code 代码中写死：**只要检测到 `ANTHROPIC_API_KEY` 环境变量，就直接走 API 计费通道，不再检查浏览器登录状态**。设置 Key 即实现“绕过”。

### 2. 手动配置 API（中转站方式）

> **适用场景**：通过第三方中转站或兼容 Anthropic API 的服务使用 Claude Code。需同时配置 **API Key** 和 **Base URL**。
>
> 若使用 `cc-switch` 进行换脑（见第四、五文档），则无需手动配置环境变量，cc-switch 会自动处理。

#### 2.1 配置 Base URL（API 端点）
将请求地址指向中转站或兼容服务的地址：

```powershell
$env:ANTHROPIC_BASE_URL = "https://your-proxy-url.com"
```

#### 2.2 配置 API Key
将密钥写入 PowerShell 的 Profile 文件，实现每次启动终端自动设置：

1. 打开 Profile 文件：
   ```powershell
   notepad $PROFILE
   ```

2. 添加以下两行（将值替换为实际的 URL 和 Key）：
   ```powershell
   $env:ANTHROPIC_BASE_URL = "https://your-proxy-url.com"
   $env:ANTHROPIC_API_KEY = "your_api_key_here"
   ```

3. 使配置立即生效：
   ```powershell
   . $PROFILE
   ```

### 3. 跳过新手引导（Onboarding）
Claude Code 首次运行会检查 `hasCompletedOnboarding` 标志。手动修改记忆文件可跳过引导：

1. 打开配置文件：
   ```powershell
   notepad $HOME\.claude.json
   ```

2. 确保包含以下字段（若文件为空或不存在，可创建）：
   ```json
   {
     "hasCompletedOnboarding": true
   }
   ```
   **注意**：JSON 格式要求严格，上一行末尾需有英文逗号（若存在其他字段）。

---

## 四、日常维护与状态验证

### 1. 关键操作备忘
- **更换 Key 或 URL**：修改 `$PROFILE` 文件中对应的 `$env:ANTHROPIC_API_KEY` 和 `$env:ANTHROPIC_BASE_URL` 值。
- **Key 或 URL 未生效**：运行 `. $PROFILE` 或重启终端窗口。
- **仍弹出登录界面**：检查 `$HOME\.claude.json` 中的 `hasCompletedOnboarding` 是否为 `true`。

### 2. 验证运行状态
在 Claude Code 会话中输入 `/status`，若看到 **API key: ANTHROPIC_API_KEY** 及正确的 **Base URL**，说明正以 API 模式运行（隔离模式）。

### 3. 管理员权限提示
在 Windows 上修改 `claude.exe` 路径或系统环境变量时，PowerShell 需以**管理员身份运行**。

---

## 五、下一步
完成基础配置后，可学习 [基础使用手册](./02-Claude%20Code%20基础使用手册.md) 掌握常用命令与交互技巧。