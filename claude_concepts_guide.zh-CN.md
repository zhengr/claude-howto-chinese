<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude 概念完整指南

涵盖斜杠命令、子代理、记忆、MCP 协议和代理技能的综合参考指南，包含表格、图表和实用示例。

---

## 目录

1. [斜杠命令](#斜杠命令)
2. [子代理](#子代理)
3. [记忆](#记忆)
4. [MCP 协议](#mcp-协议)
5. [代理技能](#代理技能)
6. [插件](#claude-code-插件)
7. [钩子](#钩子)
8. [检查点和回溯](#检查点和回溯)
9. [高级功能](#高级功能)
10. [比较与集成](#比较与集成)

---

## 斜杠命令

### 概述

斜杠命令是用户调用的快捷方式，以 Markdown 文件存储，Claude Code 可以执行它们。它们使团队能够标准化常用的提示词和工作流程。

### 架构

```mermaid
graph TD
    A["用户输入: /command-name"] -->|触发| B["搜索 .claude/commands/"]
    B -->|找到| C["command-name.md"]
    C -->|加载| D["Markdown 内容"]
    D -->|执行| E["Claude 处理提示词"]
    E -->|返回| F["上下文中的结果"]
```

### 文件结构

```mermaid
graph LR
    A["项目根目录"] -->|包含| B[".claude/commands/"]
    B -->|包含| C["optimize.md"]
    B -->|包含| D["test.md"]
    B -->|包含| E["docs/"]
    E -->|包含| F["generate-api-docs.md"]
    E -->|包含| G["generate-readme.md"]
```

### 命令组织表

| 位置 | 范围 | 可用性 | 用例 | Git 跟踪 |
|----------|-------|--------------|----------|-------------|
| `.claude/commands/` | 项目特定 | 团队成员 | 团队工作流程、共享标准 | ✅ 是 |
| `~/.claude/commands/` | 个人 | 个人用户 | 跨项目的个人快捷方式 | ❌ 否 |
| 子目录 | 命名空间 | 基于父级 | 按类别组织 | ✅ 是 |

### 功能与能力

| 功能 | 示例 | 支持 |
|---------|---------|-----------|
| Shell 脚本执行 | `bash scripts/deploy.sh` | ✅ 是 |
| 文件引用 | `@path/to/file.js` | ✅ 是 |
| Bash 集成 | `$(git log --oneline)` | ✅ 是 |
| 参数 | `/pr --verbose` | ✅ 是 |
| MCP 命令 | `/mcp__github__list_prs` | ✅ 是 |

### 实用示例

#### 示例 1：代码优化命令

**文件：** `.claude/commands/optimize.md`

```markdown
---
name: Code Optimization
description: 分析代码的性能问题并建议优化
tags: performance, analysis
---

# 代码优化

按优先级审查提供的代码是否存在以下问题：

1. **性能瓶颈** - 识别 O(n²) 操作、低效循环
2. **内存泄漏** - 查找未释放的资源、循环引用
3. **算法改进** - 建议更好的算法或数据结构
4. **缓存机会** - 识别重复计算
5. **并发问题** - 查找竞态条件或线程问题

按以下格式响应：
- 问题严重性（严重/高/中/低）
- 代码位置
- 说明
- 建议的修复方案及代码示例
```

**用法：**
```bash
# 用户在 Claude Code 中输入
/optimize

# Claude 加载提示词并等待代码输入
```

#### 示例 2：Pull Request 辅助命令

**文件：** `.claude/commands/pr.md`

```markdown
---
name: Prepare Pull Request
description: 清理代码、暂存更改并准备 pull request
tags: git, workflow
---

# Pull Request 准备清单

在创建 PR 之前，执行以下步骤：

1. 运行代码检查：`prettier --write .`
2. 运行测试：`npm test`
3. 审查 git diff：`git diff HEAD`
4. 暂存更改：`git add .`
5. 按照约定式提交创建提交消息：
   - `fix:` 用于 bug 修复
   - `feat:` 用于新功能
   - `docs:` 用于文档
   - `refactor:` 用于代码重构
   - `test:` 用于测试添加
   - `chore:` 用于维护

6. 生成 PR 摘要，包括：
   - 更改了什么
   - 为什么更改
   - 执行的测试
   - 潜在影响
```

**用法：**
```bash
/pr

# Claude 运行清单并准备 PR
```

#### 示例 3：分层文档生成器

**文件：** `.claude/commands/docs/generate-api-docs.md`

```markdown
---
name: Generate API Documentation
description: 从源代码创建全面的 API 文档
tags: documentation, api
---

# API 文档生成器

通过以下方式生成 API 文档：

1. 扫描 `/src/api/` 中的所有文件
2. 提取函数签名和 JSDoc 注释
3. 按端点/模块组织
4. 创建包含示例的 markdown
5. 包含请求/响应模式
6. 添加错误文档

输出格式：
- `/docs/api.md` 中的 Markdown 文件
- 包含所有端点的 curl 示例
- 添加 TypeScript 类型
```

### 命令生命周期图

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant FS as File System
    participant CLI as Shell/Bash

    User->>Claude: 输入 /optimize
    Claude->>FS: 搜索 .claude/commands/
    FS-->>Claude: 返回 optimize.md
    Claude->>Claude: 加载 Markdown 内容
    Claude->>User: 显示提示词上下文
    User->>Claude: 提供要分析的代码
    Claude->>CLI: （可能执行脚本）
    CLI-->>Claude: 结果
    Claude->>User: 返回分析
```

### 最佳实践

| ✅ 应该 | ❌ 不应该 |
|------|---------|
| 使用清晰、以行动为导向的名称 | 为一次性任务创建命令 |
| 在描述中记录触发词 | 在命令中构建复杂逻辑 |
| 保持命令专注于单个任务 | 创建冗余命令 |
| 对项目命令进行版本控制 | 硬编码敏感信息 |
| 在子目录中组织 | 创建长命令列表 |
| 使用简单、可读的提示词 | 使用缩写或晦涩的措辞 |

---

## 子代理

### 概述

子代理是具有独立上下文窗口和自定义系统提示词的专业化 AI 助手。它们在保持清晰关注点分离的同时实现委托任务执行。

### 架构图

```mermaid
graph TB
    User["👤 用户"]
    Main["🎯 主代理<br/>(协调器)"]
    Reviewer["🔍 代码审查员<br/>子代理"]
    Tester["✅ 测试工程师<br/>子代理"]
    Docs["📝 文档<br/>子代理"]

    User -->|请求| Main
    Main -->|委托| Reviewer
    Main -->|委托| Tester
    Main -->|委托| Docs
    Reviewer -->|返回结果| Main
    Tester -->|返回结果| Main
    Docs -->|返回结果| Main
    Main -->|综合| User
```

### 子代理生命周期

```mermaid
sequenceDiagram
    participant User
    participant MainAgent as 主代理
    participant CodeReviewer as 代码审查员<br/>子代理
    participant Context as 独立<br/>上下文窗口

    User->>MainAgent: "构建新的认证功能"
    MainAgent->>MainAgent: 分析任务
    MainAgent->>CodeReviewer: "审查此代码"
    CodeReviewer->>Context: 初始化干净上下文
    Context->>CodeReviewer: 加载审查员指令
    CodeReviewer->>CodeReviewer: 执行审查
    CodeReviewer-->>MainAgent: 返回发现
    MainAgent->>MainAgent: 整合结果
    MainAgent-->>User: 提供综合结果
```

### 子代理配置表

| 配置 | 类型 | 目的 | 示例 |
|---------------|------|---------|---------|
| `name` | String | 代理标识符 | `code-reviewer` |
| `description` | String | 目的和触发词 | `全面的代码质量分析` |
| `tools` | List/String | 允许的能力 | `read, grep, diff, lint_runner` |
| `system_prompt` | Markdown | 行为指令 | 自定义指南 |

### 工具访问层次

```mermaid
graph TD
    A["子代理配置"] -->|选项 1| B["继承所有工具<br/>从主线程"]
    A -->|选项 2| C["指定单个工具"]
    B -->|包括| B1["文件操作"]
    B -->|包括| B2["Shell 命令"]
    B -->|包括| B3["MCP 工具"]
    C -->|显式列表| C1["read, grep, diff"]
    C -->|显式列表| C2["Bash(npm:*), Bash(test:*)"]
```

### 实用示例

#### 示例 1：完整的子代理设置

**文件：** `.claude/agents/code-reviewer.md`

```yaml
---
name: code-reviewer
description: 全面的代码质量和可维护性分析
tools: read, grep, diff, lint_runner
---

# 代码审查员代理

您是专注于以下方面的专家代码审查员：
- 性能优化
- 安全漏洞
- 代码可维护性
- 测试覆盖率
- 设计模式

## 审查优先级（按顺序）

1. **安全问题** - 认证、授权、数据泄露
2. **性能问题** - O(n²) 操作、内存泄漏、低效查询
3. **代码质量** - 可读性、命名、文档
4. **测试覆盖率** - 缺失的测试、边界情况
5. **设计模式** - SOLID 原则、架构

## 审查输出格式

对于每个问题：
- **严重性**：严重 / 高 / 中 / 低
- **类别**：安全 / 性能 / 质量 / 测试 / 设计
- **位置**：文件路径和行号
- **问题描述**：问题是什么以及为什么
- **建议的修复**：代码示例
- **影响**：这如何影响系统

## 审查示例

### 问题：N+1 查询问题
- **严重性**：高
- **类别**：性能
- **位置**：src/user-service.ts:45
- **问题**：循环在每次迭代中执行数据库查询
- **修复**：使用 JOIN 或批量查询
```

**文件：** `.claude/agents/test-engineer.md`

```yaml
---
name: test-engineer
description: 测试策略、覆盖率分析和自动化测试
tools: read, write, bash, grep
---

# 测试工程师代理

您擅长：
- 编写全面的测试套件
- 确保高代码覆盖率（>80%）
- 测试边界情况和错误场景
- 性能基准测试
- 集成测试

## 测试策略

1. **单元测试** - 单个函数/方法
2. **集成测试** - 组件交互
3. **端到端测试** - 完整工作流程
4. **边界情况** - 边界条件
5. **错误场景** - 故障处理

## 测试输出要求

- 使用 Jest 进行 JavaScript/TypeScript 测试
- 为每个测试包含设置/清理
- 模拟外部依赖
- 记录测试目的
- 在相关时包含性能断言

## 覆盖率要求

- 最低 80% 代码覆盖率
- 关键路径 100% 覆盖
- 报告缺失的覆盖率区域
```

**文件：** `.claude/agents/documentation-writer.md`

```yaml
---
name: documentation-writer
description: 技术文档、API 文档和用户指南
tools: read, write, grep
---

# 文档编写员代理

您创建：
- 带示例的 API 文档
- 用户指南和教程
- 架构文档
- 变更日志条目
- 代码注释改进

## 文档标准

1. **清晰度** - 使用简单、清晰的语言
2. **示例** - 包含实用的代码示例
3. **完整性** - 覆盖所有参数和返回值
4. **结构** - 使用一致的格式
5. **准确性** - 根据实际代码验证

## 文档部分

### 对于 API
- 描述
- 参数（带类型）
- 返回值（带类型）
- 抛出（可能的错误）
- 示例（curl、JavaScript、Python）
- 相关端点

### 对于功能
- 概述
- 先决条件
- 分步说明
- 预期结果
- 故障排查
- 相关主题
```

#### 示例 2：子代理委托实战

```markdown
# 场景：构建支付功能

## 用户请求

"构建与 Stripe 集成的安全支付处理功能"

## 主代理流程

1. **规划阶段**
   - 理解需求
   - 确定所需任务
   - 规划架构

2. **委托给代码审查员子代理**
   - 任务："审查支付处理实现的安全性"
   - 上下文：认证、API 密钥、令牌处理
   - 审查：SQL 注入、密钥泄露、HTTPS 强制

3. **委托给测试工程师子代理**
   - 任务："为支付流程创建全面的测试"
   - 上下文：成功场景、失败、边界情况
   - 创建测试：有效支付、拒绝的卡、网络故障、webhook

4. **委托给文档编写员子代理**
   - 任务："记录支付 API 端点"
   - 上下文：请求/响应模式
   - 生成：带 curl 示例的 API 文档、错误码

5. **综合**
   - 主代理收集所有输出
   - 整合发现
   - 向用户返回完整解决方案
```

#### 示例 3：工具权限范围

**限制性设置 - 仅限特定命令**

```yaml
---
name: secure-reviewer
description: 具有最小权限的安全导向代码审查
tools: read, grep
---

# 安全代码审查员

仅审查安全漏洞。

此代理：
- ✅ 读取文件进行分析
- ✅ 搜索模式
- ❌ 无法执行代码
- ❌ 无法修改文件
- ❌ 无法运行测试

这确保审查员不会意外破坏任何内容。
```

**扩展设置 - 实现的所有工具**

```yaml
---
name: implementation-agent
description: 用于功能开发的完整实现能力
tools: read, write, bash, grep, edit, glob
---

# 实现代理

根据规范构建功能。

此代理：
- ✅ 读取规范
- ✅ 编写新代码文件
- ✅ 运行构建命令
- ✅ 搜索代码库
- ✅ 编辑现有文件
- ✅ 查找匹配模式的文件

具有独立功能开发的完整能力。
```

### 子代理上下文管理

```mermaid
graph TB
    A["主代理上下文<br/>50,000 tokens"]
    B["子代理 1 上下文<br/>20,000 tokens"]
    C["子代理 2 上下文<br/>20,000 tokens"]
    D["子代理 3 上下文<br/>20,000 tokens"]

    A -->|干净状态| B
    A -->|干净状态| C
    A -->|干净状态| D

    B -->|仅结果| A
    C -->|仅结果| A
    D -->|仅结果| A

    style A fill:#e1f5ff
    style B fill:#fff9c4
    style C fill:#fff9c4
    style D fill:#fff9c4
```

### 何时使用子代理

| 场景 | 使用子代理 | 原因 |
|----------|--------------|-----|
| 多步骤的复杂功能 | ✅ 是 | 分离关注点，防止上下文污染 |
| 快速代码审查 | ❌ 否 | 不必要的开销 |
| 并行任务执行 | ✅ 是 | 每个子代理有自己的上下文 |
| 需要专业知识 | ✅ 是 | 自定义系统提示词 |
| 长时间运行的分析 | ✅ 是 | 防止主上下文耗尽 |
| 单个任务 | ❌ 否 | 不必要地增加延迟 |

### 代理团队

代理团队协调多个代理处理相关任务。与一次委托给一个子代理不同，代理团队允许主代理编排一组代理，这些代理协作、共享中间结果并朝着共同目标工作。这对于大规模任务（如全栈功能开发，前端代理、后端代理和测试代理并行工作）非常有用。

---

## 记忆

### 概述

记忆使 Claude 能够在会话和对话之间保留上下文。它有两种形式：claude.ai 中的自动综合，以及 Claude Code 中基于文件系统的 CLAUDE.md。

### 记忆架构

```mermaid
graph TB
    A["Claude 会话"]
    B["用户输入"]
    C["记忆系统"]
    D["记忆存储"]

    B -->|用户提供信息| C
    C -->|每 24 小时综合| D
    D -->|自动加载| A
    A -->|使用上下文| C
```

### Claude Code 中的记忆层次（7 层）

Claude Code 从 7 层加载记忆，按优先级从高到低排列：

```mermaid
graph TD
    A["1. 托管策略<br/>企业管理员策略"] --> B["2. 项目记忆<br/>./CLAUDE.md"]
    B --> C["3. 项目规则<br/>.claude/rules/*.md"]
    C --> D["4. 用户记忆<br/>~/.claude/CLAUDE.md"]
    D --> E["5. 用户规则<br/>~/.claude/rules/*.md"]
    E --> F["6. 本地记忆<br/>.claude/local/CLAUDE.md"]
    F --> G["7. 自动记忆<br/>自动捕获的偏好"]

    style A fill:#fce4ec,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#e1f5fe,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#f3e5f5,stroke:#333,color:#333
    style F fill:#e8f5e9,stroke:#333,color:#333
    style G fill:#fff3e0,stroke:#333,color:#333
```

### 记忆位置表

| 层级 | 位置 | 范围 | 优先级 | 共享 | 最适合 |
|------|----------|-------|----------|--------|----------|
| 1. 托管策略 | 企业管理员 | 组织 | 最高 | 所有组织用户 | 合规、安全策略 |
| 2. 项目 | `./CLAUDE.md` | 项目 | 高 | 团队（Git） | 团队标准、架构 |
| 3. 项目规则 | `.claude/rules/*.md` | 项目 | 高 | 团队（Git） | 模块化项目约定 |
| 4. 用户 | `~/.claude/CLAUDE.md` | 个人 | 中 | 个人 | 个人偏好 |
| 5. 用户规则 | `~/.claude/rules/*.md` | 个人 | 中 | 个人 | 个人规则模块 |
| 6. 本地 | `.claude/local/CLAUDE.md` | 本地 | 低 | 不共享 | 机器特定设置 |
| 7. 自动记忆 | 自动 | 会话 | 最低 | 个人 | 学习的偏好、模式 |

### 自动记忆

自动记忆自动捕获会话期间观察到的用户偏好和模式。Claude 从您的交互中学习并记住：

- 编码风格偏好
- 您所做的常见更正
- 框架和工具选择
- 沟通风格偏好

自动记忆在后台运行，不需要手动配置。

### 记忆更新生命周期

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Editor as File System
    participant Memory as CLAUDE.md

    User->>Claude: "记住：使用 async/await"
    Claude->>User: "哪个记忆文件？"
    User->>Claude: "项目记忆"
    Claude->>Editor: 打开 ~/.claude/settings.json
    Claude->>Memory: 写入 ./CLAUDE.md
    Memory-->>Claude: 文件已保存
    Claude->>Claude: 加载更新的记忆
    Claude-->>User: "记忆已保存！"
```

### 实用示例

#### 示例 1：项目记忆结构

**文件：** `./CLAUDE.md`

```markdown
# 项目配置

## 项目概述
- **名称**：电子商务平台
- **技术栈**：Node.js、PostgreSQL、React 18、Docker
- **团队规模**：5 名开发人员
- **截止日期**：2025 年第四季度

## 架构

@docs/architecture.md
@docs/api-standards.md
@docs/database-schema.md

## 开发标准

### 代码风格
- 使用 Prettier 进行格式化
- 使用带 airbnb 配置的 ESLint
- 最大行长度：100 个字符
- 使用 2 空格缩进

### 命名约定
- **文件**：kebab-case（user-controller.js）
- **类**：PascalCase（UserService）
- **函数/变量**：camelCase（getUserById）
- **常量**：UPPER_SNAKE_CASE（API_BASE_URL）
- **数据库表**：snake_case（user_accounts）

### Git 工作流程
- 分支名称：`feature/description` 或 `fix/description`
- 提交消息：遵循约定式提交
- 合并前需要 PR
- 所有 CI/CD 检查必须通过
- 至少需要 1 个批准

### 测试要求
- 最低 80% 代码覆盖率
- 所有关键路径必须有测试
- 使用 Jest 进行单元测试
- 使用 Cypress 进行 E2E 测试
- 测试文件名：`*.test.ts` 或 `*.spec.ts`

### API 标准
- 仅限 RESTful 端点
- JSON 请求/响应
- 正确使用 HTTP 状态码
- API 端点版本：`/api/v1/`
- 用示例记录所有端点

### 数据库
- 使用迁移进行模式更改
- 永远不要硬编码凭据
- 使用连接池
- 在开发中启用查询日志
- 需要定期备份

### 部署
- 基于 Docker 的部署
- Kubernetes 编排
- 蓝绿部署策略
- 失败时自动回滚
- 部署前运行数据库迁移

## 常用命令

| 命令 | 目的 |
|---------|---------|
| `npm run dev` | 启动开发服务器 |
| `npm test` | 运行测试套件 |
| `npm run lint` | 检查代码风格 |
| `npm run build` | 构建生产版本 |
| `npm run migrate` | 运行数据库迁移 |

## 团队联系人

- 技术负责人：Sarah Chen (@sarah.chen)
- 产品经理：Mike Johnson (@mike.j)
- DevOps：Alex Kim (@alex.k)

## 已知问题和变通方法

- 高峰时段 PostgreSQL 连接池限制为 20
  - 变通方法：实现查询队列
- Safari 14 与异步生成器的兼容性问题
  - 变通方法：使用 Babel 转译器

## 相关项目

- 分析仪表板：`/projects/analytics`
- 移动应用：`/projects/mobile`
- 管理面板：`/projects/admin`
```

#### 示例 2：目录特定记忆

**文件：** `./src/api/CLAUDE.md`

# API 模块标准

此文件覆盖 /src/api/ 中所有内容的根 CLAUDE.md

## API 特定标准

### 请求验证
- 使用 Zod 进行模式验证
- 始终验证输入
- 返回 400 并附带验证错误
- 包含字段级错误详细信息

### 认证
- 所有端点都需要 JWT 令牌
- 令牌在 Authorization 标头中
- 令牌在 24 小时后过期
- 实现刷新令牌机制

### 响应格式

所有响应必须遵循此结构：

```json
{
  "success": true,
  "data": { /* 实际数据 */ },
  "timestamp": "2025-11-06T10:30:00Z",
  "version": "1.0"
}
```

### 错误响应：

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "用户消息",
    "details": { /* 字段错误 */ }
  },
  "timestamp": "2025-11-06T10:30:00Z"
}
```

### 分页
- 使用基于游标的分页（而非偏移）
- 包含 `hasMore` 布尔值
- 将最大页面大小限制为 100
- 默认页面大小：20

### 速率限制
- 已认证用户每小时 1000 次请求
- 公开端点每小时 100 次请求
- 超出时返回 429
- 包含 retry-after 标头

### 缓存
- 使用 Redis 进行会话缓存
- 缓存持续时间：默认 5 分钟
- 写入操作时失效
- 用资源类型标记缓存键

#### 示例 3：个人记忆

**文件：** `~/.claude/CLAUDE.md`

```markdown
# 我的开发偏好

## 关于我
- **经验水平**：8 年全栈开发
- **首选语言**：TypeScript、Python
- **沟通风格**：直接，带示例
- **学习风格**：带代码的可视化图表

## 代码偏好

### 错误处理

我更喜欢使用 try-catch 块和有意义的错误消息进行显式错误处理。
避免通用错误。始终记录错误以进行调试。

### 注释

使用注释说明"为什么"，而不是"什么"。代码应该是自文档化的。
注释应该解释业务逻辑或不明显的决策。

### 测试

我更喜欢 TDD（测试驱动开发）。
先编写测试，然后实现。
关注行为，而不是实现细节。

### 架构

我更喜欢模块化、松耦合的设计。
使用依赖注入以提高可测试性。
分离关注点（控制器、服务、存储库）。

## 调试偏好
- 使用带前缀的 console.log：`[DEBUG]`
- 包含上下文：函数名、相关变量
- 可用时使用堆栈跟踪
- 始终在日志中包含时间戳

## 沟通
- 用图表解释复杂概念
- 在解释理论之前展示具体示例
- 包含之前/之后的代码片段
- 在最后总结关键点

## 项目组织

我按以下方式组织项目：
```
project/
├── src/
│   ├── api/
│   ├── services/
│   ├── models/
│   └── utils/
├── tests/
├── docs/
└── docker/
```

## 工具
- **IDE**：带 vim 键绑定的 VS Code
- **终端**：带 Oh-My-Zsh 的 Zsh
- **格式化**：Prettier（100 字符行长度）
- **代码检查**：带 airbnb 配置的 ESLint
- **测试框架**：带 React Testing Library 的 Jest
```

#### 示例 4：会话期间的记忆更新

**会话交互：**

```markdown
用户：记住我更喜欢对所有新组件使用 React hooks 而不是类组件。

Claude：我正在将其添加到您的记忆中。这应该放在哪个记忆文件中？
1. 项目记忆 (./CLAUDE.md)
2. 个人记忆 (~/.claude/CLAUDE.md)

用户：项目记忆

Claude：✅ 记忆已保存！

已添加到 ./CLAUDE.md：
---

### 组件开发
- 使用带 React Hooks 的函数组件
- 优先使用 hooks 而不是类组件
- 使用自定义 hooks 处理可重用逻辑
- 对事件处理程序使用 useCallback
- 对昂贵计算使用 useMemo
```

### Claude Web/Desktop 中的记忆

#### 记忆综合时间线

```mermaid
graph LR
    A["第 1 天：用户<br/>对话"] -->|24 小时| B["第 2 天：记忆<br/>综合"]
    B -->|自动| C["记忆已更新<br/>已总结"]
    C -->|加载到| D["第 2-N 天：<br/>新对话"]
    D -->|添加到| E["记忆"]
    E -->|24 小时后| F["记忆已刷新"]
```

**示例记忆摘要：**

```markdown
## Claude 对用户的记忆

### 专业背景
- 拥有 8 年经验的高级全栈开发人员
- 专注于 TypeScript/Node.js 后端和 React 前端
- 活跃的开源贡献者
- 对 AI 和机器学习感兴趣

### 项目背景
- 目前正在构建电子商务平台
- 技术栈：Node.js、PostgreSQL、React 18、Docker
- 与 5 名开发人员的团队合作
- 使用 CI/CD 和蓝绿部署

### 沟通偏好
- 喜欢直接、简洁的解释
- 喜欢可视图表和示例
- 欣赏代码片段
- 在注释中解释业务逻辑

### 当前目标
- 提高 API 性能
- 将测试覆盖率提高到 90%
- 实现缓存策略
- 记录架构
```

### 记忆功能比较

| 功能 | Claude Web/Desktop | Claude Code (CLAUDE.md) |
|---------|-------------------|------------------------|
| 自动综合 | ✅ 每 24 小时 | ❌ 手动 |
| 跨项目 | ✅ 共享 | ❌ 项目特定 |
| 团队访问 | ✅ 共享项目 | ✅ Git 跟踪 |
| 可搜索 | ✅ 内置 | ✅ 通过 `/memory` |
| 可编辑 | ✅ 在聊天中 | ✅ 直接文件编辑 |
| 导入/导出 | ✅ 是 | ✅ 复制/粘贴 |
| 持久化 | ✅ 24 小时+ | ✅ 无限期 |

---

## MCP 协议

### 概述

MCP（模型上下文协议）是 Claude 访问外部工具、API 和实时数据源的标准方式。与记忆不同，MCP 提供对变化数据的实时访问。

### MCP 架构

```mermaid
graph TB
    A["Claude"]
    B["MCP 服务器"]
    C["外部服务"]

    A -->|请求: list_issues| B
    B -->|查询| C
    C -->|数据| B
    B -->|响应| A

    A -->|请求: create_issue| B
    B -->|操作| C
    C -->|结果| B
    B -->|响应| A
```

### MCP 生态系统

```mermaid
graph TB
    A["Claude"] -->|MCP| B["文件系统<br/>MCP 服务器"]
    A -->|MCP| C["GitHub<br/>MCP 服务器"]
    A -->|MCP| D["数据库<br/>MCP 服务器"]
    A -->|MCP| E["Slack<br/>MCP 服务器"]
    A -->|MCP| F["Google Docs<br/>MCP 服务器"]

    B -->|文件 I/O| G["本地文件"]
    C -->|API| H["GitHub 仓库"]
    D -->|查询| I["PostgreSQL/MySQL"]
    E -->|消息| J["Slack 工作区"]
    F -->|文档| K["Google Drive"]
```

### MCP 设置过程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Config as Config File
    participant Service as External Service

    User->>Claude: 输入 /mcp
    Claude->>Claude: 列出可用的 MCP 服务器
    Claude->>User: 显示选项
    User->>Claude: 选择 GitHub MCP
    Claude->>Config: 更新配置
    Config->>Claude: 激活连接
    Claude->>Service: 测试连接
    Service-->>Claude: 认证成功
    Claude->>User: ✅ MCP 已连接！
```

### 可用 MCP 服务器表

| MCP 服务器 | 目的 | 常用工具 | 认证 | 实时 |
|------------|---------|--------------|------|-----------|
| **文件系统** | 文件操作 | read、write、delete | OS 权限 | ✅ 是 |
| **GitHub** | 仓库管理 | list_prs、create_issue、push | OAuth | ✅ 是 |
| **Slack** | 团队沟通 | send_message、list_channels | 令牌 | ✅ 是 |
| **数据库** | SQL 查询 | query、insert、update | 凭据 | ✅ 是 |
| **Google Docs** | 文档访问 | read、write、share | OAuth | ✅ 是 |
| **Asana** | 项目管理 | create_task、update_status | API 密钥 | ✅ 是 |
| **Stripe** | 支付数据 | list_charges、create_invoice | API 密钥 | ✅ 是 |
| **记忆** | 持久记忆 | store、retrieve、delete | 本地 | ❌ 否 |

### 实用示例

#### 示例 1：GitHub MCP 配置

**文件：** `.mcp.json`（项目范围）或 `~/.claude.json`（用户范围）

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**可用的 GitHub MCP 工具：**

# GitHub MCP 工具

## Pull Request 管理
- `list_prs` - 列出仓库中的所有 PR
- `get_pr` - 获取 PR 详细信息包括 diff
- `create_pr` - 创建新 PR
- `update_pr` - 更新 PR 描述/标题
- `merge_pr` - 将 PR 合并到主分支
- `review_pr` - 添加审查评论

示例请求：
```
/mcp__github__get_pr 456

# 返回：
标题：添加暗模式支持
作者：@alice
描述：使用 CSS 变量实现暗色主题
状态：OPEN
审查者：@bob、@charlie
```

## Issue 管理
- `list_issues` - 列出所有 issues
- `get_issue` - 获取 issue 详细信息
- `create_issue` - 创建新 issue
- `close_issue` - 关闭 issue
- `add_comment` - 向 issue 添加评论

## 仓库信息
- `get_repo_info` - 仓库详细信息
- `list_files` - 文件树结构
- `get_file_content` - 读取文件内容
- `search_code` - 跨代码库搜索

## 提交操作
- `list_commits` - 提交历史
- `get_commit` - 特定提交详细信息
- `create_commit` - 创建新提交

#### 示例 2：数据库 MCP 设置

**配置：**

```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-database"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/mydb"
      }
    }
  }
}
```

**示例用法：**

```markdown
用户：获取订单数超过 10 的所有用户

Claude：我将查询您的数据库以获取该信息。

# 使用 MCP 数据库工具：
SELECT u.*, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id
HAVING COUNT(o.id) > 10
ORDER BY order_count DESC;

# 结果：
- Alice: 15 个订单
- Bob: 12 个订单
- Charlie: 11 个订单
```

#### 示例 3：多 MCP 工作流程

**场景：每日报告生成**

```markdown
# 使用多个 MCP 的每日报告工作流程

## 设置
1. GitHub MCP - 获取 PR 指标
2. 数据库 MCP - 查询销售数据
3. Slack MCP - 发布报告
4. 文件系统 MCP - 保存报告

## 工作流程

### 步骤 1：获取 GitHub 数据
/mcp__github__list_prs completed:true last:7days

输出：
- 总 PR 数：42
- 平均合并时间：2.3 小时
- 审查周转时间：1.1 小时

### 步骤 2：查询数据库
SELECT COUNT(*) as sales, SUM(amount) as revenue
FROM orders
WHERE created_at > NOW() - INTERVAL '1 day'

输出：
- 销售额：247
- 收入：$12,450

### 步骤 3：生成报告
将数据合并为 HTML 报告

### 步骤 4：保存到文件系统
将 report.html 写入 /reports/

### 步骤 5：发布到 Slack
发送摘要到 #daily-reports 频道

最终输出：
✅ 报告已生成并发布
📊 本周合并了 47 个 PR
💰 日销售额 $12,450
```

#### 示例 4：文件系统 MCP 操作

**配置：**

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/user/projects"]
    }
  }
}
```

**可用操作：**

| 操作 | 命令 | 目的 |
|-----------|---------|---------|
| 列出文件 | `ls ~/projects` | 显示目录内容 |
| 读取文件 | `cat src/main.ts` | 读取文件内容 |
| 写入文件 | `create docs/api.md` | 创建新文件 |
| 编辑文件 | `edit src/app.ts` | 修改文件 |
| 搜索 | `grep "async function"` | 在文件中搜索 |
| 删除 | `rm old-file.js` | 删除文件 |

### MCP vs 记忆：决策矩阵

```mermaid
graph TD
    A["需要外部数据？"]
    A -->|否| B["使用记忆"]
    A -->|是| C["频繁变化吗？"]
    C -->|否/很少| B
    C -->|是/经常| D["使用 MCP"]

    B -->|存储| E["偏好<br/>上下文<br/>历史"]
    D -->|访问| F["实时 API<br/>数据库<br/>服务"]

    style B fill:#e1f5ff
    style D fill:#fff9c4
```

### 请求/响应模式

```mermaid
sequenceDiagram
    participant App as Claude
    participant MCP as MCP 服务器
    participant DB as 数据库

    App->>MCP: 请求: "SELECT * FROM users WHERE id=1"
    MCP->>DB: 执行查询
    DB-->>MCP: 结果集
    MCP-->>App: 返回解析后的数据
    App->>App: 处理结果
    App->>App: 继续任务

    Note over MCP,DB: 实时访问<br/>无缓存
```

---

## 代理技能

### 概述

代理技能是可重用的、模型调用的能力，打包为包含指令、脚本和资源的文件夹。Claude 自动检测并使用相关技能。

### 技能架构

```mermaid
graph TB
    A["技能目录"]
    B["SKILL.md"]
    C["YAML 元数据"]
    D["指令"]
    E["脚本"]
    F["模板"]

    A --> B
    B --> C
    B --> D
    E --> A
    F --> A
```

### 技能加载过程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude
    participant System as 系统
    participant Skill as 技能

    User->>Claude: "创建 Excel 报告"
    Claude->>System: 扫描可用技能
    System->>System: 加载技能元数据
    Claude->>Claude: 匹配用户请求到技能
    Claude->>Skill: 加载 xlsx 技能 SKILL.md
    Skill-->>Claude: 返回指令 + 工具
    Claude->>Claude: 执行技能
    Claude->>User: 生成 Excel 文件
```

### 技能类型和位置表

| 类型 | 位置 | 范围 | 共享 | 同步 | 最适合 |
|------|----------|-------|--------|------|----------|
| 预构建 | 内置 | 全局 | 所有用户 | 自动 | 文档创建 |
| 个人 | `~/.claude/skills/` | 个人 | 否 | 手动 | 个人自动化 |
| 项目 | `.claude/skills/` | 团队 | 是 | Git | 团队标准 |
| 插件 | 通过插件安装 | 可变 | 取决于 | 自动 | 集成功能 |

### 预构建技能

```mermaid
graph TB
    A["预构建技能"]
    B["PowerPoint (pptx)"]
    C["Excel (xlsx)"]
    D["Word (docx)"]
    E["PDF"]

    A --> B
    A --> C
    A --> D
    A --> E

    B --> B1["创建演示文稿"]
    B --> B2["编辑幻灯片"]
    C --> C1["创建电子表格"]
    C --> C2["分析数据"]
    D --> D1["创建文档"]
    D --> D2["格式化文本"]
    E --> E1["生成 PDF"]
    E --> E2["填写表单"]
```

### 捆绑技能

Claude Code 现在包含 5 个开箱即用的捆绑技能：

| 技能 | 命令 | 目的 |
|-------|---------|---------|
| **Simplify** | `/simplify` | 简化复杂代码或解释 |
| **Batch** | `/batch` | 跨多个文件或项目运行操作 |
| **Debug** | `/debug` | 系统性调试问题并进行根因分析 |
| **Loop** | `/loop` | 按计时器调度循环任务 |
| **Claude API** | `/claude-api` | 直接与 Anthropic API 交互 |

这些捆绑技能始终可用，不需要安装或配置。

### 实用示例

#### 示例 1：自定义代码审查技能

**目录结构：**

```
~/.claude/skills/code-review/
├── SKILL.md
├── templates/
│   ├── review-checklist.md
│   └── finding-template.md
└── scripts/
    ├── analyze-metrics.py
    └── compare-complexity.py
```

**文件：** `~/.claude/skills/code-review/SKILL.md`

```yaml
---
name: Code Review Specialist
description: 包含安全、性能和质量分析的全面代码审查
version: "1.0.0"
tags:
  - code-review
  - quality
  - security
when_to_use: 当用户要求审查代码、分析代码质量或评估 pull requests 时
effort: high
shell: bash
---

# 代码审查技能

此技能提供全面的代码审查能力，专注于：

1. **安全分析**
   - 认证/授权问题
   - 数据泄露风险
   - 注入漏洞
   - 加密弱点
   - 敏感数据日志记录

2. **性能审查**
   - 算法效率（Big O 分析）
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
   - 函数大小（应 < 50 行）
   - 圈复杂度
   - 依赖管理
   - 类型安全

## 审查模板

对于每个审查的代码，提供：

### 摘要
- 整体质量评估（1-5）
- 关键发现数量
- 建议的优先领域

### 关键问题（如有）
- **问题**：清晰描述
- **位置**：文件和行号
- **影响**：为什么这很重要
- **严重性**：严重/高/中
- **修复**：代码示例

### 按类别发现

#### 安全（如发现问题）
列出安全漏洞及示例

#### 性能（如发现问题）
列出性能问题及复杂度分析

#### 质量（如发现问题）
列出代码质量问题及重构建议

#### 可维护性（如发现问题）
列出可维护性问题及改进建议
```

#### 示例 2：品牌语气技能

**目录结构：**

```
.claude/skills/brand-voice/
├── SKILL.md
├── brand-guidelines.md
├── tone-examples.md
└── templates/
    ├── email-template.txt
    ├── social-post-template.txt
    └── blog-post-template.md
```

**文件：** `.claude/skills/brand-voice/SKILL.md`

```yaml
---
name: Brand Voice Consistency
description: 确保所有沟通符合品牌语气和语调指南
tags:
  - brand
  - writing
  - consistency
when_to_use: 在创建营销文案、客户沟通或面向公众的内容时
---

# 品牌语气技能

## 概述
此技能确保所有沟通保持一致的品牌语气、语调和消息传达。

## 品牌标识

### 使命
帮助团队使用 AI 自动化其开发工作流程

### 价值观
- **简洁**：让复杂事情变简单
- **可靠**：坚固的执行
- **赋能**：激发人类创造力

### 语气
- **友好但专业** - 平易近人但不随意
- **清晰简洁** - 避免行话，简单解释技术概念
- **自信** - 我们知道自己在做什么
- **同理心** - 理解用户需求和痛点

## 写作指南

### 应该 ✅
- 使用"你"称呼读者
- 使用主动语态："Claude 生成报告" 而不是"报告由 Claude 生成"
- 从价值主张开始
- 使用具体示例
- 保持句子少于 20 个字
- 使用列表提高清晰度
- 包含行动号召

### 不应该 ❌
- 不要使用企业行话
- 不要居高临下或过度简化
- 不要使用"我们相信"或"我们认为"
- 除强调外不要使用全大写
- 不要创建大段文字
- 不要假设技术知识

## 词汇

### ✅ 首选术语
- Claude（不是"the Claude AI"）
- 代码生成（不是"自动编码"）
- 代理（不是"机器人"）
- 简化（不是"革命性"）
- 集成（不是"协同"）

### ❌ 避免术语
- "最前沿"（过度使用）
- "颠覆性"（模糊）
- "杠杆"（企业用语）
- "利用"（使用"使用"）
- "范式转变"（不清楚）
```

#### 示例 3：文档生成器技能

**文件：** `.claude/skills/doc-generator/SKILL.md`

```yaml
---
name: API Documentation Generator
description: 从源代码生成全面、准确的 API 文档
version: "1.0.0"
tags:
  - documentation
  - api
  - automation
when_to_use: 在创建或更新 API 文档时
---

# API 文档生成器技能

## 生成内容

- OpenAPI/Swagger 规范
- API 端点文档
- SDK 使用示例
- 集成指南
- 错误代码参考
- 认证指南

## 文档结构

### 对于每个端点

```markdown
## GET /api/v1/users/:id

### 描述
简要说明此端点的功能

### 参数

| 名称 | 类型 | 必需 | 描述 |
|------|------|----------|-------------|
| id | string | 是 | 用户 ID |

### 响应

**200 成功**
```json
{
  "id": "usr_123",
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2025-01-15T10:30:00Z"
}
```

**404 未找到**
```json
{
  "error": "USER_NOT_FOUND",
  "message": "用户不存在"
}
```

### 示例

**cURL**
```bash
curl -X GET "https://api.example.com/api/v1/users/usr_123" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**JavaScript**
```javascript
const user = await fetch('/api/v1/users/usr_123', {
  headers: { 'Authorization': 'Bearer token' }
}).then(r => r.json());
```

**Python**
```python
response = requests.get(
    'https://api.example.com/api/v1/users/usr_123',
    headers={'Authorization': 'Bearer token'}
)
user = response.json()
```
```

### 技能发现与调用

```mermaid
graph TD
    A["用户请求"] --> B["Claude 分析"]
    B -->|扫描| C["可用技能"]
    C -->|元数据检查| D["技能描述匹配？"]
    D -->|是| E["加载 SKILL.md"]
    D -->|否| F["尝试下一个技能"]
    F -->|还有技能？| D
    F -->|没有| G["使用通用知识"]
    E --> H["提取指令"]
    H --> I["执行技能"]
    I --> J["返回结果"]
```

### 技能与其他功能对比

```mermaid
graph TB
    A["扩展 Claude"]
    B["斜杠命令"]
    C["子代理"]
    D["记忆"]
    E["MCP"]
    F["技能"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F

    B -->|用户调用| G["快速快捷方式"]
    C -->|自动委托| H["隔离上下文"]
    D -->|持久化| I["跨会话上下文"]
    E -->|实时| J["外部数据访问"]
    F -->|自动调用| K["自主执行"]
```

---

## Claude Code 插件

### 概述

Claude Code 插件是捆绑的自定义集合（斜杠命令、子代理、MCP 服务器和钩子），可通过单个命令安装。它们代表最高级别的扩展机制—将多个功能组合成连贯的、可共享的包。

### 架构

```mermaid
graph TB
    A["插件"]
    B["斜杠命令"]
    C["子代理"]
    D["MCP 服务器"]
    E["钩子"]
    F["配置"]

    A -->|捆绑| B
    A -->|捆绑| C
    A -->|捆绑| D
    A -->|捆绑| E
    A -->|捆绑| F
```

### 插件加载过程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Plugin as 插件市场
    participant Install as 安装
    participant SlashCmds as 斜杠命令
    participant Subagents
    participant MCPServers as MCP 服务器
    participant Hooks
    participant Tools as 已配置工具

    User->>Claude: /plugin install pr-review
    Claude->>Plugin: 下载插件清单
    Plugin-->>Claude: 返回插件定义
    Claude->>Install: 提取组件
    Install->>SlashCmds: 配置
    Install->>Subagents: 配置
    Install->>MCPServers: 配置
    Install->>Hooks: 配置
    SlashCmds-->>Tools: 准备使用
    Subagents-->>Tools: 准备使用
    MCPServers-->>Tools: 准备使用
    Hooks-->>Tools: 准备使用
    Tools-->>Claude: 插件已安装 ✅
```

### 插件类型与分发

| 类型 | 范围 | 共享 | 权限 | 示例 |
|------|-------|--------|-----------|----------|
| 官方 | 全局 | 所有用户 | Anthropic | PR 审查、安全指南 |
| 社区 | 公开 | 所有用户 | 社区 | DevOps、数据科学 |
| 组织 | 内部 | 团队成员 | 公司 | 内部标准、工具 |
| 个人 | 个人 | 单用户 | 开发者 | 自定义工作流程 |

### 插件定义结构

```yaml
---
name: plugin-name
version: "1.0.0"
description: "插件功能描述"
author: "您的姓名"
license: MIT

# 插件元数据
tags:
  - category
  - use-case

# 依赖
requires:
  - claude-code: ">=1.0.0"

# 捆绑的组件
components:
  - type: commands
    path: commands/
  - type: agents
    path: agents/
  - type: mcp
    path: mcp/
  - type: hooks
    path: hooks/

# 配置
config:
  auto_load: true
  enabled_by_default: true
---
```

### 插件结构

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json
├── commands/
│   ├── task-1.md
│   ├── task-2.md
│   └── workflows/
├── agents/
│   ├── specialist-1.md
│   ├── specialist-2.md
│   └── configs/
├── skills/
│   ├── skill-1.md
│   └── skill-2.md
├── hooks/
│   └── hooks.json
├── .mcp.json
├── .lsp.json
├── settings.json
├── templates/
│   └── issue-template.md
├── scripts/
│   ├── helper-1.sh
│   └── helper-2.py
├── docs/
│   ├── README.md
│   └── USAGE.md
└── tests/
    └── plugin.test.js
```

### 实用示例

#### 示例 1：PR 审查插件

**文件：** `.claude-plugin/plugin.json`

```json
{
  "name": "pr-review",
  "version": "1.0.0",
  "description": "包含安全、测试和文档的完整 PR 审查工作流程",
  "author": {
    "name": "Anthropic"
  },
  "license": "MIT"
}
```

**安装：**

```bash
/plugin install pr-review

# 结果：
# ✅ 已安装 3 个斜杠命令
# ✅ 已配置 3 个子代理
# ✅ 已连接 2 个 MCP 服务器
# ✅ 已注册 4 个钩子
# ✅ 准备使用！
```

#### 示例 2：DevOps 插件

**组件：**

```
devops-automation/
├── commands/
│   ├── deploy.md
│   ├── rollback.md
│   ├── status.md
│   └── incident.md
├── agents/
│   ├── deployment-specialist.md
│   ├── incident-commander.md
│   └── alert-analyzer.md
├── mcp/
│   ├── github-config.json
│   ├── kubernetes-config.json
│   └── prometheus-config.json
├── hooks/
│   ├── pre-deploy.js
│   ├── post-deploy.js
│   └── on-error.js
└── scripts/
    ├── deploy.sh
    ├── rollback.sh
    └── health-check.sh
```

#### 示例 3：文档插件

**捆绑组件：**

```
documentation/
├── commands/
│   ├── generate-api-docs.md
│   ├── generate-readme.md
│   ├── sync-docs.md
│   └── validate-docs.md
├── agents/
│   ├── api-documenter.md
│   ├── code-commentator.md
│   └── example-generator.md
├── mcp/
│   ├── github-docs-config.json
│   └── slack-announce-config.json
└── templates/
    ├── api-endpoint.md
    ├── function-docs.md
    └── adr-template.md
```

### 插件市场

```mermaid
graph TB
    A["插件市场"]
    B["官方<br/>Anthropic"]
    C["社区<br/>市场"]
    D["企业<br/>注册"]

    A --> B
    A --> C
    A --> D

    B -->|分类| B1["开发"]
    B -->|分类| B2["DevOps"]
    B -->|分类| B3["文档"]

    C -->|搜索| C1["DevOps 自动化"]
    C -->|搜索| C2["移动开发"]
    C -->|搜索| C3["数据科学"]

    D -->|内部| D1["公司标准"]
    D -->|内部| D2["遗留系统"]
    D -->|内部| D3["合规"]
```

### 插件安装与生命周期

```mermaid
graph LR
    A["发现"] -->|浏览| B["市场"]
    B -->|选择| C["插件页面"]
    C -->|查看| D["组件"]
    D -->|安装| E["/plugin install"]
    E -->|提取| F["配置"]
    F -->|激活| G["使用"]
    G -->|检查| H["更新"]
    H -->|可用| G
    G -->|完成| I["禁用"]
    I -->|以后| J["启用"]
    J -->|回到| G
```

### 插件功能比较

| 功能 | 斜杠命令 | 技能 | 子代理 | 插件 |
|---------|---------------|-------|----------|--------|
| **安装** | 手动复制 | 手动复制 | 手动配置 | 一条命令 |
| **设置时间** | 5 分钟 | 10 分钟 | 15 分钟 | 2 分钟 |
| **捆绑** | 单个文件 | 单个文件 | 单个文件 | 多个 |
| **版本控制** | 手动 | 手动 | 手动 | 自动 |
| **团队共享** | 复制文件 | 复制文件 | 复制文件 | 安装 ID |
| **更新** | 手动 | 手动 | 手动 | 自动可用 |
| **依赖** | 无 | 无 | 无 | 可能包含 |
| **市场** | 否 | 否 | 否 | 是 |
| **分发** | 仓库 | 仓库 | 仓库 | 市场 |

### 插件用例

| 用例 | 建议 | 原因 |
|----------|-----------------|-----|
| **团队入职** | ✅ 使用插件 | 即时设置，所有配置 |
| **框架设置** | ✅ 使用插件 | 捆绑框架特定命令 |
| **企业标准** | ✅ 使用插件 | 集中分发、版本控制 |
| **快速任务自动化** | ❌ 使用命令 | 过度复杂 |
| **单一领域专长** | ❌ 使用技能 | 太重，使用技能代替 |
| **专业化分析** | ❌ 使用子代理 | 手动创建或使用技能 |
| **实时数据访问** | ❌ 使用 MCP | 独立使用，不捆绑 |

### 何时创建插件

```mermaid
graph TD
    A["我应该创建插件吗？"]
    A -->|需要多个组件| B{需要多个命令<br/>或子代理<br/>或 MCP？}
    B -->|是| C["✅ 创建插件"]
    B -->|否| D["使用单个功能"]
    A -->|团队工作流程| E{与<br/>团队共享？}
    E -->|是| C
    E -->|否| F["保持为本地的设置"]
    A -->|复杂设置| G{需要自动<br/>配置？}
    G -->|是| C
    G -->|否| D
```

### 发布插件

**发布步骤：**

1. 创建包含所有组件的插件结构
2. 编写 `.claude-plugin/plugin.json` 清单
3. 创建带文档的 `README.md`
4. 使用 `/plugin install ./my-plugin` 本地测试
5. 提交到插件市场
6. 审核并通过
7. 在市场上发布
8. 用户可通过一条命令安装

---

## 比较与集成

### 功能比较矩阵

| 功能 | 调用方式 | 持久化 | 范围 | 用例 |
|---------|-----------|------------|-------|----------|
| **斜杠命令** | 手动（`/cmd`） | 仅会话 | 单个命令 | 快速快捷方式 |
| **子代理** | 自动委托 | 隔离上下文 | 专业任务 | 任务分配 |
| **记忆** | 自动加载 | 跨会话 | 用户/团队上下文 | 长期学习 |
| **MCP 协议** | 自动查询 | 实时外部 | 实时数据访问 | 动态信息 |
| **技能** | 自动调用 | 基于文件系统 | 可重用专业知识 | 自动化工作流程 |

### 交互时间线

```mermaid
graph LR
    A["会话开始"] -->|加载| B["记忆 (CLAUDE.md)"]
    B -->|发现| C["可用技能"]
    C -->|注册| D["斜杠命令"]
    D -->|连接| E["MCP 服务器"]
    E -->|就绪| F["用户交互"]

    F -->|输入 /cmd| G["斜杠命令"]
    F -->|请求| H["技能自动调用"]
    F -->|查询| I["MCP 数据"]
    F -->|复杂任务| J["委托给子代理"]

    G -->|使用| B
    H -->|使用| B
    I -->|使用| B
    J -->|使用| B
```

### 完整功能编排

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Memory as 记忆<br/>CLAUDE.md
    participant MCP as MCP 服务器
    participant Skills as 技能
    participant SubAgent as 子代理

    User->>Claude: 请求: "构建认证系统"
    Claude->>Memory: 加载项目标准
    Memory-->>Claude: 认证标准、团队实践
    Claude->>MCP: 查询 GitHub 上的类似实现
    MCP-->>Claude: 代码示例、最佳实践
    Claude->>Skills: 检测匹配技能
    Skills-->>Claude: 安全审查技能 + 测试技能
    Claude->>SubAgent: 委托实现
    SubAgent->>SubAgent: 构建功能
    Claude->>Skills: 应用安全审查技能
    Skills-->>Claude: 安全检查清单结果
    Claude->>SubAgent: 委托测试
    SubAgent-->>Claude: 测试结果
    Claude->>User: 交付完整系统
```

### 何时使用每个功能

```mermaid
graph TD
    A["新任务"] --> B{任务类型？}

    B -->|重复工作流程| C["斜杠命令"]
    B -->|需要实时数据| D["MCP 协议"]
    B -->|下次记住| E["记忆"]
    B -->|专业子任务| F["子代理"]
    B -->|领域特定工作| G["技能"]

    C --> C1["✅ 团队快捷方式"]
    D --> D1["✅ 实时 API 访问"]
    E --> E1["✅ 持久上下文"]
    F --> F1["✅ 并行执行"]
    G --> G1["✅ 自动调用专业知识"]
```

### 选择决策树

```mermaid
graph TD
    Start["需要扩展 Claude？"]

    Start -->|快速重复任务| A{手动还是自动？}
    A -->|手动| B["斜杠命令"]
    A -->|自动| C["技能"]

    Start -->|需要外部数据| D{实时？}
    D -->|是| E["MCP 协议"]
    D -->|否/跨会话| F["记忆"]

    Start -->|复杂项目| G{多个角色？}
    G -->|是| H["子代理"]
    G -->|否| I["技能 + 记忆"]

    Start -->|长期上下文| J["记忆"]
    Start -->|团队工作流程| K["斜杠命令 +<br/>记忆"]
    Start -->|全自动化| L["技能 +<br/>子代理 +<br/>MCP"]
```

---

## 总结表

| 方面 | 斜杠命令 | 子代理 | 记忆 | MCP | 技能 | 插件 |
|--------|---|---|---|---|---|---|
| **设置难度** | 简单 | 中等 | 简单 | 中等 | 中等 | 简单 |
| **学习曲线** | 低 | 中等 | 低 | 中等 | 中等 | 低 |
| **团队收益** | 高 | 高 | 中 | 高 | 高 | 很高 |
| **自动化程度** | 低 | 高 | 中 | 高 | 高 | 很高 |
| **上下文管理** | 单会话 | 隔离 | 持久 | 实时 | 持久 | 所有功能 |
| **维护负担** | 低 | 中 | 低 | 中 | 中 | 低 |
| **可扩展性** | 良好 | 优秀 | 良好 | 优秀 | 优秀 | 优秀 |
| **可共享性** | 一般 | 一般 | 好 | 好 | 好 | 优秀 |
| **版本控制** | 手动 | 手动 | 手动 | 手动 | 手动 | 自动 |
| **安装** | 手动复制 | 手动配置 | 不适用 | 手动配置 | 手动复制 | 一条命令 |

---

## 快速入门指南

### 第 1 周：简单开始
- 为常见任务创建 2-3 个斜杠命令
- 在设置中启用记忆
- 在 CLAUDE.md 中记录团队标准

### 第 2 周：添加实时访问
- 设置 1 个 MCP（GitHub 或数据库）
- 使用 `/mcp` 进行配置
- 在工作流程中查询实时数据

### 第 3 周：分配工作
- 为特定角色创建第一个子代理
- 使用 `/agents` 命令
- 用简单任务测试委托

### 第 4 周：自动化一切
- 为重复自动化创建第一个技能
- 使用技能市场或构建自定义
- 组合所有功能用于完整工作流程

### 持续
- 每月审查和更新记忆
- 模式出现时添加新技能
- 优化 MCP 查询
- 优化子代理提示词

---

## 钩子

### 概述

钩子是事件驱动的命令，响应 Claude Code 事件自动执行。它们无需人工干预即可实现自动化、验证和自定义工作流程。

### 钩子事件

Claude Code 支持 **25 个钩子事件**，跨越四种钩子类型（command、http、prompt、agent）：

| 钩子事件 | 触发条件 | 用例 |
|------------|---------|-----------|
| **SessionStart** | 会话开始/恢复/清理/压缩 | 环境设置、初始化 |
| **InstructionsLoaded** | 加载 CLAUDE.md 或规则文件 | 验证、转换、增强 |
| **UserPromptSubmit** | 用户提交提示 | 输入验证、提示过滤 |
| **PreToolUse** | 工具运行前 | 验证、审批关卡、日志 |
| **PermissionRequest** | 显示权限对话框 | 自动批准/拒绝流程 |
| **PostToolUse** | 工具成功后 | 自动格式化、通知、清理 |
| **PostToolUseFailure** | 工具执行失败 | 错误处理、日志 |
| **Notification** | 发送通知 | 警报、外部集成 |
| **SubagentStart** | 子代理启动 | 上下文注入、初始化 |
| **SubagentStop** | 子代理完成 | 结果验证、日志 |
| **Stop** | Claude 完成响应 | 摘要生成、清理任务 |
| **StopFailure** | API 错误结束回合 | 错误恢复、日志 |
| **TeammateIdle** | 代理团队队友空闲 | 工作分配、协调 |
| **TaskCompleted** | 标记任务完成 | 任务后处理 |
| **TaskCreated** | 通过 TaskCreate 创建任务 | 任务跟踪、日志 |
| **ConfigChange** | 配置文件更改 | 验证、传播 |
| **CwdChanged** | 工作目录更改 | 目录特定设置 |
| **FileChanged** | 监控的文件更改 | 文件监控、重建触发 |
| **PreCompact** | 上下文压缩前 | 状态保留 |
| **PostCompact** | 压缩完成后 | 压缩后操作 |
| **WorktreeCreate** | 正在创建 worktree | 环境设置、依赖安装 |
| **WorktreeRemove** | 正在移除 worktree | 清理、资源释放 |
| **Elicitation** | MCP 服务器请求用户输入 | 输入验证 |
| **ElicitationResult** | 用户响应请求 | 响应处理 |
| **SessionEnd** | 会话终止 | 清理、最终日志 |

### 常用钩子

钩子在 `~/.claude/settings.json`（用户级别）或 `.claude/settings.json`（项目级别）中配置：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write $CLAUDE_FILE_PATH"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "eslint $CLAUDE_FILE_PATH"
          }
        ]
      }
    ]
  }
}
```

### 钩子环境变量

- `$CLAUDE_FILE_PATH` - 正在编辑/写入的文件路径
- `$CLAUDE_TOOL_NAME` - 使用的工具名称
- `$CLAUDE_SESSION_ID` - 当前会话标识符
- `$CLAUDE_PROJECT_DIR` - 项目目录路径

### 最佳实践

✅ **应该：**
- 保持钩子快速（< 1 秒）
- 使用钩子进行验证和自动化
- 优雅处理错误
- 使用绝对路径

❌ **不应该：**
- 使钩子具有交互性
- 使用钩子执行长时间运行的任务
- 硬编码凭据

**参见**：[06-hooks/](06-hooks/) 获取详细示例

---

## 检查点和回溯

### 概述

检查点允许您保存对话状态并回溯到之前的点，实现安全实验和多种方法的探索。

### 关键概念

| 概念 | 描述 |
|---------|-------------|
| **检查点** | 对话状态的快照，包括消息、文件和上下文 |
| **回溯** | 返回到之前的检查点，丢弃后续更改 |
| **分支点** | 从该检查点探索多种方法 |

### 访问检查点

检查点会在每个用户提示时自动创建。要回溯：

```bash
# 按两次 Esc 打开检查点浏览器
Esc + Esc

