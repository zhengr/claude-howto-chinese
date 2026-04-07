---
name: code-review-specialist
description: Comprehensive code review with security, performance, and quality analysis. Use when users ask to review code, analyze code quality, evaluate pull requests, or mention code review, security analysis, or performance optimization.
---

# 代码审查技能

本技能提供全面的代码审查功能，重点关注：

1. **安全分析**
   - 认证/授权问题
   - 数据暴露风险
   - 注入漏洞
   - 密码学弱点
   - 敏感数据日志

2. **性能审查**
   - 算法效率（大 O 分析）
   - 内存优化
   - 数据库查询优化
   - 缓存机会
   - 并发问题

3. **代码质量**
   - SOLID 原则
   - 设计模式
   - 命名约定
   - 文档
   - 测试覆盖率

4. **可维护性**
   - 代码可读性
   - 函数大小（应小于 50 行）
   - 圈复杂度
   - 依赖管理
   - 类型安全

## 审查模板

对于每个被审查的代码，提供：

### 摘要
- 总体质量评估（1-5 分）
- 关键发现数量
- 建议的优先级领域

### 关键问题（如有）
- **问题**：清晰的描述
- **位置**：文件和行号
- **影响**：为什么这很重要
- **严重性**：关键/高/中
- **修复**：代码示例

### 按类别划分的发现

#### 安全性（如有问题）
列出带有示例的安全漏洞

#### 性能（如有问题）
列出带有复杂度分析的性能问题

#### 质量（如有问题）
列出代码质量问题及重构建议

#### 可维护性（如有问题）
列出可维护性问题及改进建议

## 版本历史

- v1.0.0 (2024-12-10)：初始发布，包含安全性、性能、质量和可维护性分析
