<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code 功能目录

> Claude Code 功能快速参考指南：命令、智能体、技能、插件和钩子。

**导航**: [命令](#斜杠命令) | [权限模式](#权限模式) | [子智能体](#子智能体) | [技能](#技能) | [插件](#插件) | [MCP 服务器](#mcp-服务器) | [钩子](#钩子) | [记忆文件](#记忆文件) | [新功能](#新功能-2026年3月)

---

## 摘要

| 功能 | 内置 | 示例 | 总计 | 参考 |
|------|------|------|------|------|
| **斜杠命令** | 55+ | 8 | 63+ | [01-slash-commands/](01-slash-commands/) |
| **子智能体** | 6 | 10 | 16 | [04-subagents/](04-subagents/) |
| **技能** | 5 个捆绑 | 4 | 9 | [03-skills/](03-skills/) |
| **插件** | - | 3 | 3 | [07-plugins/](07-plugins/) |
| **MCP 服务器** | 1 | 8 | 9 | [05-mcp/](05-mcp/) |
| **钩子** | 25 个事件 | 7 | 7 | [06-hooks/](06-hooks/) |
| **记忆** | 7 种类型 | 3 | 3 | [02-memory/](02-memory/) |
| **总计** | **99** | **43** | **117** | |

---

## 斜杠命令

命令是用户调用的快捷方式，执行特定操作。

### 内置命令

| 命令 | 描述 | 何时使用 |
|------|------|----------|
| `/help` | 显示帮助信息 | 入门，学习命令 |
| `/btw` | 附加问题但不添加到上下文 | 快速跑题问题 |
| `/chrome` | 配置 Chrome 集成 | 浏览器自动化 |
| `/clear` | 清除对话历史 | 重新开始，减少上下文 |
| `/diff` | 交互式差异查看器 | 查看更改 |
| `/config` | 查看/编辑配置 | 自定义行为 |
| `/status` | 显示会话状态 | 检查当前状态 |
| `/agents` | 列出可用智能体 | 查看委派选项 |
| `/skills` | 列出可用技能 | 查看自动调用功能 |
| `/hooks` | 列出已配置钩子 | 调试自动化 |
| `/insights` | 分析会话模式 | 会话优化 |
| `/install-slack-app` | 安装 Claude Slack 应用 | Slack 集成 |
| `/keybindings` | 自定义键盘快捷键 | 键自定义 |
| `/mcp` | 列出 MCP 服务器 | 检查外部集成 |
| `/memory` | 查看已加载的记忆文件 | 调试上下文加载 |
| `/mobile` | 生成移动设备二维码 | 移动访问 |
| `/passes` | 查看使用次数 | 订阅信息 |
| `/plugin` | 管理插件 | 安装/移除扩展 |
| `/plan` | 进入规划模式 | 复杂实现 |
| `/rewind` | 回退到检查点 | 撤销更改，探索替代方案 |
| `/checkpoint` | 管理检查点 | 保存/恢复状态 |
| `/cost` | 显示 token 使用成本 | 监控支出 |
| `/context` | 显示上下文窗口使用情况 | 管理对话长度 |
| `/export` | 导出对话 | 保存参考 |
| `/extra-usage` | 配置额外使用限制 | 速率限制管理 |
| `/feedback` | 提交反馈或 bug 报告 | 报告问题 |
| `/login` | 与 Anthropic 身份验证 | 访问功能 |
| `/logout` | 退出 | 切换账户 |
| `/sandbox` | 切换沙盒模式 | 安全命令执行 |
| `/vim` | 切换 vim 模式 | Vim 样式编辑 |
| `/doctor` | 运行诊断 | 故障排除 |
| `/reload-plugins` | 重新加载已安装插件 | 插件管理 |
| `/release-notes` | 显示发布说明 | 检查新功能 |
| `/remote-control` | 启用远程控制 | 远程访问 |
| `/permissions` | 管理权限 | 控制访问 |
| `/session` | 管理会话 | 多会话工作流 |
| `/rename` | 重命名当前会话 | 组织会话 |
| `/resume` | 恢复之前的会话 | 继续工作 |
| `/todo` | 查看/管理待办事项列表 | 跟踪任务 |
| `/tasks` | 查看后台任务 | 监控异步操作 |
| `/copy` | 复制最后响应到剪贴板 | 快速共享输出 |
| `/teleport` | 将会话转移到另一台机器 | 远程继续工作 |
| `/desktop` | 打开 Claude 桌面应用 | 切换到桌面界面 |
| `/theme` | 更改颜色主题 | 自定义外观 |
| `/usage` | 显示 API 使用统计 | 监控配额和成本 |
| `/fork` | 分支当前对话 | 探索替代方案 |
| `/stats` | 显示会话统计 | 查看会话指标 |
| `/statusline` | 配置状态行 | 自定义状态显示 |
| `/stickers` | 查看会话贴纸 | 有趣的奖励 |
| `/fast` | 切换快速输出模式 | 加速响应 |
| `/terminal-setup` | 配置终端集成 | 设置终端功能 |
| `/upgrade` | 检查更新 | 版本管理 |

### 自定义命令（示例）

| 命令 | 描述 | 何时使用 | 范围 | 安装 |
|------|------|----------|------|------|
| `/optimize` | 分析代码优化 | 性能改进 | 项目 | `cp 01-slash-commands/optimize.md .claude/commands/` |
| `/pr` | 准备拉取请求 | 提交 PR 前 | 项目 | `cp 01-slash-commands/pr.md .claude/commands/` |
| `/generate-api-docs` | 生成 API 文档 | 文档化 API | 项目 | `cp 01-slash-commands/generate-api-docs.md .claude/commands/` |
| `/commit` | 创建带有上下文的 git 提交 | 提交更改 | 用户 | `cp 01-slash-commands/commit.md .claude/commands/` |
| `/push-all` | 暂存、提交和推送 | 快速部署 | 用户 | `cp 01-slash-commands/push-all.md .claude/commands/` |
| `/doc-refactor` | 重构文档 | 改进文档 | 项目 | `cp 01-slash-commands/doc-refactor.md .claude/commands/` |
| `/setup-ci-cd` | 设置 CI/CD 流水线 | 新项目 | 项目 | `cp 01-slash-commands/setup-ci-cd.md .claude/commands/` |
| `/unit-test-expand` | 扩展测试覆盖 | 改进测试 | 项目 | `cp 01-slash-commands/unit-test-expand.md .claude/commands/` |

> **范围**: `用户` = 个人工作流 (`~/.claude/commands/`)，`项目` = 团队共享 (`.claude/commands/`)

**参考**: [01-slash-commands/](01-slash-commands/) | [官方文档](https://code.claude.com/docs/en/interactive-mode)

**快速安装（所有自定义命令）**:
```bash
cp 01-slash-commands/*.md .claude/commands/
```

---

## 权限模式

Claude Code 支持 6 种权限模式，控制工具使用授权。

| 模式 | 描述 | 何时使用 |
|------|------|----------|
| `default` | 提示每个工具调用 | 标准交互使用 |
| `acceptEdits` | 自动接受文件编辑，其他操作需提示 | 受信任的编辑工作流 |
| `plan` | 只读工具，无写入 | 规划和探索 |
| `auto` | 无提示接受所有工具 | 完全自主操作（研究预览版） |
| `bypassPermissions` | 跳过所有权限检查 | CI/CD、无头环境 |
| `dontAsk` | 跳过需要权限的工具 | 非交互式脚本 |

> **注意**: `auto` 模式是研究预览功能（2026 年 3 月）。仅在受信任的沙盒环境中使用 `bypassPermissions`。

**参考**: [官方文档](https://code.claude.com/docs/en/permissions)

---

## 子智能体

具有隔离上下文的专业 AI 助手，处理特定任务。

### 内置子智能体

| 智能体 | 描述 | 工具 | 模型 | 何时使用 |
|-------|------|------|------|----------|
| **通用型** | 多步骤任务、研究 | 所有工具 | 继承模型 | 复杂研究、多文件任务 |
| **规划** | 实现规划 | Read、Glob、Grep、Bash | 继承模型 | 架构设计、规划 |
| **探索** | 代码库探索 | Read、Glob、Grep | Haiku 4.5 | 快速搜索、理解代码 |
| **Bash** | 命令执行 | Bash | 继承模型 | Git 操作、终端任务 |
| **状态行设置** | 状态行配置 | Bash、Read、Write | Sonnet 4.6 | 配置状态行显示 |
| **Claude Code 指南** | 帮助和文档 | Read、Glob、Grep | Haiku 4.5 | 获取帮助、学习功能 |

### 子智能体配置字段

| 字段 | 类型 | 描述 |
|------|------|------|
| `name` | 字符串 | 智能体标识符 |
| `description` | 字符串 | 智能体功能描述 |
| `model` | 字符串 | 模型覆盖（如 `haiku-4.5`） |
| `tools` | 数组 | 允许工具列表 |
| `effort` | 字符串 | 推理努力级别（`low`、`medium`、`high`） |
| `initialPrompt` | 字符串 | 智能体启动时注入的系统提示 |
| `disallowedTools` | 数组 | 明确拒绝此智能体的工具 |

### 自定义子智能体（示例）

| 智能体 | 描述 | 何时使用 | 范围 | 安装 |
|-------|------|----------|------|------|
| `code-reviewer` | 全面的代码质量 | 代码审查会话 | 项目 | `cp 04-subagents/code-reviewer.md .claude/agents/` |
| `code-architect` | 功能架构设计 | 新功能规划 | 项目 | `cp 04-subagents/code-architect.md .claude/agents/` |
| `code-explorer` | 深度代码库分析 | 理解现有功能 | 项目 | `cp 04-subagents/code-explorer.md .claude/agents/` |
| `clean-code-reviewer` | 整洁代码原则审查 | 可维护性审查 | 项目 | `cp 04-subagents/clean-code-reviewer.md .claude/agents/` |
| `test-engineer` | 测试策略和覆盖 | 测试规划 | 项目 | `cp 04-subagents/test-engineer.md .claude/agents/` |
| `documentation-writer` | 技术文档 | API 文档、指南 | 项目 | `cp 04-subagents/documentation-writer.md .claude/agents/` |
| `secure-reviewer` | 安全审查 | 安全审计 | 项目 | `cp 04-subagents/secure-reviewer.md .claude/agents/` |
| `implementation-agent` | 完整功能实现 | 功能开发 | 项目 | `cp 04-subagents/implementation-agent.md .claude/agents/` |
| `debugger` | 根因分析 | Bug 调查 | 用户 | `cp 04-subagents/debugger.md .claude/agents/` |
| `data-scientist` | SQL 查询、数据分析 | 数据任务 | 用户 | `cp 04-subagents/data-scientist.md .claude/agents/` |

> **范围**: `用户` = 个人 (`~/.claude/agents/`)，`项目` = 团队共享 (`.claude/agents/`)

**参考**: [04-subagents/](04-subagents/) | [官方文档](https://code.claude.com/docs/en/sub-agents)

**快速安装（所有自定义智能体）**:
```bash
cp 04-subagents/*.md .claude/agents/
```

---

## 技能

自动调用的功能，包含指令、脚本和模板。

### 示例技能

| 技能 | 描述 | 何时自动调用 | 范围 | 安装 |
|------|------|--------------|------|------|
| `code-review` | 全面代码审查 | "审查这段代码"，"检查质量" | 项目 | `cp -r 03-skills/code-review .claude/skills/` |
| `brand-voice` | 品牌一致性检查器 | 编写营销文案时 | 项目 | `cp -r 03-skills/brand-voice .claude/skills/` |
| `doc-generator` | API 文档生成器 | "生成文档"，"文档化 API" | 项目 | `cp -r 03-skills/doc-generator .claude/skills/` |
| `refactor` | 系统性代码重构（Martin Fowler） | "重构这段代码"，"清理代码" | 用户 | `cp -r 03-skills/refactor ~/.claude/skills/` |

> **范围**: `用户` = 个人 (`~/.claude/skills/`)，`项目` = 团队共享 (`.claude/skills/`)

### 技能结构

```
~/.claude/skills/skill-name/
├── SKILL.md          # 技能定义和指令
├── scripts/          # 辅助脚本
└── templates/        # 输出模板
```

### 技能前置字段

技能在 `SKILL.md` 中支持 YAML 前置字段进行配置：

| 字段 | 类型 | 描述 |
|------|------|------|
| `name` | 字符串 | 技能显示名称 |
| `description` | 字符串 | 技能功能描述 |
| `autoInvoke` | 数组 | 自动调用触发短语 |
| `effort` | 字符串 | 推理努力级别（`low`、`medium`、`high`） |
| `shell` | 字符串 | 脚本使用的 shell（`bash`、`zsh`、`sh`） |

**参考**: [03-skills/](03-skills/) | [官方文档](https://code.claude.com/docs/en/skills)

**快速安装（所有技能）**:
```bash
cp -r 03-skills/* ~/.claude/skills/
```

### 捆绑技能

| 技能 | 描述 | 何时自动调用 |
|------|------|--------------|
| `/simplify` | 检查代码质量 | 编写代码后 |
| `/batch` | 在多个文件上运行提示词 | 批量操作 |
| `/debug` | 调试失败的测试/错误 | 调试会话 |
| `/loop` | 间隔运行提示词 | 定期任务 |
| `/claude-api` | 使用 Claude API 构建应用 | API 开发 |

---

## 插件

命令、智能体、MCP 服务器和钩子的捆绑集合。

### 示例插件

| 插件 | 描述 | 组件 | 何时使用 | 范围 | 安装 |
|------|------|------|----------|------|------|
| `pr-review` | PR 审查工作流 | 3 个命令、3 个智能体、GitHub MCP | 代码审查 | 项目 | `/plugin install pr-review` |
| `devops-automation` | 部署和监控 | 4 个命令、3 个智能体、K8s MCP | DevOps 任务 | 项目 | `/plugin install devops-automation` |
| `documentation` | 文档生成套件 | 4 个命令、3 个智能体、模板 | 文档 | 项目 | `/plugin install documentation` |

> **范围**: `项目` = 团队共享，`用户` = 个人工作流

### 插件结构

```
.claude-plugin/
├── plugin.json       # 清单文件
├── commands/         # 斜杠命令
├── agents/           # 子智能体
├── skills/           # 技能
├── mcp/              # MCP 配置
├── hooks/            # 钩子脚本
└── scripts/          # 实用脚本
```

**参考**: [07-plugins/](07-plugins/) | [官方文档](https://code.claude.com/docs/en/plugins)

**插件管理命令**:
```bash
/plugin list              # 列出已安装插件
/plugin install <name>    # 安装插件
/plugin remove <name>     # 移除插件
/plugin update <name>     # 更新插件
```

---

## MCP 服务器

用于外部工具和 API 访问的模型上下文协议服务器。

### 常见 MCP 服务器

| 服务器 | 描述 | 何时使用 | 范围 | 安装 |
|--------|------|----------|------|------|
| **GitHub** | PR 管理、问题、代码 | GitHub 工作流 | 项目 | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| **Database** | SQL 查询、数据访问 | 数据库操作 | 项目 | `claude mcp add db -- npx -y @modelcontextprotocol/server-postgres` |
| **Filesystem** | 高级文件操作 | 复杂文件任务 | 用户 | `claude mcp add fs -- npx -y @modelcontextprotocol/server-filesystem` |
| **Slack** | 团队沟通 | 通知、更新 | 项目 | 在设置中配置 |
| **Google Docs** | 文档访问 | 文档编辑、审查 | 项目 | 在设置中配置 |
| **Asana** | 项目管理 | 任务跟踪 | 项目 | 在设置中配置 |
| **Stripe** | 支付数据 | 财务分析 | 项目 | 在设置中配置 |
| **Memory** | 持久记忆 | 跨会话回忆 | 用户 | 在设置中配置 |
| **Context7** | 库文档 | 最新文档查找 | 内置 | 内置 |

> **范围**: `项目` = 团队 (`.mcp.json`)，`用户` = 个人 (`~/.claude.json`)，`内置` = 预安装

### MCP 配置示例

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**参考**: [05-mcp/](05-mcp/) | [MCP 协议文档](https://modelcontextprotocol.io)

**快速安装（GitHub MCP）**:
```bash
export GITHUB_TOKEN="your_token" && claude mcp add github -- npx -y @modelcontextprotocol/server-github
```

---

## 钩子

事件驱动的自动化，在 Claude Code 事件上执行 shell 命令。

### 钩子事件

| 事件 | 描述 | 触发时间 | 使用场景 |
|------|------|----------|----------|
| `SessionStart` | 会话开始/恢复 | 会话初始化 | 设置任务 |
| `InstructionsLoaded` | 指令已加载 | CLAUDE.md 或规则文件加载 | 自定义指令处理 |
| `UserPromptSubmit` | 提示词提交前 | 用户发送消息 | 输入验证 |
| `PreToolUse` | 工具执行前 | 任何工具运行前 | 验证、日志 |
| `PermissionRequest` | 权限对话框显示 | 敏感操作前 | 自定义批准流程 |
| `PostToolUse` | 工具成功后 | 任何工具完成后 | 格式化、通知 |
| `PostToolUseFailure` | 工具执行失败 | 工具错误后 | 错误处理、日志 |
| `Notification` | 通知发送 | Claude 发送通知 | 外部警报 |
| `SubagentStart` | 子智能体生成 | 子智能体任务开始 | 初始化子智能体上下文 |
| `SubagentStop` | 子智能体完成 | 子智能体任务完成 | 链接操作 |
| `Stop` | Claude 完成响应 | 响应完成 | 清理、报告 |
| `StopFailure` | API 错误结束轮次 | API 错误发生 | 错误恢复、日志 |
| `TeammateIdle` | 智能体队友空闲 | 智能体团队协调 | 分配工作 |
| `TaskCompleted` | 任务标记完成 | 任务完成 | 后处理 |
| `TaskCreated` | 通过 TaskCreate 创建任务 | 新任务创建 | 任务跟踪、日志 |
| `ConfigChange` | 配置已更新 | 设置修改 | 响应配置更改 |
| `CwdChanged` | 工作目录更改 | 目录更改 | 目录特定设置 |
| `FileChanged` | 监控的文件更改 | 文件修改 | 文件监控、重建 |
| `PreCompact` | 压缩操作前 | 上下文压缩 | 状态保存 |
| `PostCompact` | 压缩完成后 | 压缩完成 | 压缩后操作 |
| `WorktreeCreate` | 创建工作树时 | Git 工作树创建 | 设置工作树环境 |
| `WorktreeRemove` | 移除工作树时 | Git 工作树移除 | 清理工作树资源 |
| `Elicitation` | MCP 服务器请求输入 | MCP 交互 | 输入验证 |
| `ElicitationResult` | 用户响应交互 | 用户响应 | 响应处理 |
| `SessionEnd` | 会话终止 | 会话终止 | 清理、保存状态 |

### 示例钩子

| 钩子 | 描述 | 事件 | 范围 | 安装 |
|------|------|------|------|------|
| `validate-bash.py` | 命令验证 | PreToolUse:Bash | 项目 | `cp 06-hooks/validate-bash.py .claude/hooks/` |
| `security-scan.py` | 安全扫描 | PostToolUse:Write | 项目 | `cp 06-hooks/security-scan.py .claude/hooks/` |
| `format-code.sh` | 自动格式化 | PostToolUse:Write | 用户 | `cp 06-hooks/format-code.sh ~/.claude/hooks/` |
| `validate-prompt.py` | 提示词验证 | UserPromptSubmit | 项目 | `cp 06-hooks/validate-prompt.py .claude/hooks/` |
| `context-tracker.py` | Token 使用跟踪 | Stop | 用户 | `cp 06-hooks/context-tracker.py ~/.claude/hooks/` |
| `pre-commit.sh` | 提交前验证 | PreToolUse:Bash | 项目 | `cp 06-hooks/pre-commit.sh .claude/hooks/` |
| `log-bash.sh` | 命令日志 | PostToolUse:Bash | 用户 | `cp 06-hooks/log-bash.sh ~/.claude/hooks/` |

> **范围**: `项目` = 团队 (`.claude/settings.json`)，`用户` = 个人 (`~/.claude/settings.json`)

### 钩子配置

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "~/.claude/hooks/validate-bash.py"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "~/.claude/hooks/format-code.sh"
      }
    ]
  }
}
```

**参考**: [06-hooks/](06-hooks/) | [官方文档](https://code.claude.com/docs/en/hooks)

**快速安装（所有钩子）**:
```bash
mkdir -p ~/.claude/hooks && cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh
```

---

## 记忆文件

自动加载的持久化上下文，跨会话保持。

### 记忆类型

| 类型 | 位置 | 范围 | 何时使用 |
|------|------|------|----------|
| **管理策略** | 组织管理的策略 | 组织 | 执行组织范围标准 |
| **项目** | `./CLAUDE.md` | 项目（团队） | 团队标准、项目上下文 |
| **项目规则** | `.claude/rules/` | 项目（团队） | 模块化项目规则 |
| **用户** | `~/.claude/CLAUDE.md` | 用户（个人） | 个人偏好设置 |
| **用户规则** | `~/.claude/rules/` | 用户（个人） | 模块化个人规则 |
| **本地** | `./CLAUDE.local.md` | 本地（git 忽略） | 机器特定覆盖（截至 2026 年 3 月官方文档未提及；可能是遗留功能） |
| **自动记忆** | 自动 | 会话 | 自动捕获的见解和修正 |

> **范围**: `组织` = 管理员管理，`项目` = 通过 git 与团队共享，`用户` = 个人偏好设置，`本地` = 不提交，`会话` = 自动管理

**参考**: [02-memory/](02-memory/) | [官方文档](https://code.claude.com/docs/en/memory)

**快速安装**:
```bash
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

---

## 新功能（2026 年 3 月）

| 功能 | 描述 | 如何使用 |
|------|------|----------|
| **远程控制** | 通过 API 远程控制 Claude Code 会话 | 使用远程控制 API 以编程方式发送提示词并接收响应 |
| **Web 会话** | 在基于浏览器的环境中运行 Claude Code | 通过 `claude web` 访问或通过 Anthropic 控制台 |
| **桌面应用** | Claude Code 的原生桌面应用 | 使用 `/desktop` 或从 Anthropic 网站下载 |
| **智能体团队** | 协调多个智能体处理相关任务 | 配置共享上下文的队友智能体 |
| **任务列表** | 后台任务管理和监控 | 使用 `/tasks` 查看和管理后台操作 |
| **提示建议** | 上下文感知的命令建议 | 根据当前上下文自动显示建议 |
| **Git 工作树** | 并行开发的隔离 git 工作树 | 使用工作树命令进行安全并行分支工作 |
| **沙盒化** | 安全执行的隔离环境 | 使用 `/sandbox` 切换；在受限环境中运行命令 |
| **MCP OAuth** | MCP 服务器的 OAuth 身份验证 | 在 MCP 服务器设置中配置 OAuth 凭证以实现安全访问 |
| **MCP 工具搜索** | 动态搜索和发现 MCP 工具 | 使用工具搜索查找已连接服务器中的可用 MCP 工具 |
| **定时任务** | 使用 `/loop` 和 cron 工具设置定期任务 | 使用 `/loop 5m /command` 或 CronCreate 工具 |
| **Chrome 集成** | 使用无头 Chromium 进行浏览器自动化 | 使用 `--chrome` 标志或 `/chrome` 命令 |
| **键盘自定义** | 自定义键绑定，包括组合键支持 | 使用 `/keybindings` 或编辑 `~/.claude/keybindings.json` |
| **自动模式** | 无权限提示的完全自主操作（研究预览版） | 使用 `--mode auto` 或 `/permissions auto`；2026 年 3 月 |
| **频道** | 多频道通信（Telegram、Slack 等）（研究预览版） | 配置频道插件；2026 年 3 月 |
| **语音听写** | 提示词语音输入 | 使用麦克风图标或语音快捷键 |
| **智能体钩子类型** | 生成子智能体而非运行 shell 命令的钩子 | 在钩子配置中设置 `"type": "agent"` |
| **提示钩子类型** | 将提示词文本注入对话的钩子 | 在钩子配置中设置 `"type": "prompt"` |
| **MCP 交互** | MCP 服务器可在工具执行期间请求用户输入 | 通过 `Elicitation` 和 `ElicitationResult` 钩子事件处理 |
| **WebSocket MCP 传输** | 基于 WebSocket 的 MCP 服务器连接传输 | 在 MCP 服务器配置中使用 `"transport": "websocket"` |
| **插件 LSP 支持** | 通过插件的语言服务器协议集成 | 在 `plugin.json` 中配置 LSP 服务器以实现编辑器功能 |
| **管理嵌入式配置** | 组织管理的嵌入式配置（v2.1.83） | 管理员通过管理策略配置；自动应用于所有用户 |

---

## 快速参考矩阵

### 功能选择指南

| 需求 | 推荐功能 | 原因 |
|------|----------|------|
| 快捷方式 | 斜杠命令 | 手动、即时 |
| 持久化上下文 | 记忆 | 自动加载 |
| 复杂自动化 | 技能 | 自动调用 |
| 专业任务 | 子智能体 | 隔离上下文 |
| 外部数据 | MCP 服务器 | 实时访问 |
| 事件自动化 | 钩子 | 事件触发 |
| 完整解决方案 | 插件 | 一体化捆绑 |

### 安装优先级

| 优先级 | 功能 | 命令 |
|--------|------|------|
| 1. 必需 | 记忆 | `cp 02-memory/project-CLAUDE.md ./CLAUDE.md` |
| 2. 日常使用 | 斜杠命令 | `cp 01-slash-commands/*.md .claude/commands/` |
| 3. 质量 | 子智能体 | `cp 04-subagents/*.md .claude/agents/` |
| 4. 自动化 | 钩子 | `cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh` |
| 5. 外部集成 | MCP | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| 6. 高级功能 | 技能 | `cp -r 03-skills/* ~/.claude/skills/` |
| 7. 完整方案 | 插件 | `/plugin install pr-review` |

---

## 完整一键安装

安装此存储库中的所有示例：

```bash
# 创建目录
mkdir -p .claude/{commands,agents,skills} ~/.claude/{hooks,skills}

# 安装所有功能
cp 01-slash-commands/*.md .claude/commands/ && \
cp 02-memory/project-CLAUDE.md ./CLAUDE.md && \
cp -r 03-skills/* ~/.claude/skills/ && \
cp 04-subagents/*.md .claude/agents/ && \
cp 06-hooks/*.sh ~/.claude/hooks/ && \
chmod +x ~/.claude/hooks/*.sh
```

---

## 其他资源

- [Claude Code 官方文档](https://code.claude.com/docs/en/overview)
- [MCP 协议规范](https://modelcontextprotocol.io)
- [学习路线图](LEARNING-ROADMAP.md)
- [主要 README](README.md)

---

**最后更新**: 2026 年 3 月