# 或使用 /rewind 命令
/rewind
```

当您选择检查点时，您有五个选项：
1. **恢复代码和对话** — 两者都回滚到该点
2. **恢复对话** — 回溯消息，保持当前代码
3. **恢复代码** — 回滚文件，保持对话
4. **从此处总结** — 将对话压缩为摘要
5. **算了** — 取消

### 用例

| 场景 | 工作流 |
|----------|----------|
| **探索方法** | 保存 → 尝试 A → 保存 → 回溯 → 尝试 B → 比较 |
| **安全重构** | 保存 → 重构 → 测试 → 失败则：回溯 |
| **A/B 测试** | 保存 → 设计 A → 保存 → 回溯 → 设计 B → 比较 |
| **错误恢复** | 发现问题 → 回溯到最后的好状态 |

### 配置

```json
{
  "autoCheckpoint": true
}
```

**参见**：[08-checkpoints/](08-checkpoints/) 获取详细示例

---

## 高级功能

### 规划模式

在编码之前创建详细的实现计划。

**激活：**
```bash
/plan 实现用户认证系统
```

**好处：**
- 带时间估算的清晰路线图
- 风险评估
- 系统化的任务分解
- 审查和修改的机会

### 深度思考

针对复杂问题的深度推理。

**激活：**
- 在会话中使用 `Alt+T`（macOS 上 `Option+T`）打开
- 设置 `MAX_THINKING_TOKENS` 环境变量进行编程控制

```bash
# 通过环境变量启用深度思考
export MAX_THINKING_TOKENS=50000
claude -p "我们应该使用微服务还是单体？"
```

**好处：**
- 彻底分析权衡
- 更好的架构决策
- 考虑边界情况
- 系统性评估

### 后台任务

运行长时间操作而不阻塞对话。

**用法：**
```bash
用户：在后台运行测试

