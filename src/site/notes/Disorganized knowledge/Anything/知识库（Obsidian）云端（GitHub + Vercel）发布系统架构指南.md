---
{"dg-publish":true,"permalink":"/disorganized-knowledge/anything/obsidian-git-hub-vercel/","noteIcon":""}
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


---

## 4. 🔐 私人备份：Git 全量同步体系搭建

除了面向公网发布的 Digital Garden 机制外，建议建立一套基于 **Obsidian Git** 插件的全量备份体系，以确保您的所有笔记资产、附件及插件配置安全同步至私有仓库。

### 第一步：创建 GitHub 私人备份仓库

1. **进入创建页面**：
    
    - 登录 GitHub 账号，点击页面右上角的 **`+`** 号图标，选择 **`New repository`**。
        
2. **配置仓库基本信息**：
    
    - **Owner**：确保选择的是您的个人账号。
        
    - **Repository name**：输入 `Personal-Knowledge-Base`（或任何您喜欢的名字，建议使用英文并用连字符分隔）。
        
    - **Description (Optional)**：可以填写“Obsidian 个人知识库全量备份”。
        
3. **【核心】设置隐私权限**：
    
    - 选中 **`Private`** 选项。
        
    - **理由**：Private 仓库只有您自己能看到，这能确保未发布的草稿、个人日记以及后续可能产生的敏感插件配置（如 Token）不会在公网上暴露。
        
4. **初始化设置（重要：保持留空）**：
    
    - **不要** 勾选 "Add a README file"。
        
    - **不要** 勾选 "Add .gitignore"（我们稍后会在本地手动创建更完整的版本）。
        
    - **不要** 勾选 "Choose a license"。
        
    - **理由**：保持仓库完全空白（Empty Repository），可以极大简化后续本地代码库与云端的首次关联流程，避免出现合并冲突。
        
5. **完成创建**：
    
    - 点击底部的绿色按钮 **`Create repository`**。
        
    - **保存地址**：创建成功后，页面会显示该仓库的 HTTPS 地址（如 `https://github.com/YourName/Personal-Knowledge-Base.git`），请点击右侧的复制图标将其记下，我们在配置本地插件时会用到它。
        

### 第二步：配置隐私保护与过滤规则（核心步骤）

为了防止将包含个人隐私、插件密钥或冗余的系统缓存文件上传至云端，必须在同步前配置忽略规则：

1. **创建忽略文件**：在 Obsidian 根目录下新建一个名为 `.gitignore` 的文本文件。
    
2. **写入忽略名单**：将以下内容完整复制并保存：
    

```
# 忽略 Obsidian 运行时产生的临时布局文件（核心必填）
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.obsidian/cache/

# 忽略系统自动生成的垃圾文件
Desktop.ini
.DS_Store

# 忽略以后可能出现的 IDE 配置文件
.idea/
.vscode/

# 忽略插件生成的敏感配置文件（包含发布 Token 等隐私）
.obsidian/plugins/digitalgarden/data.json
```

3. **作用**：该配置将确保 Git 在同步时自动跳过这些文件，既能保护隐私安全，又能保持云端仓库的纯净。
    

### 第三步：Obsidian Git 插件配置与日常同步

1. **安装插件与基础环境检查**：
    * **安装插件**：在 Obsidian 插件市场搜索并安装 **Obsidian Git**。
    * **环境要求**：确保您的电脑已安装 Git 客户端，并在当前知识库根目录完成初始化（若未初始化，请在 CMD 执行 `git init`）。

2. **建立远程关联（仅需执行一次）**：
    为了让插件知道往哪里推送，需要在 CMD 中手动绑定您的私人仓库地址：
    * 打开 CMD，定位到知识库根目录。
    * **添加远程仓库**：执行 `git remote add origin https://github.com/您的用户名/仓库名.git`。
    * **设置默认分支**：执行 `git branch -M main`。

