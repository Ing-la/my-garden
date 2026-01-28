---
{"dg-publish":true,"permalink":"/full-stack-ai-engineering/01-web/","noteIcon":""}
---

# 🌐 Web 基础架构与通讯协议

> **核心定位：** API 是连接一切的血脉。本文档系统阐述 Web 三层物理结构、请求-响应生命周期、HTTP 协议、CORS 跨域机制与 API-First 设计理念。

**关联文档：** [[Full-Stack_AI_Engineering/Full-Stack_AI_Engineering\|Full-Stack_AI_Engineering]] | [[Full-Stack_AI_Engineering/02-现代前端工程：从V8到React\|02-现代前端工程：从V8到React]] | [[Full-Stack_AI_Engineering/03-服务端逻辑与Agent计算环境\|03-服务端逻辑与Agent计算环境]]

---

## 目录

1. Web 三层物理架构
2. 请求-响应生命周期
3. HTTP 协议与 RESTful API
4. CORS 跨域资源共享机制
5. API-First 设计理念
6. 知识要点总结

---

## 一、Web 三层物理架构

在 Web 的世界里，所有的应用（无论是淘宝、抖音网页版还是你的 AI 聊天室）在物理上都遵循一个极其经典的**三层架构（3-Tier Architecture）**。

### 1.1 表示层 (Presentation Layer) —— 前端/客户端

- **物理位置**：运行在**用户的浏览器**（Chrome, Safari）里。
- **职能**：负责"颜值"和"交互"。它把代码变成你能看得见、摸得着的按钮和图片。
- **核心技术**：HTML（骨架）、CSS（衣服）、JavaScript（灵魂/动作）。
- **隐喻**：餐厅的**前厅和服务员**。你看到的是精美的菜单，服务员负责收你的订单。

**详细内容请参考：** [[Full-Stack_AI_Engineering/02-现代前端工程：从V8到React\|02-现代前端工程：从V8到React]]

---

### 1.2 逻辑层 (Application Layer) —— 后端/服务端

- **物理位置**：运行在远程的**云服务器（如阿里云 ECS）**上。
- **职能**：负责"大脑"和"权力"。它处理复杂的业务逻辑、权限校验和计算。
- **核心技术**：Node.js, Python (Django/Flask), Java (Spring), Go 等。
- **隐喻**：餐厅的**厨房和主厨**。主厨不直接见客，但他决定了菜怎么做、能不能做（权限校验）。

**详细内容请参考：** [[Full-Stack_AI_Engineering/03-服务端逻辑与Agent计算环境\|03-服务端逻辑与Agent计算环境]]

---

### 1.3 数据层 (Data Layer) —— 数据库/存储

- **物理位置**：通常运行在专门的**数据库服务器**或**云存储（如阿里云 OSS）**上。
- **职能**：负责"记忆"。所有的用户信息、存档数据、视频素材都锁在这里。
- **核心技术**：MySQL (关系型), MongoDB (文档型), Redis (缓存)。
- **隐喻**：餐厅的**仓库和冰柜**。只有主厨有钥匙去拿食材。

---

### 1.4 为什么 Web 开发要分层？

你可能会想：为什么要搞这么麻烦？把所有东西都写在一个地方不行吗？

#### 独立扩展（弹性）

假设你的游戏突然火了，用户暴涨。你只需要多买几台**逻辑层（云服务器）**来分担计算压力，而不必动数据库或前端代码。这叫"横向扩展"。

#### 安全隔离（防黑客）

把数据库藏在最深处。黑客能看到你的前端代码，但他摸不到你的数据库，因为只有主厨（后端）才有钥匙。

#### 多端共用

你写好了这一套后端逻辑。今天可以用网页访问，明天你可以写个手机 App，后天可以写个小程序，它们都去调同一个后端接口。**这就是"前后端分离"最牛的地方。**

---

## 二、请求-响应生命周期

在 Web 世界，由于前后端物理距离可能隔着半个地球，通话靠的是 **HTTP 协议**。

当你在浏览器里点击"提交测试结果"时，发生了如下物理过程：

### 2.1 发起请求 (HTTP Request)

- 前端把你的分数打包，加上一个"地址（URL）"，像发快递一样发出去。
- **物理路径**：数据通过你的网线，穿过无数路由器，最后精准击中那台跑着 Node.js 代码的阿里云服务器。

### 2.2 后端处理 (Processing)

- 云服务器上的后端程序接收到请求。
- 它先检查你的登录态（有没有偷跑），然后根据公式计算结果，最后通过"内网"跟数据库打招呼，把结果存起来。

### 2.3 返回响应 (HTTP Response)

- 后端把计算好的结果打包（通常是 JSON 格式），再顺着网线发回给浏览器。

### 2.4 前端渲染 (Rendering)

- 浏览器收到这串 JSON 文本，React 状态更新，屏幕上瞬间跳出"你是个外向型人格！"。

---

## 三、HTTP 协议与 RESTful API

### 3.1 API 接口：前后端握手的"标准合同"