Claude：已启动任务 bg-1234

/task list           # 显示所有任务
/task status bg-1234 # 检查进度
/task show bg-1234   # 查看输出
/task cancel bg-1234 # 取消任务
```

### 权限模式

控制 Claude 可以做什么。

| 模式 | 描述 | 用例 |
|------|-------------|----------|
| **default** | 标准权限，敏感操作需确认 | 常规开发 |
| **acceptEdits** | 自动接受文件编辑无需确认 | 可信编辑工作流 |
| **plan** | 仅分析和规划，不修改文件 | 代码审查、架构规划 |
| **auto** | 自动批准安全操作，仅对有风险的操作提示 | 平衡的自主性与安全性 |
| **dontAsk** | 执行所有操作无需确认提示 | 经验丰富的用户、自动化 |
| **bypassPermissions** | 完全无限制访问，无安全检查 | CI/CD 流水线、可信脚本 |

**用法：**
```bash
claude --permission-mode plan          # 只读分析
claude --permission-mode acceptEdits   # 自动接受编辑
claude --permission-mode auto          # 自动批准安全操作
claude --permission-mode dontAsk       # 无确认提示
```

### 无头模式（打印模式）

使用 `-p`（打印）标志运行 Claude Code 无需交互输入，用于自动化和 CI/CD。

**用法：**
```bash
# 运行特定任务
claude -p "运行所有测试"

