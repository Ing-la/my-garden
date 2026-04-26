---
{"dg-publish":true,"permalink":"/multi-agent-workflow-framework/multi-agent/","noteIcon":""}
---




<div align="center">

![Status](https://img.shields.io/badge/status-Active-success.svg)
![AI](https://img.shields.io/badge/AI-Multi--Agent-orange.svg)
![Python](https://img.shields.io/badge/python-3.8+-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
[![GitHub](https://img.shields.io/badge/GitHub-Repository-brightgreen.svg)](https://github.com/Ing-la/multi-agent-workflow-framework)

**🤖 一个基于多Agent协作的数据分析工作流框架实践**

通过规范化的角色分工、文档驱动的协作机制，实现高效、可控、可追溯的AI赋能数据分析体系

[📖 文档](#文档导航) • [🚀 快速开始](#快速开始) • [💡 核心特性](#核心特性) • [🏗️ 架构设计](#架构设计)

---

**Keywords:** `Multi-Agent Systems` `Agentic AI` `Workflow Orchestration` `Data Analysis` `AI Collaboration` `Document-Driven` `Enterprise AI`

</div>

---

## 📋 项目简介

本项目是一个**多Agent协作实践框架**，旨在探索如何利用AI驱动的智能协作机制，重塑数据分析与业务研发的工作方式。通过构建一套高效、可控、透明、可复现的多Agent协作分析流程，实现从业务框架制定到脚本执行、结果审阅的全流程自动化协作。

### 🎯 核心价值

- **规范化协作**：通过明确的角色分工，实现设计—执行—审阅—复盘的完整闭环
- **文档驱动**：以Markdown为统一信息载体，确保上下文一致、过程可追溯
- **安全可控**：Agent仅生成脚本，不访问真实数据，执行由人工完成
- **知识沉淀**：每个环节产出均有留痕，形成可复用的企业级知识模板

## 🏗️ 架构设计

### 角色定义

| 角色 | 职责说明 |
|------|-----------|
| **数据分析Agent（DataAnalyst）** | 设计分析框架，拆解任务，制定需求清单，汇总结果并提炼洞察 |
| **脚本编写Agent（ScriptWriter）** | 根据方案编写最小可执行脚本（MVP Script），仅生成代码，不执行、不访问真实数据 |
| **审阅Agent（Reviewer）** | 核查脚本与方案一致性，识别新增、遗漏或偏差，输出审阅结论与修正建议 |
| **人工环节（Human）** | 负责数据安全、脚本执行、结果判定与最终确认 |

### 工作流程

```
┌─────────────────┐
│ 业务流程框架制定 │ (DataAnalyst)
└────────┬────────┘
         │
┌────────▼────────┐
│ 阶段性方案制定   │ (DataAnalyst)
└────────┬────────┘
         │
┌────────▼────────┐
│ 脚本编写与执行   │ (ScriptWriter + Human)
└────────┬────────┘
         │
┌────────▼────────┐
│ 一致性审阅      │ (Reviewer)
└────────┬────────┘
         │
┌────────▼────────┐
│ 阶段分析与优化   │ (DataAnalyst)
└────────┬────────┘
         │
┌────────▼────────┐
│ 最终汇总与沉淀   │ (DataAnalyst + Human)
└─────────────────┘
```

## ✨ 核心特性

### 🔒 数据安全与控制机制

| 环节 | 措施 |
|------|------|
| 数据访问 | Agent仅生成脚本，不运行、不读取真实数据 |
| 脚本执行 | 仅人工在本地执行，确保安全 |
| 审阅机制 | 独立审阅Agent检查方案与脚本一致性 |
| 信息留痕 | 所有阶段产出均以Markdown文档保存，可追溯可复盘 |

### 📊 流程特征

1. **文档驱动**：所有Agent通过Markdown交互，信息载体统一、上下文一致
2. **角色闭环**：设计—执行—审阅—复盘全流程分工明确、职责独立
3. **安全可控**：Agent不访问真实数据；执行由人工完成；结果可追溯
4. **可复用性高**：每个环节产出均有留痕，形成标准模板与知识沉淀

## 📋 环境要求

- **Python**: 3.8 或更高版本
- **依赖包**:
  - pandas >= 1.5.0
  - numpy >= 1.23.0
  - openpyxl >= 3.0.0
  - matplotlib >= 3.6.0 (可选，用于可视化)

## 🔧 安装

### 1. 克隆项目

```bash
git clone https://github.com/Ing-la/multi-agent-workflow-framework.git
cd multi-agent-workflow-framework
```

### 2. 安装依赖

```bash
pip install -r requirements.txt
```

### 3. 验证安装

```bash
python --version  # 应显示 Python 3.8+
pip list | grep pandas  # 应显示 pandas 版本
```

## 🚀 快速开始

### 项目结构

```
.
├── README.md                          # 项目说明文档
├── AI实践.md                          # 主实践文档
├── Appendix/                          # 子文档目录
│   ├── 框架方案.md                    # 业务框架方案流程
│   ├── 2.3流程复现.md                 # 完整流程复现示例
│   ├── 数据分析师agent阶段汇总.md     # 阶段汇总文档
│   └── ...                            # 其他相关文档
├── high-value-customer-product-analysis-demo/  # 实践案例Demo
│   └── README.md                      # Demo说明文档
└── Picture/                           # 图片资源目录
    └── ...                            # 流程截图与示意图
```

### 使用指南

1. **阅读主文档**：从 [[AI实践|`AI实践.md`]] 开始，了解整体框架设计
2. **查看实践案例**：参考 [[README|`high-value-customer-product-analysis-demo/README.md`]] 了解完整的数据分析实践案例
3. **查看流程复现**：参考 [[222流程复现|`Appendix/2.3流程复现.md`]] 了解具体执行过程
4. **学习框架制定**：参考 [[框架方案|`Appendix/框架方案.md`]] 了解业务框架制定逻辑
5. **理解阶段汇总**：参考 [[ab数据分析师agent阶段汇总|`Appendix/数据分析师agent阶段汇总.md`]] 了解多阶段协作机制

## 📚 文档导航

### 主要文档

- **[[multi-agent-workflow-framework/AI实践\|AI实践.md]]** - 完整的框架说明与实践总结
- **[[multi-agent-workflow-framework/high-value-customer-product-analysis-demo/b6高价值客户产品分析实验记录\|b6高价值客户产品分析实验记录]]** - 完整的数据分析实践案例
- **[[multi-agent-workflow-framework/Appendix/框架方案\|框架方案.md]]** - 业务分析框架的制定逻辑与规范化方向
- **[[multi-agent-workflow-framework/Appendix/222流程复现\|2.3流程复现.md]]** - 从方案生成到脚本执行、结果审阅的全过程展示
- **[[multi-agent-workflow-framework/Appendix/ab数据分析师agent阶段汇总\|数据分析师agent阶段汇总.md]]** - 各阶段结果汇总与业务框架维护过程
- **[[multi-agent-workflow-framework/Appendix/prompt-rule工程\|Prompt/Rule工程实践]]** - Agent角色规范的模板与实践参考

### 详细文档

- **实践案例**：更多详细的实验记录、脚本、审阅报告等，请查看 [`high-value-customer-product-analysis-demo/`]目录
- **框架文档**：更多详细的执行文档、脚本审阅报告、分析结论等，请查看 [`Appendix/`] 目录

## 🎓 经验与思考

1. **AI的知识储备远超个人经验**，应充分利用
2. **人工控制与Agent放权需平衡**，过度干预会限制AI能力
3. **Rule即培训机制**——为Agent制定明确规范，成本远低于培训员工
4. **Agent也会"幻觉"**：输入不完整时会自行补全；规范流程可降低风险
5. **知识库建设是核心资产**：流程规范与经验沉淀可转化为可复用智能资产

## 🚀 实践案例

### Ops Risk Analytics - 完整应用开发案例

[**Ops Risk Analytics**](https://github.com/Ing-la/automated-ops-reporting) 是一个使用本框架方法论开发的完整数据分析自动化应用，展示了AI辅助开发在实际项目中的成功应用。

**项目特点**：
- ✅ **完整端到端实现**：从数据处理、指标计算、可视化到LLM分析、报告生成、OSS上传、飞书推送的完整流程
- ✅ **前后端架构**：包含GUI界面和命令行两种使用方式
- ✅ **高效开发**：使用Cursor等AI协作工具，在探索AI辅助开发的过程中，3天内完成从需求分析到系统上线的完整开发周期
- ✅ **生产可用**：已部署使用，支持月度自动化分析报告生成

**与本框架的关系**：
- 本项目（Multi-Agent Workflow Framework）提供**方法论和协作框架**
- Ops Risk Analytics 展示了**方法论在实际项目中的应用效果**
- 两者形成从"如何思考"到"如何实现"的完整链条

> 💡 如果您想了解如何将AI协作方法论应用到实际项目中，欢迎查看 [Ops Risk Analytics](https://github.com/Ing-la/automated-ops-reporting) 项目。

## 🗺️ 落地路径

### 短期目标：培养标准化Agent
按业务场景定制Rule，明确输入输出规范，建立统一模板，形成可直接协作的"虚拟员工"。

### 长期目标：构建企业级智能Agent体系
建立公司级知识库，统一语义框架与上下文，优化Agent决策逻辑，让Agent具备企业经验与持续学习能力。

## 💬 反馈与交流

本项目为实践案例分享，欢迎：

- ⭐ [Star 本项目](https://github.com/Ing-la/multi-agent-workflow-framework)
- 💡 分享您的实践经验和改进建议
- 🐛 [提交 Issue](https://github.com/Ing-la/multi-agent-workflow-framework/issues) 反馈问题或讨论

---

<div align="center">

**如果这个项目对您有帮助，请给一个 ⭐ Star！**

Made with ❤️ by [Ing-la](https://github.com/Ing-la)

</div>

