---
{"dg-publish":true,"permalink":"/multi-agent-workflow-framework/high-value-customer-product-analysis-demo/readme/","noteIcon":""}
---


# 高价值客户产品分析 Demo

> 基于多Agent协作工作流框架的数据分析实践案例

## 📋 项目简介

本Demo展示了如何使用多Agent协作框架完成一个完整的数据分析任务：**高价值客户与产品分析**。通过数据分析Agent、脚本编写Agent、审阅Agent的协作，实现了从方案制定、脚本生成、执行审阅到结果反脱敏的全流程自动化。

## 🎯 分析目标

- 识别高价值客户与产品（产生实际收益的关键群体）
- 识别低价值客户与产品（资源消耗大但收益有限的群体）
- 发现高低价值群体的特征差异，为策略调整提供决策依据
- 通过结果反脱敏，实现业务层面的直接落地与验证

## 📁 目录结构

```
high-value-customer-product-analysis-demo/
├── README.md                          # 本文件：Demo简介
├── 高价值客户产品分析实验记录.md      # 详细实验流程记录
├── data/                              # 数据目录
│   ├── 脱敏数据.xlsx                  # 原始脱敏数据
│   ├── 脱敏数据_增强版.xlsx           # 数据增强后的数据（分析使用）
│   ├── 脱敏数据增强版数据信息.md      # 数据特征描述文档
│   └── 映射记录.json                  # 反脱敏映射表
├── Picture/                           # 交互过程截图
│   └── 图片5-15.png                   # Agent交互过程截图
├── 分析方案/                          # 数据分析师Agent输出
│   └── 高价值客户产品分析方案.md      # 分析框架方案
├── 执行脚本/                          # 脚本编写Agent输出
│   ├── 高价值客户产品分析脚本.py      # 主分析脚本
│   └── 反脱敏脚本.py                  # 反脱敏脚本
├── 脚本审阅报告/                      # 审阅Agent输出
│   ├── 高价值客户产品分析脚本一致性审阅报告.md
│   └── 反脱敏脚本功能分析报告.md
└── 分析结果/                          # 分析结果与报告
    ├── 高价值客户产品分析结果.xlsx    # 分析结果数据
    ├── 高价值客户产品分析报告.md      # 分析报告
    └── 真实结论/                      # 反脱敏后的业务结论
        ├── 高价值客户产品分析报告.md  # 反脱敏后的分析报告
        └── 高价值客户产品分析结果.xlsx # 反脱敏后的分析结果
```

## 🔄 工作流程

```
数据分析Agent → 分析方案制定
     ↓
脚本编写Agent → 脚本生成
     ↓
人工执行 → 本地运行脚本（确保数据安全）
     ↓
审阅Agent → 一致性检查
     ↓
数据分析Agent → 结果分析与报告
     ↓
脚本编写Agent → 反脱敏脚本生成
     ↓
人工执行 + 审阅Agent → 反脱敏结果确认
     ↓
真实业务结论 → 可直接用于业务决策
```

## 🚀 快速开始

### 1. 查看详细实验记录

阅读 [[multi-agent-workflow-framework/high-value-customer-product-analysis-demo/高价值客户产品分析实验记录\|高价值客户产品分析实验记录.md]] 了解完整的实验流程和Agent交互过程。

### 2. 查看分析方案

查看 [[multi-agent-workflow-framework/high-value-customer-product-analysis-demo/分析方案/高价值客户产品分析方案\|分析方案/高价值客户产品分析方案.md]] 了解分析框架的设计思路。

### 3. 运行分析脚本

```bash
cd 执行脚本
python 高价值客户产品分析脚本.py
```

**注意**：脚本会读取 `data/脱敏数据_增强版.xlsx`，输出结果到 `分析结果/高价值客户产品分析结果.xlsx`。

### 4. 查看审阅报告

查看 [脚本审阅报告] 目录下的审阅报告，了解Agent如何检查方案与脚本的一致性。

### 5. 查看业务结论

查看 [分析结果/真实结论/] 目录下的反脱敏后的分析结果，这些是可直接用于业务决策的真实业务标识结果。

## 📊 核心特性

- **文档驱动**：所有Agent通过Markdown文档交互，信息载体统一
- **角色闭环**：设计—执行—审阅—复盘全流程分工明确
- **安全可控**：Agent仅生成脚本，不访问真实数据；执行由人工完成
- **可追溯性**：所有中间产物均保存，确保全流程可追溯

## 📚 相关文档

- **详细实验记录**：[[multi-agent-workflow-framework/high-value-customer-product-analysis-demo/高价值客户产品分析实验记录\|高价值客户产品分析实验记录.md]]
- **分析方案**：[[multi-agent-workflow-framework/high-value-customer-product-analysis-demo/分析方案/高价值客户产品分析方案\|分析方案/高价值客户产品分析方案.md]]
- **数据信息**：[[multi-agent-workflow-framework/high-value-customer-product-analysis-demo/data/脱敏数据增强版数据信息\|data/脱敏数据增强版数据信息.md]]
- **业务结论**：[分析结果/真实结论/] - 反脱敏后的可直接用于业务决策的结果

## 💡 实践价值

本Demo展示了多Agent协作框架在实际数据分析任务中的应用，验证了：
- Agent协作的高效性与可控性
- 文档驱动的协作机制的有效性
- 数据安全与业务落地的平衡

---

**返回主项目**：[[README\|../README.md]]
