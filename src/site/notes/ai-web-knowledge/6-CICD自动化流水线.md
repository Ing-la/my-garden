---
{"dg-publish":true,"permalink":"/ai-web-knowledge/6-cicd/","noteIcon":""}
---


# 6. CICD 自动化流水线

> 手动部署熟练后，进入"推送即上线"的自动化阶段。本章涵盖阿里云 ACR 配置、GitHub Actions 流水线编写、以及镜像驱动的数据同步机制。

---

## 6.1 核心哲学

```
手动部署流程：
本地写代码 → Git Push → SSH 登录 → Git Pull → Docker 重启
                        ↑ 每一步都要人盯着

自动化流程：
本地写代码 → Git Push → [GitHub Actions 自动完成剩下的一切]
                        ↑ 只需一行命令，剩下交给机器
```

**三大分离原则**：

| 原则 | 含义 | 实现方式 |
|------|------|----------|
| **代码与配置分离** | 代码进 Git，配置留在服务器 | `.env` 不进入版本库 |
| **构建与运行分离** | 云端构建镜像，服务器只拉取运行 | GitHub Actions + ACR |
| **基础设施与业务分离** | 数据库/Nginx 不动，业务代码流动 | Docker Compose 编排 |

---

## 6.2 架构全景

| 环节 | 工具 | 职责 | 安全感的来源 |
|------|------|------|-------------|
| **开发层** | Next.js / Cursor | 编写业务逻辑 | 本地 `npm run dev` 实时反馈 |
| **构建层** | GitHub Actions | 纯净环境构建 | 排除"我电脑能跑，服务器不行" |
| **中转层** | 阿里云 ACR（容器镜像服务） | 存储不可变镜像 | 解决跨境网速，提供版本备份 |
| **运行层** | 北京云服务器 | 拉取并运行容器 | 只负责执行，保持"毛坯房"纯净度 |

### 自动化流水线的完整链路

```
1. 本地: git push origin main
       ↓
2. GitHub Actions: 检测到 main 分支推送
       ↓
3. 构建 Job: 拉代码 → 登录 ACR → docker build → docker push
       ↓
4. 部署 Job: SSH 到服务器 → 登录 ACR → 修改 .env 中的镜像版本 → docker compose pull → up -d
       ↓
5. 完成: 新服务无缝替换旧服务
```

---

## 6.3 阿里云 ACR（镜像仓库）初始化

### 第一步：创建个人版实例

