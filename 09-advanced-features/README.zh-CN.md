<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# 高级功能

Claude Code 高级功能的综合指南，包括规划模式、扩展思考、自动模式、后台任务、权限模式、打印模式（非交互式）、会话管理、交互功能、频道、语音听写、远程控制、Web 会话、桌面应用、任务列表、提示建议、Git 工作树、沙箱、托管设置和配置。

## 目录

1. [概述](#概述)
2. [规划模式](#规划模式)
3. [扩展思考](#扩展思考)
4. [自动模式](#自动模式)
5. [后台任务](#后台任务)
6. [计划任务](#计划任务)
7. [权限模式](#权限模式)
8. [无头模式](#无头模式)
9. [会话管理](#会话管理)
10. [交互功能](#交互功能)
11. [语音听写](#语音听写)
12. [频道](#频道)
13. [Chrome 集成](#chrome-集成)
14. [远程控制](#远程控制)
15. [Web 会话](#web-会话)
16. [桌面应用](#桌面应用)
17. [任务列表](#任务列表)
18. [提示建议](#提示建议)
19. [Git 工作树](#git-工作树)
20. [沙箱](#沙箱)
21. [托管设置（企业版）](#托管设置企业版)
22. [配置和设置](#配置和设置)
23. [最佳实践](#最佳实践)
24. [其他资源](#其他资源)

---

## 概述

Claude Code 的高级功能通过规划、推理、自动化和控制机制扩展了核心能力。这些功能为复杂的开发任务、代码审查、自动化和多会话管理提供了复杂的工作流程。

**主要高级功能包括：**
- **规划模式**：在编码之前创建详细的实现计划
- **扩展思考**：针对复杂问题的深度推理
- **自动模式**：后台安全分类器在执行前审查每个操作（研究预览版）
- **后台任务**：在不阻塞对话的情况下运行长时间操作
- **权限模式**：控制 Claude 可以做什么（`default`、`acceptEdits`、`plan`、`auto`、`dontAsk`、`bypassPermissions`）
- **打印模式**：非交互式运行 Claude Code 以实现自动化和 CI/CD（`claude -p`）
- **会话管理**：管理多个工作会话
- **交互功能**：键盘快捷键、多行输入和命令历史
- **语音听写**：支持 20 种语言的 STT 按键通话语音输入
- **频道**：MCP 服务器将消息推送到正在运行的会话（研究预览版）
- **远程控制**：从 Claude.ai 或 Claude 应用控制 Claude Code
- **Web 会话**：在浏览器中运行 Claude Code，地址为 claude.ai/code
- **桌面应用**：用于视觉差异审查和多会话的独立应用
- **任务列表**：跨上下文压缩的持久任务跟踪
- **提示建议**：基于上下文的智能命令建议
- **Git 工作树**：用于并行工作的隔离工作树分支
- **沙箱**：操作系统级别的文件系统和网络隔离
- **托管设置**：通过 plist、注册表或托管文件进行企业部署
- **配置**：使用 JSON 配置文件自定义行为

---

## 规划模式

规划模式允许 Claude 在实现之前仔细思考复杂任务，创建一个可以审查和批准的详细计划。

### 什么是规划模式？

规划模式是一个两阶段方法：
1. **规划阶段**：Claude 分析任务并创建详细的实现计划
2. **实现阶段**：批准后，Claude 执行计划

### 何时使用规划模式

✅ 使用规划模式用于：
- 复杂的多文件重构
- 新功能实现
- 架构变更
- 数据库迁移
- 主要 API 重新设计

❌ 不要使用规划模式用于：
- 简单的 bug 修复
- 格式化更改
- 单文件编辑
- 快速查询

### 激活规划模式

**斜杠命令**：
```bash
/plan 实现用户认证系统
```

**CLI 标志**：
```bash
claude --permission-mode plan
```

**设置为默认**：
```json
{
  "permissions": {
    "defaultMode": "plan"
  }
}
```

**键盘快捷键**：
- `Shift + Tab` - 切换权限模式（包括 plan）
- `Alt + M` - 切换权限模式（Windows/Linux）

### 规划模式的好处

- **结构化实现方法**：实现步骤的详细分解
- **审查和批准**：在执行之前批准或调整计划
- **风险识别**：在实现之前识别潜在问题
- **清晰的阶段**：有组织的实现阶段和里程碑

### 示例：功能实现

```
用户：/plan 添加实时通知系统

Claude：我将为实现实时通知创建一个计划。

## 实现计划

### 阶段 1：基础设施设置
1. 安装 WebSocket 库（socket.io）
2. 设置 WebSocket 服务器
3. 配置连接管理

### 阶段 2：后端实现
4. 创建通知服务
5. 实现通知队列
6. 添加通知的数据库架构
7. 创建 API 端点

### 阶段 3：前端集成
8. 设置 WebSocket 客户端
9. 创建通知 UI 组件
10. 实现通知状态管理
11. 添加通知权限

### 阶段 4：测试和部署
12. 编写单元测试
13. 编写集成测试
14. 更新文档
15. 部署到暂存环境

预计时间：3-4 小时
要修改的文件：12
要创建的新文件：8

准备继续吗？（yes/no/modify plan）

用户：yes

Claude：[开始按照计划实现]
```

### 计划修改

```
用户：修改计划 - 暂时跳过队列，我们稍后再添加

Claude：更新后的计划：
[显示删除了队列的修改后的计划]

用户：看起来不错，继续

Claude：[实现修改后的计划]
```

### 规划模式配置

规划模式通过 CLI 标志或斜杠命令激活：

```bash
# 通过 CLI 激活规划模式
claude --permission-mode plan

# 或在 REPL 中使用 /plan 斜杠命令
/plan 实现用户认证系统
```

**规划的模型别名**：使用 `opusplan` 作为模型别名，使用 Opus 进行规划，使用 Sonnet 执行：

```bash
claude --model opusplan "设计并实现新的 API"
```

**外部编辑计划**：按 `Ctrl+G` 在外部编辑器中打开当前计划以进行详细修改。

---

## 扩展思考

扩展思考允许 Claude 在提供解决方案之前花更多时间推理复杂问题。

### 什么是扩展思考？

扩展思考是一个深思熟虑的、逐步推理过程，其中 Claude：
- 分解复杂问题
- 考虑多种方法
- 评估权衡
- 推理边缘情况

### 激活扩展思考

**键盘快捷键**：
- `Option + T` (macOS) / `Alt + T` (Windows/Linux) - 切换扩展思考

**自动激活**：
- 默认为所有模型启用（Opus 4.6、Sonnet 4.6、Haiku 4.5）
- Opus 4.6：具有努力级别的自适应推理：`low`（○）、`medium`（◐）、`high`（●）、`max`（仅 Opus 4.6）
- 其他模型：固定预算最多 31,999 个 token

**配置方法**：
- 切换：`Alt+T` / `Option+T`，或通过 `/config`
- 查看推理：`Ctrl+O`（详细模式）
- 设置努力：`/effort` 命令或 `--effort` 标志

**自定义预算**：
```bash
export MAX_THINKING_TOKENS=1024
```

**努力级别**（仅 Opus 4.6）：
```bash
export CLAUDE_CODE_EFFORT_LEVEL=high   # low (○)、medium (◐)、high (●) 或 max（仅 Opus 4.6）
```

**CLI 标志**：
```bash
claude --effort high "复杂的架构审查"
```

**斜杠命令**：
```
/effort high
```

> **注意**：提示中的关键词 "ultrathink" 会激活深度推理模式。努力级别 `low`、`medium`、`high` 和 `max`（仅 Opus 4.6）控制 Claude 执行多少推理。

### 扩展思考的好处

- **深度推理**：花更多时间思考复杂问题
- **更好的架构决策**：通过全面评估做出明智选择
- **考虑边缘情况**：思考潜在问题和场景
- **彻底分析**：结构化方法解决复杂问题

### 示例：架构决策

```
用户：/think 我们应该为我们的电子商务平台使用微服务还是单体？

Claude：让我仔细思考这个架构决策...

<extended_thinking>
## 上下文分析
- 团队规模：5 名开发人员
- 预期流量：最初 10k 日用户
- 需要扩展到 100k+ 用户
- DevOps 资源有限
- 到 MVP 的时间表：6 个月

## 方法 1：微服务
优点：
+ 独立扩展
+ 技术灵活性
+ 团队独立性
+ 故障隔离

缺点：
- 运营复杂性
- 网络开销
- 需要分布式跟踪
- 需要 DevOps 专业知识
- 初始开发较慢

## 方法 2：单体
优点：
+ 更简单的部署
+ 更快的初始开发
+ 更容易调试
+ 更低的运营开销
+ 单一代码库

缺点：
- 后期扩展挑战
- 全有或全无的部署
- 技术锁定
- 可能紧密耦合

## 决策因素
1. 团队规模（5 名开发人员）→ 倾向于单体
2. DevOps 资源（有限）→ 倾向于单体
3. 时间表（6 个月）→ 倾向于单体
4. 未来规模（100k 用户）→ 单体 + 缓存可实现
5. 复杂性预算 → 有限

## 建议
从**模块化单体**开始：
- 单个部署单元
- 清晰的模块边界
- 模块之间的清晰接口
- 为将来提取到微服务做好准备

这为您提供：
✓ 快速初始开发
✓ 低运营复杂性
✓ 稍后提取服务的选项
✓ 清晰的架构

迁移路径：当您达到 50k+ 用户或特定模块需要独立扩展时，将这些模块提取到微服务。
</extended_thinking>

**建议**：实现具有清晰模块边界的模块化单体。

这种方法平衡了您当前的约束（团队规模、时间表、DevOps 资源）与未来的可扩展性需求。当有明确的业务需求时，您可以稍后将特定模块迁移到微服务。
```

### 扩展思考配置

扩展思考通过环境变量、键盘快捷键和 CLI 标志控制：

```bash
# 设置思考 token 预算
export MAX_THINKING_TOKENS=16000

# 设置努力级别（仅 Opus 4.6）：low (○)、medium (◐)、high (●) 或 max（仅 Opus 4.6）
export CLAUDE_CODE_EFFORT_LEVEL=high
```

在会话期间使用 `Alt+T` / `Option+T` 切换，使用 `/effort` 设置努力，或通过 `/config` 配置。

---

## 自动模式

自动模式是一个研究预览版权限模式（2026 年 3 月），使用后台安全分类器在执行前审查每个操作。它允许 Claude 自主工作，同时阻止危险操作。

### 要求

- **计划**：团队计划（企业版和 API 正在推出）
- **模型**：Claude Sonnet 4.6 或 Opus 4.6
- **分类器**：在 Claude Sonnet 4.6 上运行（增加额外的 token 成本）

### 启用自动模式

```bash
# 使用 CLI 标志解锁自动模式
claude --enable-auto-mode

# 然后在 REPL 中使用 Shift+Tab 切换到它
```

或将其设置为默认权限模式：

```bash
claude --permission-mode auto
```

通过配置设置：
```json
{
  "permissions": {
    "defaultMode": "auto"
  }
}
```

### 分类器如何工作

后台分类器使用以下决策顺序评估每个操作：

1. **允许/拒绝规则** -- 首先检查明确的权限规则
2. **只读/编辑自动批准** -- 文件读取和编辑自动通过
3. **分类器** -- 后台分类器审查操作
4. **回退** -- 在 3 次连续或 20 次总阻止后回退到提示

### 默认阻止的操作

自动模式默认阻止以下操作：

| 阻止的操作 | 示例 |
|----------------|---------|
| 管道到 shell 安装 | `curl \| bash` |
| 向外部发送敏感数据 | 通过网络发送 API 密钥、凭据 |
| 生产部署 | 针对生产的部署命令 |
| 大规模删除 | 对大目录的 `rm -rf` |
| IAM 更改 | 权限和角色修改 |
| 强制推送到 main | `git push --force origin main` |

### 默认允许的操作

| 允许的操作 | 示例 |
|----------------|---------|
| 本地文件操作 | 读取、写入、编辑项目文件 |
| 声明的依赖安装 | 从清单进行 `npm install`、`pip install` |
| 只读 HTTP | `curl` 用于获取文档 |
| 推送到当前分支 | `git push origin feature-branch` |

### 配置自动模式

**将默认规则打印为 JSON**：
```bash
claude auto-mode defaults
```

**通过 `autoMode.environment` 托管设置配置受信任的基础设施**，用于企业部署。这允许管理员定义受信任的 CI/CD 环境、部署目标和基础设施模式。

### 回退行为

当分类器不确定时，自动模式回退到提示用户：
- 在 **3 次连续**分类器阻止后
- 在会话中 **20 次总**分类器阻止后

这确保当分类器无法自信地批准操作时，用户始终保留控制权。

### 播种自动模式等效权限（无需团队计划）

如果您没有团队计划或想要没有后台分类器的更简单方法，您可以使用安全的权限规则保守基线来播种您的 `~/.claude/settings.json`。该脚本从只读和本地检查规则开始，然后仅在您需要时才允许您选择加入编辑、测试、本地 git 写入、包安装和 GitHub 写入操作。

**文件：** `09-advanced-features/setup-auto-mode-permissions.py`

```bash
# 预览将要添加的内容（不写入更改）
python3 09-advanced-features/setup-auto-mode-permissions.py --dry-run

# 应用保守基线
python3 09-advanced-features/setup-auto-mode-permissions.py

# 仅在需要时添加更多功能
python3 09-advanced-features/setup-auto-mode-permissions.py --include-edits --include-tests
python3 09-advanced-features/setup-auto-mode-permissions.py --include-git-write --include-packages
```

该脚本添加以下类别的规则：

| 类别 | 示例 |
|----------|---------|
| 核心只读工具 | `Read(*)`、`Glob(*)`、`Grep(*)`、`Agent(*)`、`WebSearch(*)`、`WebFetch(*)` |
| 本地检查 | `Bash(git status:*)`、`Bash(git log:*)`、`Bash(git diff:*)`、`Bash(cat:*)` |
| 可选编辑 | `Edit(*)`、`Write(*)`、`NotebookEdit(*)` |
| 可选测试/构建 | `Bash(pytest:*)`、`Bash(python3 -m pytest:*)`、`Bash(cargo test:*)` |
| 可选 git 写入 | `Bash(git add:*)`、`Bash(git commit:*)`、`Bash(git stash:*)` |
| Git（本地写入） | `Bash(git add:*)`、`Bash(git commit:*)`、`Bash(git checkout:*)` |
| 包管理器 | `Bash(npm install:*)`、`Bash(pip install:*)`、`Bash(cargo build:*)` |
| 构建和测试 | `Bash(make:*)`、`Bash(pytest:*)`、`Bash(go test:*)` |
| 常见 shell | `Bash(ls:*)`、`Bash(cat:*)`、`Bash(find:*)`、`Bash(cp:*)`、`Bash(mv:*)` |
| GitHub CLI | `Bash(gh pr view:*)`、`Bash(gh pr create:*)`、`Bash(gh issue list:*)` |

危险操作（`rm -rf`、`sudo`、强制推送、`DROP TABLE`、`terraform destroy` 等）被有意排除。该脚本是幂等的 -- 运行两次不会重复规则。

---

## 后台任务

后台任务允许长时间运行的操作在不阻塞对话的情况下执行。

### 什么是后台任务？

后台任务在您继续工作时异步运行：
- 长时间测试套件
- 构建过程
- 数据库迁移
- 部署脚本
- 分析工具

**基本用法：**
```bash
用户：在后台运行测试

Claude：已启动任务 bg-1234

/task list           # 显示所有任务
/task status bg-1234 # 检查进度
/task show bg-1234   # 查看输出
/task cancel bg-1234 # 取消任务
```

### 启动后台任务

```
用户：在后台运行完整的测试套件

Claude：在后台启动测试（任务 ID：bg-1234）
您可以在测试运行时继续工作。

[您可以继续对话]

用户：同时，让我们重构 auth 模块

Claude：[在测试在后台运行时处理 auth 模块]

[稍后，当测试完成时]

Claude：📢 后台任务 bg-1234 已完成：
✅ 245 个测试通过
❌ 3 个测试失败
查看结果：/task show bg-1234
```

### 管理后台任务

**列出活动任务**：
```
用户：/task list

活动后台任务：
1. [bg-1234] 运行测试（50% 完成，剩余 2 分钟）
2. [bg-1235] 构建 Docker 镜像（25% 完成，剩余 8 分钟）
3. [bg-1236] 部署到暂存环境（90% 完成，剩余 30 秒）
```

**检查任务状态**：
```
用户：/task status bg-1234

任务 bg-1234：运行测试
状态：进行中
进度：120/245 测试（49%）
开始时间：2025-11-08 10:30:15
预计完成时间：2025-11-08 10:34:22
```

**查看任务输出**：
```
用户：/task show bg-1234

[显示测试运行的实时输出]
```

**取消任务**：
```
用户：/task cancel bg-1234

已取消后台任务 bg-1234
```

### 示例：并行开发

```
用户：在后台运行构建

Claude：启动构建...（任务 ID：bg-5001）

用户：同时在后台运行 linter

Claude：启动 linter...（任务 ID：bg-5002）

用户：当这些运行时，让我们实现新的 API 端点

Claude：[在构建和 linter 运行时实现 API 端点]

[10 分钟后]

Claude：📢 构建成功完成（bg-5001）
📢 Linter 发现 12 个问题（bg-5002）

用户：向我显示 linter 问题

Claude：[显示来自 bg-5002 的 linter 输出]
```

### 配置

```json
{
  "backgroundTasks": {
    "enabled": true,
    "maxConcurrentTasks": 5,
    "notifyOnCompletion": true,
    "autoCleanup": true,
    "logOutput": true
  }
}
```

---

## 计划任务

计划任务允许您按重复计划或作为一次性提醒自动运行提示。任务是会话范围的 -- 它们在 Claude Code 活动时运行，并在会话结束时清除。自 v2.1.72+ 起可用。

### `/loop` 命令

```bash
# 显式间隔
/loop 5m 检查部署是否完成

# 自然语言
/loop 每 30 分钟检查构建状态
```

也支持标准的 5 字段 cron 表达式以进行精确调度。

### 一次性提醒

设置在特定时间触发一次的提醒：

```
在下午 3 点提醒我推送发布分支
在 45 分钟内，运行集成测试
```

### 管理计划任务

| 工具 | 描述 |
|------|-------------|
| `CronCreate` | 创建新的计划任务 |
| `CronList` | 列出所有活动的计划任务 |
| `CronDelete` | 删除计划任务 |

**限制和行为**：
- 每个会话最多 **50 个计划任务**
- 会话范围 -- 会话结束时清除
- 重复任务在 **3 天**后自动过期
- 任务仅在 Claude Code 运行时触发 -- 错过的触发不会追赶

### 行为详细信息

| 方面 | 详细信息 |
|--------|--------|
| **重复抖动** | 最多间隔的 10%（最多 15 分钟） |
| **一次性抖动** | 在 :00/:30 边界上最多 90 秒 |
| **错过的触发** | 无追赶 -- 如果 Claude Code 未运行则跳过 |
| **持久性** | 不会在重启之间持久化 |

### 云计划任务

使用 `/schedule` 创建在 Anthropic 基础设施上运行的云计划任务：

```
/schedule 每天上午 9 点运行测试套件并报告失败
```

云计划任务在重启之间持久化，不需要 Claude Code 在本地运行。

### 禁用计划任务

```bash
export CLAUDE_CODE_DISABLE_CRON=1
```

### 示例：监控部署

```
/loop 5m 检查暂存环境的部署状态。
        如果部署成功，通知我并停止循环。
        如果失败，显示错误日志。
```

> **提示**：计划任务是会话范围的。对于在重启后存活的持久自动化，请改用 CI/CD 管道、GitHub Actions 或桌面应用计划任务。

---

## 权限模式

权限模式控制 Claude 可以在无需明确批准的情况下采取的操作。

### 可用的权限模式

| 模式 | 行为 |
|---|---|
| `default` | 仅读取文件；提示所有其他操作 |
| `acceptEdits` | 读取和编辑文件；提示命令 |
| `plan` | 仅读取文件（研究模式，无编辑） |
| `auto` | 所有操作都经过后台安全分类器检查（研究预览版） |
| `bypassPermissions` | 所有操作，无权限检查（危险） |
| `dontAsk` | 仅执行预批准的工具；所有其他操作被拒绝 |

在 CLI 中使用 `Shift+Tab` 循环切换模式。使用 `--permission-mode` 标志或 `permissions.defaultMode` 设置设置默认值。

### 激活方法

**键盘快捷键**：
```bash
Shift + Tab  # 循环切换所有 6 种模式
```

**斜杠命令**：
```bash
/plan                  # 进入规划模式
```

**CLI 标志**：
```bash
claude --permission-mode plan
claude --permission-mode auto
```

**设置**：
```json
{
  "permissions": {
    "defaultMode": "auto"
  }
}
```

### 权限模式示例

#### 默认模式
Claude 要求对重要操作进行确认：

```
用户：修复 auth.ts 中的 bug

Claude：我需要修改 src/auth.ts 来修复 bug。
更改将更新密码验证逻辑。

批准此更改？（yes/no/show）
```

#### 规划模式
在执行之前审查实现计划：

```
用户：/plan 实现用户认证系统

Claude：我将为实现认证创建一个计划。

## 实现计划
[包含阶段和步骤的详细计划]

准备继续吗？（yes/no/modify）
```

#### 接受编辑模式
自动接受文件修改：

```
用户：acceptEdits
用户：修复 auth.ts 中的 bug

Claude：[无需询问即可进行更改]
```

### 用例

**代码审查**：
```
用户：claude --permission-mode plan
用户：审查此 PR 并提出改进建议

Claude：[读取代码，提供反馈，但无法修改]
```

**结对编程**：
```
用户：claude --permission-mode default
用户：让我们一起实现该功能

Claude：[在每次更改之前请求批准]
```

**自动化任务**：
```
用户：claude --permission-mode acceptEdits
用户：修复代码库中的所有 linting 问题

Claude：[自动接受文件编辑而无需询问]
```

---

## 无头模式

打印模式（`claude -p`）允许 Claude Code 在没有交互输入的情况下运行，非常适合自动化和 CI/CD。这是非交互模式，取代了较旧的 `--headless` 标志。

### 什么是打印模式？

打印模式启用：
- 自动化脚本执行
- CI/CD 集成
- 批处理
- 计划任务

### 在打印模式下运行（非交互式）

```bash
# 运行特定任务
claude -p "运行所有测试"

# 处理管道内容
cat error.log | claude -p "分析这些错误"

# CI/CD 集成（GitHub Actions）
- name: AI 代码审查
  run: claude -p "审查 PR"
```

### 其他打印模式使用示例

```bash
# 运行特定任务并捕获输出
claude -p "运行所有测试并生成覆盖率报告"

# 使用结构化输出
claude -p --output-format json "分析代码质量"

# 使用来自 stdin 的输入
echo "分析代码质量" | claude -p "解释这个"
```

### 示例：CI/CD 集成

**GitHub Actions**：
```yaml
# .github/workflows/code-review.yml
name: AI 代码审查

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: 安装 Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: 运行 Claude 代码审查
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p --output-format json \
            --max-turns 3 \
            "审查此 PR 的：
            - 代码质量问题
            - 安全漏洞
            - 性能问题
            - 测试覆盖率
            将结果输出为 JSON" > review.json

      - name: 发布审查评论
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            const review = JSON.parse(fs.readFileSync('review.json', 'utf8'));
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: JSON.stringify(review, null, 2)
            });
```

### 打印模式配置

打印模式（`claude -p`）支持几个用于自动化的标志：

```bash
# 限制自主轮次
claude -p --max-turns 5 "重构此模块"

# 结构化 JSON 输出
claude -p --output-format json "分析此代码库"

# 使用架构验证
claude -p --json-schema '{"type":"object","properties":{"issues":{"type":"array"}}}' \
  "在此代码中查找 bug"

# 禁用会话持久性
claude -p --no-session-persistence "一次性分析"
```

---

## 会话管理

有效地管理多个 Claude Code 会话。

### 会话管理命令

| 命令 | 描述 |
|---------|-------------|
| `/resume` | 按 ID 或名称恢复对话 |
| `/rename` | 命名当前会话 |
| `/fork` | 将当前会话分叉到新分支 |
| `claude -c` | 继续最近的对话 |
| `claude -r "session"` | 按名称或 ID 恢复会话 |

### 恢复会话

**继续最后的对话**：
```bash
claude -c
```

**恢复命名会话**：
```bash
claude -r "auth-refactor" "完成此 PR"
```

**重命名当前会话**（在 REPL 内）：
```
/rename auth-refactor
```

### 分叉会话

分叉会话以尝试替代方法而不丢失原始会话：

```
/fork
```

或从 CLI：
```bash
claude --resume auth-refactor --fork-session "尝试 OAuth 代替"
```

### 会话持久性

会话自动保存并可以恢复：

```bash
# 继续最后的对话
claude -c

# 按名称或 ID 恢复特定会话
claude -r "auth-refactor"

# 恢复并分叉以进行实验
claude --resume auth-refactor --fork-session "替代方法"
```

---

## 交互功能

### 键盘快捷键

Claude Code 支持键盘快捷键以提高效率。以下是官方文档中的完整参考：

| 快捷键 | 描述 |
|----------|-------------|
| `Ctrl+C` | 取消当前输入/生成 |
| `Ctrl+D` | 退出 Claude Code |
| `Ctrl+G` | 在外部编辑器中编辑计划 |
| `Ctrl+L` | 清除终端屏幕 |
| `Ctrl+O` | 切换详细输出（查看推理） |
| `Ctrl+R` | 反向搜索历史 |
| `Ctrl+T` | 切换任务列表视图 |
| `Ctrl+B` | 后台运行任务 |
| `Esc+Esc` | 倒带代码/对话 |
| `Shift+Tab` / `Alt+M` | 切换权限模式 |
| `Option+P` / `Alt+P` | 切换模型 |
| `Option+T` / `Alt+T` | 切换扩展思考 |

**行编辑（标准 readline 快捷键）：**

| 快捷键 | 操作 |
|----------|--------|
| `Ctrl + A` | 移动到行首 |
| `Ctrl + E` | 移动到行尾 |
| `Ctrl + K` | 剪切到行尾 |
| `Ctrl + U` | 剪切到行首 |
| `Ctrl + W` | 向后删除单词 |
| `Ctrl + Y` | 粘贴（yank） |
| `Tab` | 自动完成 |
| `↑ / ↓` | 命令历史 |

### 自定义键绑定

通过运行 `/keybindings` 创建自定义键盘快捷键，这将打开 `~/.claude/keybindings.json` 进行编辑（v2.1.18+）。

**配置格式**：

```json
{
  "$schema": "https://www.schemastore.org/claude-code-keybindings.json",
  "bindings": [
    {
      "context": "Chat",
      "bindings": {
        "ctrl+e": "chat:externalEditor",
        "ctrl+u": null,
        "ctrl+k ctrl+s": "chat:stash"
      }
    },
    {
      "context": "Confirmation",
      "bindings": {
        "ctrl+a": "confirmation:yes"
      }
    }
  ]
}
```

将绑定设置为 `null` 以取消绑定默认快捷键。

### 可用的上下文

键绑定范围限定于特定的 UI 上下文：

| 上下文 | 关键操作 |
|---------|-------------|
| **Chat** | `submit`、`cancel`、`cycleMode`、`modelPicker`、`thinkingToggle`、`undo`、`externalEditor`、`stash`、`imagePaste` |
| **Confirmation** | `yes`、`no`、`previous`、`next`、`nextField`、`cycleMode`、`toggleExplanation` |
| **Global** | `interrupt`、`exit`、`toggleTodos`、`toggleTranscript` |
| **Autocomplete** | `accept`、`dismiss`、`next`、`previous` |
| **HistorySearch** | `search`、`previous`、`next` |
| **Settings** | 上下文特定的设置导航 |
| **Tabs** | 选项卡切换和管理 |
| **Help** | 帮助面板导航 |

总共有 18 个上下文，包括 `Transcript`、`Task`、`ThemePicker`、`Attachments`、`Footer`、`MessageSelector`、`DiffDialog`、`ModelPicker` 和 `Select`。

### 和弦支持

键绑定支持和弦序列（多键组合）：

```
"ctrl+k ctrl+s"   → 两键序列：按 ctrl+k，然后按 ctrl+s
"ctrl+shift+p"    → 同时按修饰键
```

**击键语法**：
- **修饰键**：`ctrl`、`alt`（或 `opt`）、`shift`、`meta`（或 `cmd`）
- **大写意味着 Shift**：`K` 等同于 `shift+k`
- **特殊键**：`escape`、`enter`、`return`、`tab`、`space`、`backspace`、`delete`、箭头键

### 保留和冲突的键

| 键 | 状态 | 说明 |
|-----|--------|-------|
| `Ctrl+C` | 保留 | 无法重新绑定（中断） |
| `Ctrl+D` | 保留 | 无法重新绑定（退出） |
| `Ctrl+B` | 终端冲突 | tmux 前缀键 |
| `Ctrl+A` | 终端冲突 | GNU Screen 前缀键 |
| `Ctrl+Z` | 终端冲突 | 进程挂起 |

> **提示**：如果快捷键不起作用，请检查与终端模拟器或多路复用器的冲突。

### Tab 完成

Claude Code 提供智能 tab 完成：

```
用户：/rew<TAB>
→ /rewind

用户：/plu<TAB>
→ /plugin

用户：/plugin <TAB>
→ /plugin install
→ /plugin enable
→ /plugin disable
```

### 命令历史

访问以前的命令：

```
用户：<↑>  # 上一个命令
用户：<↓>  # 下一个命令
用户：Ctrl+R  # 搜索历史

(reverse-i-search)`test': 运行所有测试
```

### 多行输入

对于复杂查询，使用多行模式：

```bash
用户：\
> 长而复杂的提示
> 跨越多行
> \end
```

**示例：**

```
用户：\
> 实现用户认证系统
> 具有以下要求：
> - JWT 令牌
> - 电子邮件验证
> - 密码重置
> - 2FA 支持
> \end

Claude：[处理多行请求]
```

### 内联编辑

在发送之前编辑命令：

```
用户：Deploy to prodcution<Backspace><Backspace>uction

[在发送之前就地编辑]
```

### Vim 模式

启用 Vi/Vim 键盘绑定进行文本编辑：

**激活**：
- 使用 `/vim` 命令或 `/config` 启用
- 使用 `Esc` 切换到 NORMAL 模式，使用 `i/a/o` 切换到 INSERT 模式

**导航键**：
- `h` / `l` - 左/右移动
- `j` / `k` - 下/上移动
- `w` / `b` / `e` - 按单词移动
- `0` / `$` - 移动到行首/行尾
- `gg` / `G` - 跳转到文本开头/结尾

**文本对象**：
- `iw` / `aw` - 单词内/单词周围
- `i"` / `a"` - 引号字符串内/周围
- `i(` / `a(` - 括号内/周围

### Bash 模式

使用 `!` 前缀直接执行 shell 命令：

```bash
! npm test
! git status
! cat src/index.js
```

用于快速执行命令，无需切换上下文。

---

## 语音听写

语音听写为 Claude Code 提供按键通话语音输入，允许您口述提示而不是键入。

### 激活语音听写

```
/voice
```

### 功能

| 功能 | 描述 |
|---------|-------------|
| **按键通话** | 按住键录音，松开即发送 |
| **20 种语言** | 语音转文本支持 20 种语言 |
| **自定义键绑定** | 通过 `/keybindings` 配置按键通话键 |
| **账户要求** | 需要 Claude.ai 账户进行 STT 处理 |

### 配置

在键绑定文件（`/keybindings`）中自定义按键通话键绑定。语音听写使用 Claude.ai 账户进行语音转文本处理。

---

## 频道

频道（研究预览版）允许 MCP 服务器将消息推送到运行中的 Claude Code 会话，实现与外部服务的实时集成。

### 订阅频道

```bash
# 在启动时订阅频道插件
claude --channels discord,telegram
```

### 支持的集成

| 集成 | 描述 |
|-------------|-------------|
| **Discord** | 在会话中接收和回复 Discord 消息 |
| **Telegram** | 在会话中接收和回复 Telegram 消息 |

### 配置

企业部署的**托管设置**：

```json
{
  "allowedChannelPlugins": ["discord", "telegram"]
}
```

`allowedChannelPlugins` 托管设置控制整个组织中允许使用哪些频道插件。

### 工作原理

1. MCP 服务器充当连接到外部服务的频道插件
2. 传入消息被推送到活动的 Claude Code 会话
3. Claude 可以在会话上下文中读取和回复消息
4. 频道插件必须通过 `allowedChannelPlugins` 托管设置批准

---

## Chrome 集成

Chrome 集成将 Claude Code 连接到 Chrome 或 Microsoft Edge 浏览器，实现实时 Web 自动化和调试。这是自 v2.0.73+ 起可用的测试版功能（Edge 支持在 v1.0.36+ 添加）。

### 启用 Chrome 集成

**启动时**：

```bash
claude --chrome      # 启用 Chrome 连接
claude --no-chrome   # 禁用 Chrome 连接
```

**在会话内**：

```
/chrome
```

选择"默认启用"可为所有未来会话激活 Chrome 集成。Claude Code 共享浏览器的登录状态，因此可以与经过身份验证的 Web 应用交互。

### 功能

| 功能 | 描述 |
|------------|-------------|
| **实时调试** | 读取控制台日志、检查 DOM 元素、实时调试 JavaScript |
| **设计验证** | 将渲染页面与设计模型进行比较 |
| **表单验证** | 测试表单提交、输入验证和错误处理 |
| **Web 应用测试** | 与经过身份验证的应用交互（Gmail、Google Docs、Notion 等） |
| **数据提取** | 从网页抓取和处理内容 |
| **会话录制** | 将浏览器交互录制为 GIF 文件 |

### 站点级权限

Chrome 扩展管理每站点访问权限。随时通过扩展弹窗授予或撤销特定站点的访问权限。Claude Code 只与您明确允许的站点交互。

### 工作原理

Claude Code 在可见窗口中控制浏览器 -- 您可以实时观察操作的发生。当浏览器遇到登录页面或 CAPTCHA 时，Claude 会暂停并等待您手动处理后再继续。

### 已知限制

- **浏览器支持**：仅支持 Chrome 和 Edge -- 不支持 Brave、Arc 和其他 Chromium 浏览器
- **WSL**：在 Windows Subsystem for Linux 中不可用
- **第三方提供商**：不支持 Bedrock、Vertex 或 Foundry API 提供商
- **Service Worker 空闲**：Chrome 扩展 service worker 在长时间会话期间可能会空闲

> **提示**：Chrome 集成是测试版功能。浏览器支持可能会在未来版本中扩展。

---

## 远程控制

远程控制允许您从手机、平板或任何浏览器继续本地运行的 Claude Code 会话。您的本地会话继续在您的机器上运行 -- 没有任何东西移动到云端。适用于 Pro、Max、Team 和 Enterprise 计划（v2.1.51+）。

### 启动远程控制

**从 CLI**：

```bash
# 使用默认会话名称启动
claude remote-control

# 使用自定义名称启动
claude remote-control --name "Auth Refactor"
```

**从会话内**：

```
/remote-control
/remote-control "Auth Refactor"
```

**可用标志**：

| 标志 | 描述 |
|------|-------------|
| `--name "title"` | 自定义会话标题，便于识别 |
| `--verbose` | 显示详细的连接日志 |
| `--sandbox` | 启用文件系统和网络隔离 |
| `--no-sandbox` | 禁用沙箱（默认） |

### 连接到会话

从其他设备连接的三种方式：

1. **会话 URL** -- 会话启动时打印到终端；在任何浏览器中打开
2. **二维码** -- 启动后按 `空格键` 显示可扫描的二维码
3. **按名称查找** -- 在 claude.ai/code 或 Claude 移动应用（iOS/Android）中浏览您的会话

### 安全性

- 您的机器上**没有打开入站端口**
- **仅出站 HTTPS**，通过 TLS
- **限定范围的凭证** -- 多个短期、范围狭窄的令牌
- **会话隔离** -- 每个远程会话是独立的

### 远程控制 vs Web 上的 Claude Code

| 方面 | 远程控制 | Web 上的 Claude Code |
|--------|---------------|-------------------|
| **执行** | 在您的机器上运行 | 在 Anthropic 云端运行 |
| **本地工具** | 完全访问本地 MCP 服务器、文件和 CLI | 无本地依赖 |
| **用例** | 从其他设备继续本地工作 | 从任何浏览器全新开始 |

### 限制

- 每个 Claude Code 实例一个远程会话
- 主机上的终端必须保持打开
- 如果网络不可达，会话约 10 分钟后超时

### 用例

- 离开办公桌时从移动设备或平板控制 Claude Code
- 在保持本地工具执行的同时使用更丰富的 claude.ai UI
- 使用完整的本地开发环境进行快速代码审查

---

## Web 会话

Web 会话允许您直接在浏览器的 claude.ai/code 中运行 Claude Code，或从 CLI 创建 Web 会话。

### 创建 Web 会话

```bash
# 从 CLI 创建新的 Web 会话
claude --remote "实现新的 API 端点"
```

这将在 claude.ai 上启动一个 Claude Code 会话，您可以从任何浏览器访问。

### 在本地恢复 Web 会话

如果您在网上启动了会话并希望继续在本地进行：

```bash
# 在本地终端恢复 Web 会话
claude --teleport
```

或从交互式 REPL：
```
/teleport
```

### 用例

- 在一台机器上开始工作，在另一台机器上继续
- 与会话 URL 团队成员共享
- 使用 Web UI 进行视觉差异审查，然后切换到终端执行

---

## 桌面应用

Claude Code 桌面应用提供独立应用程序，具有视觉差异审查、并行会话和集成的连接器。适用于 macOS 和 Windows（Pro、Max、Team 和 Enterprise 计划）。

### 安装

从 [claude.ai](https://claude.ai) 下载适用于您平台的应用：
- **macOS**：通用版本（Apple Silicon 和 Intel）
- **Windows**：提供 x64 和 ARM64 安装程序

请参阅[桌面快速入门](https://code.claude.com/docs/en/desktop-quickstart)获取设置说明。

### 从 CLI 交递

将当前 CLI 会话传输到桌面应用：

```
/desktop
```

### 核心功能

| 功能 | 描述 |
|---------|-------------|
| **差异视图** | 逐个文件视觉审查，带有内联评论；Claude 读取评论并修订 |
| **应用预览** | 自动启动开发服务器，带有嵌入式浏览器进行实时验证 |
| **PR 监控** | GitHub CLI 集成，自动修复 CI 失败，检查通过后自动合并 |
| **并行会话** | 侧边栏中的多个会话，带有自动 Git 工作树隔离 |
| **计划任务** | 重复任务（每小时、每天、工作日、每周），在应用打开时运行 |
| **丰富渲染** | 代码、markdown 和图表渲染，带有语法高亮 |

### 应用预览配置

在 `.claude/launch.json` 中配置开发服务器行为：

```json
{
  "command": "npm run dev",
  "port": 3000,
  "readyPattern": "ready on",
  "persistCookies": true
}
```

### 连接器

连接外部服务以获得更丰富的上下文：

| 连接器 | 功能 |
|-----------|------------|
| **GitHub** | PR 监控、问题跟踪、代码审查 |
| **Slack** | 通知、频道上下文 |
| **Linear** | 问题跟踪、sprint 管理 |
| **Notion** | 文档、知识库访问 |
| **Asana** | 任务管理、项目跟踪 |
| **日历** | 日程感知、会议上下文 |

> **注意**：远程（云端）会话不可用连接器。

### 远程和 SSH 会话

- **远程会话**：在 Anthropic 云基础设施上运行；在应用关闭时继续运行。可从 claude.ai/code 或 Claude 移动应用访问
- **SSH 会话**：通过 SSH 连接到远程机器，完全访问远程文件系统和工具。远程机器上必须安装 Claude Code

### 桌面中的权限模式

桌面应用支持与 CLI 相同的 4 种权限模式：

| 模式 | 行为 |
|------|----------|
| **请求权限**（默认） | 审查和批准每个编辑和命令 |
| **自动接受编辑** | 文件编辑自动批准；命令需要手动批准 |
| **规划模式** | 在进行任何更改之前审查方法 |
| **绕过权限** | 自动执行（仅限沙箱，管理员控制） |

### 企业功能

- **管理控制台**：控制组织的代码选项卡访问和权限设置
- **MDM 部署**：在 macOS 上通过 MDM 或 Windows 上通过 MSIX 部署
- **SSO 集成**：要求组织成员进行单点登录
- **托管设置**：集中管理团队配置和模型可用性

---

## 任务列表

任务列表功能提供持久的任务跟踪，可在上下文压缩（当对话历史记录被修剪以适应上下文窗口时）中存活。

### 切换任务列表

在会话期间按 `Ctrl+T` 切换任务列表视图的开启或关闭。

### 持久任务

任务在上下文压缩中持久化，确保在对话上下文被修剪时不会丢失长期运行的工作项。这对于复杂的多步骤实现特别有用。

### 命名任务目录

使用 `CLAUDE_CODE_TASK_LIST_ID` 环境变量创建跨会话共享的命名任务目录：

```bash
export CLAUDE_CODE_TASK_LIST_ID=my-project-sprint-3
```

这允许多个会话共享同一个任务列表，使其对于团队工作流程或多会话项目很有用。

---

## 提示建议

提示建议根据您的 git 历史记录和当前对话上下文显示灰色的示例命令。

### 工作原理

- 建议在输入提示下方显示为灰色文本
- 按 `Tab` 接受建议
- 按 `Enter` 接受并立即提交
- 建议具有上下文感知能力，从 git 历史记录和对话状态中提取

### 禁用提示建议

```bash
export CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false
```

---

## Git 工作树

Git 工作树允许您在隔离的工作树中启动 Claude Code，无需存储或切换即可在不同分支上并行工作。

### 在工作树中启动

```bash
# 在隔离的工作树中启动 Claude Code
claude --worktree
# 或
claude -w
```

### 工作树位置

工作树创建在：
```
<repo>/.claude/worktrees/<name>
```

### 单仓库的稀疏检出

使用 `worktree.sparsePaths` 设置在单仓库中执行稀疏检出，减少磁盘使用量和克隆时间：

```json
{
  "worktree": {
    "sparsePaths": ["packages/my-package", "shared/"]
  }
}
```

### 工作树工具和钩子

| 项目 | 描述 |
|------|-------------|
| `ExitWorktree` | 退出并清理当前工作树的工具 |
| `WorktreeCreate` | 创建工作时触发的钩子事件 |
| `WorktreeRemove` | 移除工作时触发的钩子事件 |

### 自动清理

如果在工作树中未进行任何更改，则会话结束时会自动清理。

### 用例

- 在处理功能分支时保持主分支不受影响
- 在隔离环境中运行测试，不影响工作目录
- 在一次性环境中尝试实验性更改
- 在单仓库中稀疏检出特定包以加快启动速度

---

## 沙箱

沙箱为 Claude Code 执行的 Bash 命令提供操作系统级别的文件系统和网络隔离。这是对权限规则的补充，提供了额外的安全层。

### 启用沙箱

**斜杠命令**：
```
/sandbox
```

**CLI 标志**：
```bash
claude --sandbox       # 启用沙箱
claude --no-sandbox    # 禁用沙箱
```

### 配置设置

| 设置 | 描述 |
|---------|-------------|
| `sandbox.enabled` | 启用或禁用沙箱 |
| `sandbox.failIfUnavailable` | 如果无法激活沙箱则失败 |
| `sandbox.filesystem.allowWrite` | 允许写入访问的路径 |
| `sandbox.filesystem.allowRead` | 允许读取访问的路径 |
| `sandbox.filesystem.denyRead` | 拒绝读取访问的路径 |
| `sandbox.enableWeakerNetworkIsolation` | 在 macOS 上启用较弱的网络隔离 |

### 配置示例

```json
{
  "sandbox": {
    "enabled": true,
    "failIfUnavailable": true,
    "filesystem": {
      "allowWrite": ["/Users/me/project"],
      "allowRead": ["/Users/me/project", "/usr/local/lib"],
      "denyRead": ["/Users/me/.ssh", "/Users/me/.aws"]
    },
    "enableWeakerNetworkIsolation": true
  }
}
```

### 工作原理

- Bash 命令在受限文件系统访问的沙箱环境中运行
- 可以隔离网络访问以防止意外的外部连接
- 与权限规则一起工作以实现深度防御
- 在 macOS 上，使用 `sandbox.enableWeakerNetworkIsolation` 进行网络限制（macOS 上不可用完整的网络隔离）

### 用例

- 安全地运行不受信任或生成的代码
- 防止意外修改项目之外的文件
- 在自动化任务期间限制网络访问

---

## 托管设置（企业版）

托管设置使企业管理员能够使用平台原生管理工具在整个组织内部署 Claude Code 配置。

### 部署方法

| 平台 | 方法 | 版本 |
|----------|--------|-------|
| macOS | 托管 plist 文件（MDM） | v2.1.51+ |
| Windows | Windows 注册表 | v2.1.51+ |
| 跨平台 | 托管配置文件 | v2.1.51+ |
| 跨平台 | 托管 drop-ins（`managed-settings.d/` 目录） | v2.1.83+ |

### 托管 Drop-ins

从 v2.1.83 起，管理员可以将多个托管设置文件部署到 `managed-settings.d/` 目录。文件按字母顺序合并，允许跨团队的模块化配置：

```
~/.claude/managed-settings.d/
  00-org-defaults.json
  10-team-policies.json
  20-project-overrides.json
```

### 可用的托管设置

| 设置 | 描述 |
|---------|-------------|
| `disableBypassPermissionsMode` | 防止用户启用绕过权限 |
| `availableModels` | 限制用户可以选择的模型 |
| `allowedChannelPlugins` | 控制允许使用哪些频道插件 |
| `autoMode.environment` | 为自动模式配置受信任的基础设施 |
| 自定义策略 | 组织特定的权限和工具策略 |

### 示例：macOS Plist

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
  "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>disableBypassPermissionsMode</key>
  <true/>
  <key>availableModels</key>
  <array>
    <string>claude-sonnet-4-6</string>
    <string>claude-haiku-4-5</string>
  </array>
</dict>
</plist>
```

---

## 配置和设置

### 配置文件位置

1. **全局配置**：`~/.claude/config.json`
2. **项目配置**：`./.claude/config.json`
3. **用户配置**：`~/.config/claude-code/settings.json`

### 完整配置示例

**核心高级功能配置：**

```json
{
  "permissions": {
    "mode": "default"
  },
  "hooks": {
    "PreToolUse:Edit": "eslint --fix ${file_path}",
    "PostToolUse:Write": "~/.claude/hooks/security-scan.sh"
  },
  "mcp": {
    "enabled": true,
    "servers": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"]
      }
    }
  }
}
```

**扩展配置示例：**

```json
{
  "permissions": {
    "mode": "default",
    "allowedTools": ["Bash(git log:*)", "Read"],
    "disallowedTools": ["Bash(rm -rf:*)"]
  },

  "hooks": {
    "PreToolUse": [{ "matcher": "Edit", "hooks": ["eslint --fix ${file_path}"] }],
    "PostToolUse": [{ "matcher": "Write", "hooks": ["~/.claude/hooks/security-scan.sh"] }],
    "Stop": [{ "hooks": ["~/.claude/hooks/notify.sh"] }]
  },

  "mcp": {
    "enabled": true,
    "servers": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "env": {
          "GITHUB_TOKEN": "${GITHUB_TOKEN}"
        }
      }
    }
  }
}
```

### 环境变量

使用环境变量覆盖配置：

```bash
# 模型选择
export ANTHROPIC_MODEL=claude-opus-4-6
export ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-4-6
export ANTHROPIC_DEFAULT_SONNET_MODEL=claude-sonnet-4-6
export ANTHROPIC_DEFAULT_HAIKU_MODEL=claude-haiku-4-5

# API 配置
export ANTHROPIC_API_KEY=sk-ant-...

# 思考配置
export MAX_THINKING_TOKENS=16000
export CLAUDE_CODE_EFFORT_LEVEL=high

# 功能切换
export CLAUDE_CODE_DISABLE_AUTO_MEMORY=true
export CLAUDE_CODE_DISABLE_BACKGROUND_TASKS=true
export CLAUDE_CODE_DISABLE_CRON=1
export CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS=true
export CLAUDE_CODE_DISABLE_TERMINAL_TITLE=true
export CLAUDE_CODE_DISABLE_1M_CONTEXT=true
export CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK=true
export CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false
export CLAUDE_CODE_ENABLE_TASKS=true
export CLAUDE_CODE_SIMPLE=true              # 由 --bare 标志设置

# MCP 配置
export MAX_MCP_OUTPUT_TOKENS=50000
export ENABLE_TOOL_SEARCH=true

# 任务管理
export CLAUDE_CODE_TASK_LIST_ID=my-project-tasks

# Agent 团队（实验性）
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=true

# 子代理和插件配置
export CLAUDE_CODE_SUBAGENT_MODEL=sonnet
export CLAUDE_CODE_PLUGIN_SEED_DIR=./my-plugins
export CLAUDE_CODE_NEW_INIT=true

# 子进程和流式
export CLAUDE_CODE_SUBPROCESS_ENV_SCRUB="SECRET_KEY,DB_PASSWORD"
export CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=80
export CLAUDE_STREAM_IDLE_TIMEOUT_MS=30000
export ANTHROPIC_CUSTOM_MODEL_OPTION=my-custom-model
export SLASH_COMMAND_TOOL_CHAR_BUDGET=50000
```

### 配置管理命令

```
用户：/config
[打开交互式配置菜单]
```

`/config` 命令提供交互式菜单来切换设置，例如：
- 扩展思考开/关
- 详细输出
- 权限模式
- 模型选择

### 每个项目的配置

在项目中创建 `.claude/config.json`：

```json
{
  "hooks": {
    "PreToolUse": [{ "matcher": "Bash", "hooks": ["npm test && npm run lint"] }]
  },
  "permissions": {
    "mode": "default"
  },
  "mcp": {
    "servers": {
      "project-db": {
        "command": "mcp-postgres",
        "env": {
          "DATABASE_URL": "${PROJECT_DB_URL}"
        }
      }
    }
  }
}
```

---

## 最佳实践

### 规划模式
- ✅ 用于复杂的多步任务
- ✅ 在批准之前审查计划
- ✅ 在需要时修改计划
- ❌ 不要用于简单任务

### 扩展思考
- ✅ 用于架构决策
- ✅ 用于复杂问题解决
- ✅ 审查推理过程
- ❌ 不要用于简单查询

### 后台任务
- ✅ 用于长时间运行的操作
- ✅ 监控任务进度
- ✅ 优雅处理任务失败
- ❌ 不要启动太多并发任务

### 权限
- ✅ 使用 `plan` 进行代码审查（只读）
- ✅ 使用 `default` 进行交互式开发
- ✅ 使用 `acceptEdits` 进行自动化工作流
- ✅ 使用 `auto` 进行带安全防护的自主工作
- ❌ 除非绝对必要，不要使用 `bypassPermissions`

### 会话
- ✅ 对不同任务使用单独的会话
- ✅ 保存重要的会话状态
- ✅ 清理旧会话
- ❌ 不要在一个会话中混合不相关的工作

---

## 其他资源

有关 Claude Code 和相关功能的更多信息：

- [官方交互模式文档](https://code.claude.com/docs/en/interactive-mode)
- [官方无头模式文档](https://code.claude.com/docs/en/headless)
- [CLI 参考](https://code.claude.com/docs/en/cli-reference)
- [检查点指南](../08-checkpoints/) - 会话管理和倒带
- [斜杠命令](../01-slash-commands/) - 命令参考
- [内存指南](../02-memory/) - 持久上下文
- [技能指南](../03-skills/) - 自主能力
- [子代理指南](../04-subagents/) - 委托任务执行
- [MCP 指南](../05-mcp/) - 外部数据访问
- [钩子指南](../06-hooks/) - 事件驱动的自动化
- [插件指南](../07-plugins/) - 捆绑扩展
- [官方计划任务文档](https://code.claude.com/docs/en/scheduled-tasks)
- [官方 Chrome 集成文档](https://code.claude.com/docs/en/chrome)
- [官方远程控制文档](https://code.claude.com/docs/en/remote-control)
- [官方键绑定文档](https://code.claude.com/docs/en/keybindings)
- [官方桌面应用文档](https://code.claude.com/docs/en/desktop)

