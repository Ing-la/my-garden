---
{"dg-publish":true,"permalink":"/ai-knowledge//obsidian-vercel/","noteIcon":""}
---



# 🚀 知识库云端发布系统：核心架构指南

本架构利用 **Obsidian + GitHub + Vercel** 的联动，将原本私密的本地笔记转化为一个可全球访问、且具备交互式图谱的“数字花园”。

## 1. 核心组件拆解

|**组件**|**名称**|**定义与扮演的角色**|**核心作用**|**解决的问题**|
|---|---|---|---|---|
|**Obsidian**|**创作基地**|基于 Markdown 文件的**本地离线编辑器**。|提供双向链接、局部图谱和强大的插件生态。|解决了对云端平台的依赖，确保知识永久私有、离线可用。|
|**Digital Garden**|**搬运插件**|运行在 Obsidian 内部的**发布管理工具**。|自动处理 MD 转换、图片扫描、并一键推送至 GitHub。|解决了手动上传代码、配置网页前端的复杂技术门槛。|
|**GitHub**|**云端仓库**|全球最大的**代码托管与版本管理平台**。|作为笔记和图片的“云端保险箱”，并充当 Vercel 的数据源。|解决了数据备份、版本回溯以及云端存储的需求。|
|**Vercel**|**展示窗口**|现代化的**静态网站部署与托管平台**。|自动抓取 GitHub 代码进行构建（Build），生成公网网址。|解决了服务器部署、运维以及如何让外部用户访问的问题。|

---

## 2. 自动化发布流（Workflow）

整个系统通过以下四个步骤实现“一键上云”：

1. **标签驱动 (Obsidian)**：你在本地笔记头部加入 `dg-publish: true`。插件据此识别哪些文件允许公开。
    
2. **同步推送 (Digital Garden)**：你点击“小树苗”图标（Publication Center），插件将 MD 文件和图片同步推送到 GitHub 仓库的 `src/site/notes` 路径。
    
3. **触发构建 (GitHub → Vercel)**：GitHub 接收到新代码后，立刻通知 Vercel。Vercel 启动构建任务，将 Markdown 转化为 HTML 网页。
    
4. **实时访问 (Vercel)**：构建成功后，你的域名（如 `your-garden.vercel.app`）会立即显示最新的笔记和关联图谱。
    

---

## 3. 🌐 数字花园：Obsidian + GitHub + Vercel 搭建全指南

本节详细介绍如何从零开始构建这套自动化发布系统。

### 第一步：GitHub 仓库环境初始化

1. **获取正确模板**：
    * 访问 [Digital Garden 官方发布模板仓库](https://github.com/oleeskild/digitalgarden)。
    * **注意**：必须使用这个特定的仓库，因为它包含了 Vercel 构建网页所需的 Eleventy 环境和配置文件。

2. **创建您的私人副本**：
    * 在页面右上角，找到并点击绿色的 **`Use this template`** 按钮。
    * 在下拉菜单中选择 **`Create a new repository`**。

3. **配置新仓库信息**：
    * **Owner**: 选择您的个人 GitHub 账号。
    * **Repository name**: 为您的数字花园起一个简洁的名字（例如 `my-garden` 或 `brain-notes`）。
    * **Privacy（隐私设置）**：
        * **Public**: 任何人都可以看到您的源码，但部署最简单。
        * **Private**: 源码仅您可见。Vercel 同样支持私有库部署，建议选择 **Private** 以保护未发布的草稿内容。
    * **Include all branches**: 无需勾选，保持默认即可。
    * **确认创建**：点击底部的 **`Create repository from template`**。

4. **确认仓库就绪**：
    * 页面会自动跳转到您新创建的仓库。
    * 请确认目录下存在 `src` 文件夹和 `package.json` 文件，这代表环境已经克隆成功。
    

### 第二步：Obsidian 内部插件配置

1. **安装插件**：在 Obsidian 插件市场搜索并安装 **Digital Garden**。

2. **获取 GitHub Token（修正后的正确顺序）**：
    为了让 Obsidian 有权将笔记推送到您的仓库，需要创建一个身份验证令牌（Token）：
    * **进入设置**：登录 GitHub，点击右上角头像 -> **Settings**。
    * **开发者选项**：在左侧菜单最下方找到并点击 **Developer settings**。
    * **选择 Token 类型**：点击 **Personal access tokens** -> **Fine-grained tokens**。
    * **点击生成**：点击 **Generate new token**。
    * **填写基础信息**：
        * **Token name**: 填写 `Obsidian-Garden`。
        * **Expiration**: 建议设置为 `Custom`（选择一年或更久）以避免频繁失效。
    * **【关键】选择仓库范围**：
        * 在 **Repository access** 区域，选择 **Only select repositories**。
        * 在弹出的下拉框中，搜索并选中您刚创建的笔记仓库（例如 `my-garden`）。
    * **【关键】配置权限**（选中仓库后才会生效）：
        * 点击下方的 **Permissions** -> **Repository permissions**。
        * 找到 **Contents** 选项，在右侧下拉框中选择 **Read and write**（读与写）。
    * **生成并保存**：点击页面最下方的 **Generate token**。
    * **注意**：生成的 `github_pat_` 开头的 Token 只会显示一次，请立即复制并妥善保存。

3. **连接配置**：
    * 回到 Obsidian，进入 **Digital Garden** 插件设置页面。
    * **GitHub username**: 填写您的 GitHub 用户名。
    * **GitHub repo**: 填写您的仓库名称。
    * **GitHub token**: 粘贴刚才生成的 Token。
    * **Base URL**: 暂时留空。待下一步 Vercel 部署完成后，将 Vercel 提供的网址填入此处，以便插件生成正确的预览链接。
    

### 第三步：Vercel 自动化部署

1. **关联项目**：
    * 登录 [Vercel 官网](https://vercel.com/)（建议直接使用 GitHub 账号授权登录）。
    * 在 Dashboard 页面，点击右上角的 **`Add New...`** 黑色按钮，选择 **`Project`**。

2. **导入 GitHub 仓库**：
    * **首次连接**：如果是第一次使用，点击 **`Install Resource`** 并按照弹窗提示，授权 Vercel 访问您的 GitHub 账号。
    * **选择仓库**：在 "Import Git Repository" 列表中找到您刚才创建的仓库（如 `my-garden`），点击旁边的 **`Import`** 按钮。

3. **配置与一键部署**：
    * **Project Name**: 可以保持默认，或者起一个您喜欢的名字。
    * **Framework Preset**: 确保识别为 **`Other`**（因为模板是基于 Eleventy 构建的）。
    * **Root Directory**: 保持默认的 `./` 即可。
    * **Build and Output Settings**: 默认情况下无需修改。
    * **执行部署**：点击下方的黑色 **`Deploy`** 按钮。

4. **确认上线与后续操作**：
    * **等待构建**：页面会出现烟花效果，表示部署成功。
    * **获取域名**：点击 **`Visit`** 预览按钮，或在项目控制台找到以 `.vercel.app` 结尾的链接。这就是您数字花园的公网地址。
    * **【关键回填】**：复制这个完整的域名，回到 **Obsidian 的 Digital Garden 插件设置**，将其填入 **Base URL** 框中。这样插件生成的预览链接和搜索索引才能准确指向您的在线站点。