在 Electron 里，前后端靠 IPC 打电话；但在 Web 里，前端（浏览器）和后端（云服务器）往往相隔千里。它们要协作，必须通过一种标准化的通信方式——**RESTful API**。

**关联文档：** [[Full-Stack_AI_Engineering/04-桌面端专题：Electron深度架构\|04-桌面端专题：Electron深度架构]] - IPC 与 HTTP 的本质区别

---

### 3.2 JSON：跨越时空的"脱水数据"

JSON 是目前 Web 世界最通用的数据交换格式。

- **物理本质**：它是一段极其精简的字符串。
- **为什么选它？** 因为它轻量、易读，而且无论是 Python、Java 还是 JS，都能瞬间把它"脱水"成代码里的对象。

**序列化示例：**
```javascript
// JavaScript 对象
const data = { name: "张三", score: 95 };

// JSON 序列化（脱水）
const jsonString = JSON.stringify(data);
// 结果：'{"name":"张三","score":95}'

// JSON 反序列化（复活）
const restoredData = JSON.parse(jsonString);
```

---

### 3.3 HTTP 动词：对话的"语态"

一个标准的 API 请求（如：`GET /api/user/score`）包含了明确的指令：

- **GET**：我要拿东西（比如查看分数）。
- **POST**：我要存东西（比如上传测试结果）。
- **PUT/PATCH**：我要改东西（比如更新头像）。
- **DELETE**：我要删东西（比如清空数据）。

**RESTful API 设计原则：**
- 使用名词表示资源（如 `/api/users`、`/api/scores`）
- 使用 HTTP 动词表示操作（GET、POST、PUT、DELETE）
- 返回标准化的 JSON 格式响应

---

### 3.4 API 请求的完整结构

一个完整的 HTTP 请求包含以下部分：

```
GET /api/user/score HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer token123

{
  "userId": "12345"
}
```

- **请求行**：方法 + 路径 + 协议版本
- **请求头**：元数据（Host、Content-Type、Authorization 等）
- **请求体**：实际数据（GET 请求通常没有请求体）

---

## 四、CORS 跨域资源共享机制

**CORS (Cross-Origin Resource Sharing)**，即跨域资源共享。它不是一种编程语言，而是一种基于 **HTTP 协议头（Header）** 的机制。

### 4.1 核心背景：同源策略 (Same-Origin Policy)

浏览器为了保护用户信息安全，默认执行极度严格的"同源策略"。

- **同源定义**：协议（HTTP/HTTPS）、域名（`www.google.com`）、端口（80/443/8000）必须**完全一致**。
- **冲突场景**：当你把前端放在 Vercel，后端放在阿里云 ECS 时，两者的域名不同，浏览器会默认拦截前端对后端的访问，并报出 `CORS Error`。

**同源示例：**
- ✅ `https://example.com` 与 `https://example.com/api` → 同源
- ❌ `https://example.com` 与 `http://example.com` → 不同源（协议不同）
- ❌ `https://example.com` 与 `https://api.example.com` → 不同源（域名不同）
- ❌ `https://example.com` 与 `https://example.com:8000` → 不同源（端口不同）

---

### 4.2 工作机制：预检请求 (Preflight)

当浏览器发现跨域请求时，它会进行一次"心理测试"：

#### OPTIONS 请求（预检）

浏览器先发送一个探测信号（OPTIONS 方法），询问服务器："我这个来源（Origin）可以访问你的资源吗？"

**预检请求示例：**
```
OPTIONS /api/data HTTP/1.1
Origin: https://frontend.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: Content-Type
```

#### 通行证确认

服务器在返回的 Header 中包含 `Access-Control-Allow-Origin` 等字段。如果匹配成功，浏览器才允许发送真正的业务数据。

**服务器响应示例：**
```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://frontend.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Max-Age: 86400
```

---

### 4.3 CORS 配置实践

#### 简单请求 vs 预检请求

**简单请求**（无需预检）：
- 方法：GET、POST、HEAD
- 请求头：仅限简单头（Accept、Content-Language、Content-Type 等）
- Content-Type：`application/x-www-form-urlencoded`、`multipart/form-data`、`text/plain`

**预检请求**（需要 OPTIONS）：
- 方法：PUT、DELETE、PATCH
- 自定义请求头
- Content-Type：`application/json`

#### Nginx 配置示例

```nginx
server {
    listen 80;
    server_name api.example.com;

    location /api {
        # 允许的源
        add_header 'Access-Control-Allow-Origin' 'https://frontend.com' always;
        
        # 允许的方法
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
        
        # 允许的请求头
        add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization' always;
        
        # 预检请求缓存时间
        add_header 'Access-Control-Max-Age' '86400' always;
        
        # 处理预检请求
        if ($request_method = 'OPTIONS') {
            return 204;
        }
        
        proxy_pass http://localhost:8000;
    }
}
```

**关联文档：** [[Full-Stack_AI_Engineering/05-AI云原生部署与托管实战(2026)\|05-AI云原生部署与托管实战(2026)]] - Nginx 网关配置

---

### 4.4 2026 全栈架构的经济与安全博弈

在完成 Demo 之后，企业在正式上线时通常会面临"跨域"还是"同源"的决策。

