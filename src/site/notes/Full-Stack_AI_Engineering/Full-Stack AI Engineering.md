---
{"dg-publish":true,"permalink":"/full-stack-ai-engineering/full-stack-ai-engineering/","noteIcon":""}
---

# 📂 2026 AI 全栈开发知识库索引

> 本知识库采用"从宏观到微观，从研发到部署"的逻辑结构，系统梳理 AI 全栈开发的核心知识体系。

---

## 📑 知识库结构概览

本知识库由 **1 个主索引** 和 **5 个专项文档** 组成，采用 Obsidian 双链结构，便于知识关联与检索。

### 核心文档导航

- [[Full-Stack_AI_Engineering/01-Web基础架构与通讯协议\|01-Web基础架构与通讯协议]] - Web 三层物理结构、请求-响应生命周期、HTTP 协议、CORS 跨域机制、API-First 理念
- [[Full-Stack_AI_Engineering/02-现代前端工程：从V8到React\|02-现代前端工程：从V8到React]] - V8 引擎原理、DOM 树渲染、React 声明式编程、状态管理、Vite 编译链路
- [[Full-Stack_AI_Engineering/03-服务端逻辑与Agent计算环境\|03-服务端逻辑与Agent计算环境]] - Node.js/Libuv 异步机制、Python 后端选型、数据持久化、安全管理、Docker 沙箱
- [[Full-Stack_AI_Engineering/04-桌面端专题：Electron深度架构\|04-桌面端专题：Electron深度架构]] - 主进程与渲染进程、IPC 通信、Preload 脚本安全、二进制 Binding 桥接
- [[Full-Stack_AI_Engineering/05-AI云原生部署与托管实战(2026)\|05-AI云原生部署与托管实战(2026)]] - Vercel vs Streamlit、300 秒红线、CDN 加速、自定义域名、异步状态流、内网互通

---

## 🗺️ 知识地图：从研发到部署

### 01. 架构总览
**核心文档：** [[Full-Stack_AI_Engineering/01-Web基础架构与通讯协议\|01-Web基础架构与通讯协议]]

- Web 三层物理结构（表示层、逻辑层、数据层）
- 请求-响应生命周期
- HTTP 协议与 RESTful API
- CORS 跨域资源共享机制
- API-First 设计理念

**关联文档：** [[Full-Stack_AI_Engineering/02-现代前端工程：从V8到React\|02-现代前端工程：从V8到React]] | [[Full-Stack_AI_Engineering/03-服务端逻辑与Agent计算环境\|03-服务端逻辑与Agent计算环境]]

---

### 02. 前端演进
**核心文档：** [[Full-Stack_AI_Engineering/02-现代前端工程：从V8到React\|02-现代前端工程：从V8到React]]

- JavaScript 运行环境与 V8 引擎
- DOM 树渲染与浏览器渲染引擎
- React 声明式编程与 Virtual DOM
- 状态管理（useState、useEffect、Zustand）
- Vite 编译链路与 JSX 转换

**关联文档：** [[Full-Stack_AI_Engineering/01-Web基础架构与通讯协议\|01-Web基础架构与通讯协议]] | [[Full-Stack_AI_Engineering/04-桌面端专题：Electron深度架构\|04-桌面端专题：Electron深度架构]]

---

### 03. 后端与计算
**核心文档：** [[Full-Stack_AI_Engineering/03-服务端逻辑与Agent计算环境\|03-服务端逻辑与Agent计算环境]]

- Node.js 原理与 Libuv 异步机制
- Python 后端选型与 FastAPI
- 数据持久化（JSON/OSS）
- 安全管理（API Key 隐藏、环境变量）
- Docker 容器化与沙箱隔离

**关联文档：** [[Full-Stack_AI_Engineering/01-Web基础架构与通讯协议\|01-Web基础架构与通讯协议]] | [[Full-Stack_AI_Engineering/05-AI云原生部署与托管实战(2026)\|05-AI云原生部署与托管实战(2026)]]

---

### 04. 跨平台专题
**核心文档：** [[Full-Stack_AI_Engineering/04-桌面端专题：Electron深度架构\|04-桌面端专题：Electron深度架构]]

- Electron 架构：主进程与渲染进程
- IPC 进程间通信机制
- Preload 脚本安全隔离
- C++ Binding 桥接与二进制分发
- 前后端协作流程

**关联文档：** [[Full-Stack_AI_Engineering/02-现代前端工程：从V8到React\|02-现代前端工程：从V8到React]] | [[Full-Stack_AI_Engineering/03-服务端逻辑与Agent计算环境\|03-服务端逻辑与Agent计算环境]]

---

### 05. 基础设施 (DevOps)
**核心文档：** [[Full-Stack_AI_Engineering/05-AI云原生部署与托管实战(2026)\|05-AI云原生部署与托管实战(2026)]]

- Docker 容器化打包
- Nginx 反向代理与网关
- 网络优化与 CDN 加速
- 自定义域名配置

**关联文档：** [[Full-Stack_AI_Engineering/03-服务端逻辑与Agent计算环境\|03-服务端逻辑与Agent计算环境]]

---

### 06. AI 工程化 (2026 特供)
**核心文档：** [[Full-Stack_AI_Engineering/05-AI云原生部署与托管实战(2026)\|05-AI云原生部署与托管实战(2026)]]

- Vercel vs Streamlit 成本与性能对比
- 300 秒运行时间限制与解决方案
- 长任务处理：异步状态流架构
- Serverless 限制与云端托管选型
- 内网互通与流量成本优化

**关联文档：** [[Full-Stack_AI_Engineering/03-服务端逻辑与Agent计算环境\|03-服务端逻辑与Agent计算环境]]

---

## 📚 使用建议

1. **新手入门路径：** 01 → 02 → 03 → 05
2. **桌面应用开发路径：** 01 → 02 → 03 → 04
3. **AI 应用部署路径：** 01 → 03 → 05
4. **深入理解路径：** 按需查阅各专项文档的详细章节

---

## 🎯 知识库特色

- ✅ **体系化结构**：从宏观架构到微观实现，逻辑清晰
- ✅ **实战导向**：聚焦 2026 年 AI 全栈开发的实际需求
- ✅ **深度解析**：拒绝结论化，深入讲解物理本质与运行机制
- ✅ **关联索引**：Obsidian 双链结构，便于知识关联与检索
- ✅ **信息完整**：保留所有重要细节，必要时进行补充说明

---
[[Home\|Home]]
