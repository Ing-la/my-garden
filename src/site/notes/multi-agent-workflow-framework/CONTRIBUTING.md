---
{"dg-publish":true,"permalink":"/multi-agent-workflow-framework/contributing/","noteIcon":""}
---


# Contributing to Multi-Agent Workflow Framework

感谢您对 Multi-Agent Workflow Framework 项目的关注！我们欢迎所有形式的贡献。

## 如何贡献

### 报告问题

如果您发现了问题或有改进建议，请：

1. 检查 [Issues](https://github.com/Ing-la/multi-agent-workflow-framework/issues) 中是否已有相关问题
2. 如果没有，请创建新的 Issue，并包含：
   - 清晰的问题描述
   - 复现步骤（如果适用）
   - 预期行为 vs 实际行为
   - 环境信息（Python 版本、操作系统等）

### 提交代码

1. **Fork 项目**
   ```bash
   git clone https://github.com/Ing-la/multi-agent-workflow-framework.git
   cd multi-agent-workflow-framework
   ```

2. **创建分支**
   ```bash
   git checkout -b feature/your-feature-name
   # 或
   git checkout -b fix/your-bug-fix
   ```

3. **安装依赖**
   ```bash
   pip install -r requirements.txt
   ```

4. **进行修改**
   - 遵循项目的代码风格
   - 添加必要的注释和文档
   - 确保代码可以正常运行

5. **提交更改**
   ```bash
   git add .
   git commit -m "描述你的更改"
   git push origin feature/your-feature-name
   ```

6. **创建 Pull Request**
   - 在 GitHub 上创建 Pull Request
   - 提供清晰的描述说明你的更改
   - 等待代码审查

## 代码规范

### Python 代码风格

- 遵循 PEP 8 代码风格指南
- 使用有意义的变量和函数名
- 添加必要的文档字符串（docstring）
- 保持函数简洁，单一职责

### 文档规范

- Markdown 文件使用中文编写
- 代码注释使用中文
- 保持文档结构清晰，使用适当的标题层级

### 提交信息规范

提交信息应该清晰描述更改内容：

- `feat: 添加新功能`
- `fix: 修复bug`
- `docs: 更新文档`
- `refactor: 代码重构`
- `style: 代码格式调整`
- `test: 添加测试`

## 项目结构

```
.
├── README.md                    # 项目主文档
├── AI实践.md                    # 实践文档
├── Appendix/                    # 子文档目录
├── high-value-customer-product-analysis-demo/  # 实践案例
└── requirements.txt            # 依赖文件
```

## 开发环境

- Python 3.8+
- 推荐使用虚拟环境
- 安装依赖：`pip install -r requirements.txt`

## 问题反馈

如果您有任何问题或建议，欢迎：

- 提交 [Issue](https://github.com/Ing-la/multi-agent-workflow-framework/issues)
- 发起讨论

再次感谢您的贡献！