# 管道输入进行分析
cat error.log | claude -p "解释此错误"

# CI/CD 集成（GitHub Actions）
- name: AI 代码审查
  run: claude -p "审查 PR 更改并报告问题"

# 用于脚本的 JSON 输出
claude -p --output-format json "列出 src/ 中的所有函数"
```

### 调度任务

使用 `/loop` 命令按重复计划运行任务。

**用法：**
```bash
/loop every 30m "运行测试并报告失败"
/loop every 2h "检查依赖更新"
/loop every 1d "生成代码更改每日摘要"
```

调度任务在后台运行并在完成时报告结果。它们适用于持续监控、定期检查和自动化维护工作流程。

### Chrome 集成

Claude Code 可以与 Chrome 浏览器集成，以执行 Web 自动化任务。这使得能够在开发工作流程中直接浏览网页、填写表单、截屏和从网站提取数据。

### 会话管理

管理多个工作会话。

**命令：**
```bash
/resume                # 恢复之前的对话
/rename "功能"          # 命名当前会话
/fork                  # 分叉到新会话
claude -c              # 继续最近的对话
claude -r "功能"        # 按名称/ID 恢复会话
```

### 交互功能

**键盘快捷键：**
- `Ctrl + R` - 搜索命令历史
- `Tab` - 自动补全
- `↑ / ↓` - 命令历史
- `Ctrl + L` - 清屏

**多行输入：**
```bash
用户: \
> 长且复杂的提示
> 跨越多行
> \end
```

### 配置

完整的配置示例：

```json
{
  "planning": {
    "autoEnter": true,
    "requireApproval": true
  },
  "extendedThinking": {
    "enabled": true,
    "showThinkingProcess": true
  },
  "backgroundTasks": {
    "enabled": true,
    "maxConcurrentTasks": 5
  },
  "permissions": {
    "mode": "default"
  }
}
```

**参见**：[09-advanced-features/](09-advanced-features/) 获取完整指南

---

## 资源

- [Claude Code 文档](https://code.claude.com/docs/en/overview)
- [Anthropic 文档](https://docs.anthropic.com)
- [MCP GitHub 服务器](https://github.com/modelcontextprotocol/servers)
- [Anthropic 食谱](https://github.com/anthropics/anthropic-cookbook)

---

*最后更新：2026 年 3 月*
*适用于 Claude Haiku 4.5、Sonnet 4.6 和 Opus 4.6*
*现已包含：钩子、检查点、规划模式、深度思考、后台任务、权限模式（6 种模式）、无头模式、会话管理、自动记忆、代理团队、调度任务、Chrome 集成、频道、语音输入和捆绑技能*
