---
{"dg-publish":true,"permalink":"/remote-control/moc/","noteIcon":""}
---

# 📱 手机远程控制 — 知识体系

> 从"用手机操作电脑 CLI"到"自建 Agent 远程操控接口"的完整技术原理与实操指南。涵盖 WebSocket、PTY、SSH、网络穿透、会话持久化等核心概念，以及从零开始的 Tailscale + Termius 配置步骤。

---

## 📑 知识库结构概览

本知识库由 **1 个主索引（本文档）** 和 **3 个核心文档** 组成，采用"先理解原理，再上手操作"的学习路径。

---

## 🗺️ 知识地图

### 01. 理论原理
**核心文档：** [[远程控制-remote-control/1-远程控制-理论与原理\|1-远程控制-理论与原理]]

- 两种需求（自建 Agent 远程接口 vs 操作 Claude Code CLI）与统一的技术基础
- HTTP 与 WebSocket 的协议对比及角色分工
- WebSocket 的本质、浏览器与 Node.js 端实现
- 信息传播完整路径分析
- PTY 原理、进程标准输入输出、信号传递机制
- 网络穿透原理（NAT、Tailscale、ngrok、Cloudflare Tunnel）
- tmux 与会话持久化原理
- SSH 远程控制原理（加密握手、端口转发）
- P2P 直连 vs Gateway 调度架构模式

---

### 02. 实操指南
**核心文档：** [[远程控制-remote-control/2-远程控制-实操指南\|2-远程控制-实操指南]]

- 准备工作与前置条件说明
- 电脑端配置（Tailscale 安装、SSH 服务开启、电源设置）
- 手机端配置（Tailscale + Termius）
- Termius 连接配置与测试
- 远程运行 Claude Code 的完整流程（启动、中断、重连）
- 会话持久化的场景说明

---

### 03. 前置准备：美区 Apple ID
**核心文档：** [[远程控制-remote-control/3-外区Apple ID注册教程\|3-外区Apple ID注册教程]]

- 网页注册美区 Apple ID 流程
- iPhone App Store 切换账号
- VPN 开启与 Tailscale 搜索下载

---
[[Home\|Home]]