#### 为什么企业倾向于"全家桶"部署（如同选阿里云）？

虽然 Vercel 的前端体验极佳，但企业往往会将前后端都迁入同一个云生态，原因在于：

- **内网互通 (Intranet)**：同一厂商的服务器之间走的是内部带宽。数据传输不经过公网，速度接近物理极限，且**安全性极高**。
- **流量成本**：云厂商通常对"入方向"流量免费，但对"出方向"公网流量收费。前后端同源可以大幅减少昂贵的公网带宽支出。
- **绕过 CORS**：利用 Nginx，可以将前端和后端挂在同一个域名下（如 `/` 是前端，`/api` 是后端）。这种**"同源部署"**从根源上消灭了跨域问题，不再需要复杂的配置。

**关联文档：** [[Full-Stack_AI_Engineering/05-AI云原生部署与托管实战(2026)\|05-AI云原生部署与托管实战(2026)]] - 内网互通与流量成本优化

---

## 五、API-First 设计理念

### 5.1 重新定义大模型：API 即后端

在 AI 时代，API 不再仅仅是前后端通信的桥梁，它已经成为**后端逻辑的核心载体**。

- **传统后端**：你自己写逻辑处理数据。
- **AI 后端**：Google (Gemini) 或 OpenAI 帮你写逻辑处理数据。
- **你的任务**：编写一个"中间层"，把用户的需求翻译给大模型，再把大模型的结果翻译给用户。这个中间层，就是你的 **API 服务器**。

**关联文档：** [[Full-Stack_AI_Engineering/03-服务端逻辑与Agent计算环境\|03-服务端逻辑与Agent计算环境]] - API 调用与后端中转

---

### 5.2 API-First 的核心原则

#### 接口优先于实现

在设计系统时，先定义清晰的 API 接口规范，再实现具体的业务逻辑。这样可以：
- 前后端并行开发
- 接口版本管理
- 多端复用（Web、App、小程序）

#### 统一的数据格式

- 使用 JSON 作为标准数据交换格式
- 定义统一的错误响应格式
- 使用 HTTP 状态码表示请求结果

**标准错误响应格式：**
```json
{
  "error": {
    "code": "INVALID_INPUT",
    "message": "用户ID不能为空",
    "details": {}
  }
}
```

---

### 5.3 API 版本管理

随着业务发展，API 可能需要升级。常见的版本管理策略：

- **URL 版本控制**：`/api/v1/users`、`/api/v2/users`
- **Header 版本控制**：`Accept: application/vnd.api+json;version=2`
- **查询参数版本控制**：`/api/users?version=2`

**推荐策略**：URL 版本控制，简单直观，易于理解。

---

## 六、知识要点总结

### Web 三层架构对照表

| **层级** | **物理位置** | **核心技术** | **核心职能** |
|---------|------------|------------|------------|
| **表示层** | 用户浏览器 | HTML/CSS/JS | 视觉呈现与交互响应 |
| **逻辑层** | 云服务器 | Node.js/Python/Java | 业务计算与权限控制 |
| **数据层** | 数据库服务器 | MySQL/MongoDB/OSS | 数据存储与持久化 |

---

### HTTP 协议核心概念

| **概念** | **职能对位** | **物理本质** |
|---------|------------|------------|
| **HTTP 请求** | 前端向后端发送指令 | 通过互联网传输的数据包 |
| **HTTP 响应** | 后端向前端返回结果 | 包含状态码、头部、正文的数据包 |
| **RESTful API** | 标准化的接口设计规范 | 基于 HTTP 动词和资源的接口设计 |
| **JSON** | 数据交换格式 | 轻量级的文本格式，易于解析 |

---

### CORS 机制要点

| **环节** | **参与者** | **核心职能** |
|---------|----------|------------|
| **同源策略** | 浏览器安全机制 | 默认阻止跨域请求 |
| **预检请求** | 浏览器 + 服务器 | 询问服务器是否允许跨域 |
| **CORS 响应头** | 服务器配置 | 声明允许的源、方法、头部 |
| **同源部署** | Nginx 反向代理 | 通过同域名消除跨域问题 |

---

### API-First 设计原则

1. **接口优先**：先定义接口规范，再实现业务逻辑
2. **统一格式**：使用标准化的 JSON 格式和错误响应
3. **版本管理**：通过 URL 或 Header 管理 API 版本
4. **文档完善**：提供清晰的 API 文档和使用示例

---

**下一步学习：**
- 深入学习前端实现：[[Full-Stack_AI_Engineering/02-现代前端工程：从V8到React\|02-现代前端工程：从V8到React]]
- 深入学习后端实现：[[Full-Stack_AI_Engineering/03-服务端逻辑与Agent计算环境\|03-服务端逻辑与Agent计算环境]]
- 学习部署实践：[[Full-Stack_AI_Engineering/05-AI云原生部署与托管实战(2026)\|05-AI云原生部署与托管实战(2026)]]

---
[[Full-Stack_AI_Engineering/Full-Stack_AI_Engineering\|Full-Stack_AI_Engineering]]