1. 登录 [阿里云官网](https://www.aliyun.com/)
2. 搜索"容器镜像服务"或"ACR"
3. 在左侧导航栏点击 **实例列表** → **创建个人版实例**（免费）
4. 创建成功后，点击实例名称进入管理后台

### 第二步：设置访问凭证

在个人版实例左侧菜单点击 **访问凭证** → **设置固定密码**。
- **用户名**：系统自动生成，复制保存
- **密码**：手动设置复杂密码

这里的用户名+密码就是以后在任何地方执行 `docker login` 的通行证。

### 第三步：创建命名空间

左侧菜单 **命名空间** → **创建命名空间**：

| 参数 | 建议值 | 说明 |
|------|--------|------|
| 命名空间名称 | `ai-web-app` | 用项目简称 |
| 默认仓库类型 | **私有** | 别人无法拉取你的镜像 |

### 第四步：创建镜像仓库

左侧菜单 **镜像仓库** → **创建镜像仓库**：

1. **基本信息**：选择刚创建的命名空间，仓库名称如 `frontend`，类型 **私有**
2. **代码源**：选择 **本地仓库**（因为我们让 GitHub Actions 打包后推给阿里云，不是让阿里云去拉 GitHub）

### 第五步：服务器端登录验证（取货准入证）

```bash
# 在服务器上执行（命令从 ACR 控制台【访问凭证】页面复制）
sudo docker login --username=nick8984315946 crpi-2kxck9tj3kavmcmf.cn-beijing.personal.cr.aliyun.com
# 输入固定密码（屏幕不显示输入过程）
# 看到 Login Succeeded 即可
```

**原理说明**：
> 阿里云 ACR 的私有镜像需要登录才能拉取。服务器执行 `docker login` 后，凭证持久化存储，后续无需重复登录。

---

## 6.4 GitHub Actions 流水线配置

### GitHub Secrets 配置

进入 GitHub 项目 → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**，添加以下 5 个密钥：

| Secret 名称 | 填入内容 | 来源 |
|-------------|----------|------|
| `ACR_USERNAME` | `nick8984315946` | 阿里云访问凭证用户名 |
| `ACR_PASSWORD` | `********` | 阿里云固定密码 |
| `ACR_REGISTRY` | `crpi-2kxck9tj3kavmcmf...` | 阿里云控制台专属域名地址 |
| `SERVER_HOST` | `123.123.123.123` | 服务器公网 IP |
| `SERVER_SSH_KEY` | `-----BEGIN RSA...-----END...` | 服务器生成的私钥完整内容 |

### 服务器端：生成 SSH 密钥对

```bash
# 在服务器上执行，用于 GitHub Actions 远程登录
ssh-keygen -t rsa -b 4096 -f ~/.ssh/github_actions_key

# 安装公钥
cat ~/.ssh/github_actions_key.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh

# 获取私钥（复制完整内容，包含 -----BEGIN/END-----）
cat ~/.ssh/github_actions_key
```

### `deploy.yml` 完整解析

在项目根目录创建 `.github/workflows/deploy.yml`：

```yaml
name: Deploy to Beijing Server

# 触发器：发令枪
on:
  push:
    branches: [ "main" ]      # 只有推送到 main 才触发

jobs:
  # ========== Job 1：云端工厂（构建并推送镜像）==========
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      - name: 1. 搬运代码
        uses: actions/checkout@v3
        # 将 GitHub 仓库最新代码复制到虚拟车间

      - name: 2. 登录阿里云 ACR
        uses: aliyun/acr-login@v1
        with:
          login-server: ${{ secrets.ACR_REGISTRY }}
          username: ${{ secrets.ACR_USERNAME }}
          password: ${{ secrets.ACR_PASSWORD }}
        # 用 Secrets 里存的证件登录，密码不显示为明文

      - name: 3. 构建并推送镜像
        run: |
          docker build -t ${{ secrets.ACR_REGISTRY }}/ai-web-app/frontend:latest ./frontend
          docker push ${{ secrets.ACR_REGISTRY }}/ai-web-app/frontend:latest
        # docker build → docker push，镜像进入阿里云 ACR

  # ========== Job 2：远程遥控（部署到服务器）==========
  deploy:
    needs: build-and-push        # 【逻辑锁】打包成功后才部署
    runs-on: ubuntu-latest

    steps:
      - name: 1. SSH 连接到服务器
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: root
          key: ${{ secrets.SERVER_SSH_KEY }}
          script: |
            # 以下命令在服务器上执行

            # 进入项目目录
            cd /root/ing/openrise

            # 登录阿里云（拉取私有镜像需要）
            docker login --username=${{ secrets.ACR_USERNAME }} --password=${{ secrets.ACR_PASSWORD }} ${{ secrets.ACR_REGISTRY }}

            # 动态更新 .env 中的镜像版本
            sed -i "s|^FRONTEND_IMAGE=.*|FRONTEND_IMAGE=${{ secrets.ACR_REGISTRY }}/ai-web-app/frontend:latest|" .env

            # 拉取最新镜像并重启
            docker compose --profile production pull frontend
            docker compose --profile production up -d frontend

            # 清理旧镜像，防止磁盘爆满
            docker image prune -f
```

### deploy.yml 关键设计解析

**触发器（`on: push: branches: [main]`）**
> - 只有推送到 `main` 分支才触发
> - 在开发分支乱改代码时，不会误触发生产环境部署

**逻辑锁（`needs: build-and-push`）**
> - 两个 Job 是串行的：构建失败 → 部署不会执行
> - 线上旧版本保持运行，**确保服务永不中断**

**动态更新配置（`sed -i`）**
> ```bash
> sed -i "s|^FRONTEND_IMAGE=.*|FRONTEND_IMAGE=xxx:latest|" .env
> ```
> - `sed` 是 Linux 强大的文本替换工具
> - 将 `.env` 中的 `FRONTEND_IMAGE` 替换为新推送的镜像版本
> - Docker Compose 读取 `.env` 决定拉取哪个版本的镜像

**基础镜像 vs 业务镜像**：
> - **PostgreSQL、Nginx** 等基础镜像直接从官方拉取（镜像加速器加速）
> - **Frontend（业务代码）** 通过 GitHub Actions 构建后推送到 ACR，服务器再从 ACR 拉取
> - 这种"分步拉取"策略解决了服务器直连 Docker Hub 速度慢的问题

---

## 6.5 安全与回滚策略

### 失败熔断

- GitHub 构建阶段失败 → `deploy` Job 不会执行
- 服务器上的旧版本保持运行，**服务不中断**

### 版本回滚

| 方式 | 操作方法 | 速度 |
|------|----------|------|
| **Git 回滚** | `git revert` 撤销代码 → 推送 → 自动触发部署 | 分钟级 |
| **镜像回滚** | 修改服务器 `.env` 指向 ACR 中的旧版本 Tag | 秒级 |

### 资源保护

```bash
# 每次部署末尾清理旧镜像，防止磁盘爆满
docker image prune -f
```

---

## 6.6 镜像驱动型数据同步

### 总体设计哲学

在自动化部署架构中，不再手动操作数据库或存储。所有的结构需求"写在代码里"，镜像在启动的一瞬间，强制让外部环境向镜像内的标准看齐。

### 文件角色分工

| 文件 | 角色 | 核心作用 |
|------|------|----------|
| **`prisma/schema.prisma`** | 立法者 | 定义数据库"长什么样"，所有变更的源头 |
| **`prisma/migrations/`** | 执行令 | 记录从版本 A 到版本 B 的 SQL 动作 |
| **`entrypoint.sh`** | 督察员 | 在网站运行前强制执行数据库迁移 |
| **`Dockerfile`** | 封装盒 | 将以上所有逻辑打包成自包含的镜像 |

### "先更新，后运行"的启动链条

```bash
# frontend/entrypoint.sh — 容器启动的"第一公民"
#!/bin/sh
set -e

# 步骤 A：数据库同步（先执行迁移）
echo "正在对比数据库版本并执行迁移..."
npx prisma migrate deploy
# 不成功 → 立即退出，旧代码不会在错误的数据结构上运行

# 步骤 B：启动业务
echo "数据库已就绪，正在启动 Next.js 应用..."
exec node server.js
```

```dockerfile
# frontend/Dockerfile — 赋予 entrypoint 最高启动权
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
```

**原理说明**：
> - `ENTRYPOINT` 指令指定容器启动时**必须先跑这个脚本**
> - `set -e`：任何命令失败立即退出
> - 数据库迁移失败 → 容器保持在 Crash 状态 → 旧业务代码不会启动
> - 这种"自我防护"机制叫**原子化升级**：要么全对，要么不跑

### 自动化同步路径

```
代码提交 → 镜像封装 → 容器启动 → entrypoint.sh → prisma migrate deploy → 应用启动
  ↓           ↓           ↓           ↓                  ↓                ↓
改 schema   prisma/    新镜像      脚本先执行        扫描未执行的 SQL    数据库就绪后
            打包进镜像  拉取并运行                     自动执行迁移       网站才上线
```

---

## 6.7 标准开发工作流（自动化版）

```bash
# 1. 本地开发与验证
npm run dev

# 2. 代码提交（触发自动部署）
git add .
git commit -m "feat: 描述你的更新内容"
git push origin main

# 3. 观察部署结果
# 打开 GitHub → Actions 页面 → 查看绿色对勾
# 或者直接访问网站确认更新已上线

# 4. 如有问题 → 回滚
git revert HEAD
git push origin main  # 自动触发回滚部署
```

**运维 Checklist**：
- [ ] 镜像版本化：不要只用 `latest`，使用 `git-sha` 或版本号
- [ ] 健康检查：在 `docker-compose` 中配置 `healthcheck`
- [ ] 定时备份：设置 Crontab 每天备份数据库
- [ ] 监控告警：使用 UptimeRobot 监控域名，服务器掉线立即通知

---

**← [[ai-web-knowledge/MOC-AI全栈开发部署知识库\|返回知识库]]** | **→ [[ai-web-knowledge/7-数据存储与云服务基础\|下一篇：数据存储与云服务]]**
