---
{"dg-publish":true,"permalink":"/ai-web-knowledge/moc-ai/","noteIcon":""}
---

# 🚀 AI 全栈开发部署知识库

> 本知识库系统梳理从**技术选型**到**生产部署**的完整工程实践，涵盖架构设计、开发环境、安全防御、服务器搭建、Docker 部署、CI/CD 自动化、数据存储与扩展知识，形成从"本地开发"到"云端上线"的全链路知识体系。

---

## 📑 知识库结构概览

本知识库由 **1 个主索引 (MOC)** 和 **9 个核心文档** 组成，采用"从架构设计到生产运维"的自然开发逻辑，每篇文档均采用**操作步骤 + 原理说明**的双线编排模式。

### 核心文档导航

1. [[ai-web-knowledge/1-架构设计与技术选型\|1-架构设计与技术选型]] - 技术栈全景、Next.js 选型、四容器架构、Prisma 原理、项目结构规约与启动访问
2. [[ai-web-knowledge/2-开发环境与工作流\|2-开发环境与工作流]] - Vibe Coding 设计工作流、Windows 开发考量、网络访问原理、开发者标准流水线
3. [[ai-web-knowledge/3-安全架构：验证与防御\|3-安全架构：验证与防御]] - NextAuth 认证体系、三层机器人防御、Nginx 反向代理、域名路由与备案
4. [[ai-web-knowledge/4-服务器环境搭建与Docker安装\|4-服务器环境搭建与Docker安装]] - 服务器健康检查、Docker 三种安装方案、镜像加速器配置、卷权限问题全解
5. [[ai-web-knowledge/5-从代码到云端的部署实战\|5-从代码到云端的部署实战]] - SSH 连接、代码传输、环境变量、Docker 构建编排、生产环境配置、数据库运维、增量更新
6. [[ai-web-knowledge/6-CICD自动化流水线\|6-CICD自动化流水线]] - 阿里云 ACR 初始化、GitHub Actions 流水线、镜像驱动数据同步、安全与回滚策略
7. [[ai-web-knowledge/7-数据存储与云服务基础\|7-数据存储与云服务基础]] - 云服务概念（ECS/CDN/OSS）、MinIO vs SeaweedFS 对比、决策框架与迁移策略
8. [[ai-web-knowledge/8-扩展知识：边缘计算与AI缓存\|8-扩展知识：边缘计算与AI缓存]] - 边缘计算原理、KV Cache 机制、语义缓存与 Prompt Caching
9. [[ai-web-knowledge/9-附录：项目参考与命令手册\|9-附录：项目参考与命令手册]] - Linux 命令速查、项目结构参考、API 路由一览、部署哲学复盘

---

## 🗺️ 知识地图：从技术选型到生产运维

### 01. 架构设计与技术选型
**核心文档：** [[ai-web-knowledge/1-架构设计与技术选型\|1-架构设计与技术选型]]

- 技术栈全景与选型依据
- Next.js 初始化与配置决策
- 四容器全栈部署架构设计
- Prisma ORM 工作原理与工作流
- 项目目录结构与核心配置文件
- 开发服务器启动与访问流程

---

### 02. 开发环境与工作流
**核心文档：** [[ai-web-knowledge/2-开发环境与工作流\|2-开发环境与工作流]]

- Vibe Coding + Google Stitch 四步设计工作流
- Windows 开发与 Linux 部署的差异与统一
- 开发者标准闭环流水线
- 网络访问原理（监听、网卡、localhost、0.0.0.0）
- 手机局域网访问配置与脚本原理
- 关键工具（WSL2、Docker Desktop、Git、SSH）职责

---

### 03. 安全架构：验证与防御
**核心文档：** [[ai-web-knowledge/3-安全架构：验证与防御\|3-安全架构：验证与防御]]

- NextAuth + PostgreSQL + Prisma 认证体系
- Resend 邮箱验证与沙箱限制
- 三层防御体系（蜜罐、速率限制、Turnstile）
- 流量穿透三级跳（DNS → 安全组 → Nginx）
- Nginx 反向代理配置
- NEXTAUTH_URL 环境变量全阶段设置
- 域名、IP 与备案的物权关系

---

### 04. 服务器环境搭建与 Docker 安装
**核心文档：** [[ai-web-knowledge/4-服务器环境搭建与Docker安装\|4-服务器环境搭建与Docker安装]]

- 服务器健康检查命令清单
- Docker 引擎三种安装方案（标准、镜像源、插件）
- Docker Compose 独立版与插件版
- Docker 镜像加速器配置（daemon.json）
- Docker 卷权限问题根本原因分析
- 五种权限解决方案对比与最佳实践

---

### 05. 从代码到云端的部署实战
**核心文档：** [[ai-web-knowledge/5-从代码到云端的部署实战\|5-从代码到云端的部署实战]]

- SSH 密钥对生成与 Config 别名配置
- 三种代码传输方案（Git、加速代理、SCP 空投）
- 环境变量双文件配置策略
- Docker Compose 深度解析（up/down/build 原理）
- Dockerfile 多阶段构建与分层缓存
- 一键启动与验证清单
- Prisma Studio 数据库管理
- 增量更新与原子化部署
- 常见问题排查

---

### 06. CI/CD 自动化流水线
**核心文档：** [[ai-web-knowledge/6-CICD自动化流水线\|6-CICD自动化流水线]]

- 三大分离原则（代码/配置、构建/运行、基础设施/业务）
- 阿里云 ACR 初始化四步流程
- GitHub Actions deploy.yml 完整解析
- SSH 密钥对与 Secrets 配置映射
- 安全回滚与资源保护策略
- 镜像驱动型数据同步机制
- entrypoint.sh "先迁移再启动" 原子化升级

---

### 07. 数据存储与云服务基础
**核心文档：** [[ai-web-knowledge/7-数据存储与云服务基础\|7-数据存储与云服务基础]]

- 六大云服务概念（ECS、CDN、OSS、SLB、RDS、VPC）
- 传统文件系统 vs 对象存储对比
- MinIO vs SeaweedFS 深度对比
- MinIO 单节点部署与代码集成
- 按项目阶段的决策框架
- 渐进式迁移四步策略

---

### 08. 扩展知识：边缘计算与 AI 缓存
**核心文档：** [[ai-web-knowledge/8-扩展知识：边缘计算与AI缓存\|8-扩展知识：边缘计算与AI缓存]]

- 边缘计算核心哲学与三大痛点
- 静态资源按需缓存（LRU 算法）
- Edge Functions V8 Isolate 原理
- KV Cache 备忘录机制
- Prompt Caching 与缓存命中
- 语义缓存与向量相似度判断

---

### 09. 附录：项目参考与命令手册
**核心文档：** [[ai-web-knowledge/9-附录：项目参考与命令手册\|9-附录：项目参考与命令手册]]

- Linux 命令速查（SSH、导航、文件、系统、Docker）
- 项目完整目录结构
- API 路由一览（认证、课程、章节、小节）
- 权限系统结构
- 工程哲学复盘与思维转变

---

*文档数量：9 份核心知识文档*