3. **插件核心设置与快捷键绑定**：
    * **禁用自动同步**：进入插件设置，将 `Vault backup interval` 设为 `0`。
    * **理由**：自动同步在网络不佳（如热点切换）时极易产生冲突或报错，建议改为手动触发。
    * **设置快捷键**：进入 Obsidian **快捷键 (Hotkeys)** 设置：
        * 搜索 `Git: Commit all changes`，绑定为 `Ctrl + Alt + S`。
        * 搜索 `Git: Push`，绑定为 `Ctrl + Alt + D`。

4. **日常同步逻辑 (SOP)**：
    * **阶段性保存**：完成一段重要写作后，按下 `Ctrl + Alt + S`。
        * **效果**：插件会自动扫描改动文件，并以当前时间戳作为备注完成本地提交。
    * **同步上云**：接着按下 `Ctrl + Alt + D`。
        * **效果**：将本地记录上传至 GitHub。若成功，右下角会弹出 `Pushed x files` 提示。

5. **命令行保险方案（防断连）**：
    若遇到 VPN 波动或插件弹出红色 `SSL/TLS` 报错，请直接打开 CMD 执行以下“手动三部曲”：

```bash
# 1. 检查状态
git status

# 2. 打包并提交（本地保存）
git add .
git commit -m "manual backup: %date%"

# 3. 推送上云（通过代理同步到 GitHub）
git push origin main
# 如果报错，请确保执行过：git config --global http.proxy http://127.0.0.1:7897
git push origin main
```
 

---

**至此，您已经完成了“双轨制”同步体系的搭建。本地创作的每一篇笔记，既可以通过 Git 实现全量私有备份，也可以通过 Digital Garden 精准发布到公网。大功告成！**

## 5. ⚠️ 重要：文件名命名规范与冲突规避

在 Digital Garden 发布流程中，**文件名（File Name）** 直接决定了最终网页的 **URL 路径**。由于插件在生成 URL 时会执行“英文字符提取”策略（自动忽略汉字、只保留英文与数字），如果文件名缺乏足够的英文区分度，极易导致多个文档指向同一个 URL，从而触发 Vercel 的 `Output conflict` 报错。

### 1. 建议的命名规范：(英文简称) 模式

为了确保每个文档在云端拥有绝对唯一的“门牌号”，且无需手动配置复杂的插件属性，建议统一采用 **“中文题目 + (英文简称)”** 的命名方式：

- **规范示例**：
    
    - `软件工程演进与AI Agent调研报告 (software-evolution-report).md`
        
    - `AI Agent对就业市场的重塑影响 (agent-job-market-impact).md`
        
- **操作逻辑**：
    
    - **自动识别**：插件会自动无视中文，将括号内的英文作为该网页的 URL。
        
    - **强制唯一**：只要您确保括号内的英文简称互不相同，Vercel 就永远不会发生路径冲突。
        

### 2. 出错后的“强力修复”SOP

如果您在发布过程中由于命名不当遇到了红色的 `Output conflict` 报错（提示多个文件正在写入同一个 index.html），请按以下步骤快速修复：

1. **GitHub 端物理删除**：
    
    - 登录您的 GitHub 仓库，手动进入 `src/site/notes/` 下对应的子文件夹。
        
    - **直接删除** 报错中提到的那几个旧的、未按规范命名的 `.md` 文件。
        
    - **作用**：这一步是彻底清除云端的“占位符”，为新路径腾出空间。
        
2. **本地规范化重命名**：
    
    - 回到 Obsidian，将刚才冲突的文档题目按照 **(英文简称)** 模式进行修改。
        
3. **重新发布**：
    
    - 在 **Publication Center** 中勾选重命名后的文件，点击 **`PUBLISH SELECTED`**。
        
    - 此时 Vercel 会检测到唯一的英文路径并重新开始构建，网站将顺利恢复正常。
        
