<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Hooks（钩子）

钩子是自动执行的脚本，会在 Claude Code 会话期间响应特定事件而运行。它们支持自动化、验证、权限管理和自定义工作流。

## 概述

钩子是在 Claude Code 中特定事件发生时自动执行的动作（Shell 命令、HTTP Webhook、LLM 提示词或子智能体评估）。它们接收 JSON 输入，并通过退出码和 JSON 输出进行通信。

**主要特性：**
- 事件驱动的自动化
- 基于 JSON 的输入/输出
- 支持 command（命令）、prompt（提示词）、HTTP 和 agent（智能体）四种钩子类型
- 通过模式匹配实现针对特定工具的钩子

## 配置

钩子在设置文件中进行配置，具有特定的结构：

- `~/.claude/settings.json` - 用户设置（适用于所有项目）
- `.claude/settings.json` - 项目设置（可共享，可提交）
- `.claude/settings.local.json` - 本地项目设置（不提交）
- 管理策略 - 组织范围的设置
- 插件 `hooks/hooks.json` - 插件作用域的钩子
- Skill/Agent frontmatter - 组件生命周期内的钩子

### 基本配置结构

```json
{
  "hooks": {
    "EventName": [
      {
        "matcher": "ToolPattern",
        "hooks": [
          {
            "type": "command",
            "command": "your-command-here",
            "timeout": 60
          }
        ]
      }
    ]
  }
}
```

**关键字段：**

| 字段 | 描述 | 示例 |
|-------|-------------|---------|
| `matcher` | 匹配工具名称的模式（区分大小写） | `"Write"`，`"Edit\|Write"`，`"*"` |
| `hooks` | 钩子定义数组 | `[{ "type": "command", ... }]` |
| `type` | 钩子类型：`"command"`（bash）、`"prompt"`（LLM）、`"http"`（webhook）或 `"agent"`（子智能体） | `"command"` |
| `command` | 要执行的 Shell 命令 | `"$CLAUDE_PROJECT_DIR/.claude/hooks/format.sh"` |
| `timeout` | 可选的超时时间（秒），默认 60 | `30` |
| `once` | 如果为 `true`，则每个会话只运行一次该钩子 | `true` |

### 匹配器模式

| 模式 | 描述 | 示例 |
|---------|-------------|---------|
| 精确字符串 | 匹配特定工具 | `"Write"` |
| 正则表达式 | 匹配多个工具 | `"Edit\|Write"` |
| 通配符 | 匹配所有工具 | `"*"` 或 `""` |
| MCP 工具 | 服务器和工具模式 | `"mcp__memory__.*"` |

## 钩子类型

Claude Code 支持四种钩子类型：

### Command Hooks（命令钩子）

默认钩子类型。执行 Shell 命令，通过 JSON stdin/stdout 和退出码进行通信。

```json
{
  "type": "command",
  "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate.py\"",
  "timeout": 60
}
```

### HTTP Hooks（HTTP 钩子）

> 于 v2.1.63 版本新增。

远程 Webhook 端点，接收与命令钩子相同的 JSON 输入。HTTP 钩子将 JSON POST 到 URL 并接收 JSON 响应。启用沙箱时，HTTP 钩子会通过沙箱路由。出于安全考虑，URL 中的环境变量插值需要显式的 `allowedEnvVars` 列表。

```json
{
  "hooks": {
    "PostToolUse": [{
      "type": "http",
      "url": "https://my-webhook.example.com/hook",
      "matcher": "Write"
    }]
  }
}
```

**关键属性：**
- `"type": "http"` -- 标识这是 HTTP 钩子
- `"url"` -- Webhook 端点 URL
- 启用沙箱时通过沙箱路由
- 对 URL 中环境变量的插值，需要显式的 `allowedEnvVars` 列表

### Prompt Hooks（提示词钩子）

LLM 评估的提示词，钩子内容是 Claude 评估的提示词。主要用于 `Stop` 和 `SubagentStop` 事件，用于智能任务完成检查。

```json
{
  "type": "prompt",
  "prompt": "Evaluate if Claude completed all requested tasks.",
  "timeout": 30
}
```

LLM 评估提示词并返回结构化决策（详见[基于提示词的钩子](#prompt-based-hooks)）。

### Agent Hooks（智能体钩子）

基于子智能体的验证钩子，启动专用智能体来评估条件或执行复杂检查。与提示词钩子（单轮 LLM 评估）不同，智能体钩子可以使用工具并执行多步推理。

```json
{
  "type": "agent",
  "prompt": "Verify the code changes follow our architecture guidelines. Check the relevant design docs and compare.",
  "timeout": 120
}
```

**关键属性：**
- `"type": "agent"` -- 标识这是智能体钩子
- `"prompt"` -- 子智能体的任务描述
- 智能体可以使用工具（Read、Grep、Bash 等）进行评估
- 返回与提示词钩子类似的结构化决策

## 钩子事件

Claude Code 支持 **25 个钩子事件**：

| 事件 | 触发时机 | 匹配器输入 | 可阻塞 | 常见用途 |
|-------|---------------|---------------|-----------|------------|
| **SessionStart** | 会话开始/恢复/清除/压缩 | startup/resume/clear/compact | 否 | 环境设置 |
| **InstructionsLoaded** | 加载 CLAUDE.md 或规则文件后 | (无) | 否 | 修改/过滤指令 |
| **UserPromptSubmit** | 用户提交提示词 | (无) | 是 | 验证提示词 |
| **PreToolUse** | 工具执行前 | 工具名称 | 是（允许/拒绝/询问） | 验证、修改输入 |
| **PermissionRequest** | 显示权限对话框 | 工具名称 | 是 | 自动批准/拒绝 |
| **PostToolUse** | 工具成功后 | 工具名称 | 否 | 添加上下文、反馈 |
| **PostToolUseFailure** | 工具执行失败 | 工具名称 | 否 | 错误处理、日志记录 |
| **Notification** | 发送通知 | 通知类型 | 否 | 自定义通知 |
| **SubagentStart** | 子智能体启动 | 智能体类型名称 | 否 | 子智能体设置 |
| **SubagentStop** | 子智能体完成 | 智能体类型名称 | 是 | 子智能体验证 |
| **Stop** | Claude 完成响应 | (无) | 是 | 任务完成检查 |
| **StopFailure** | API 错误结束轮次 | (无) | 否 | 错误恢复、日志记录 |
| **TeammateIdle** | 智能体团队成员空闲 | (无) | 是 | 团队成员协调 |
| **TaskCompleted** | 任务标记为完成 | (无) | 是 | 任务后操作 |
| **TaskCreated** | 通过 TaskCreate 创建任务 | (无) | 否 | 任务跟踪、日志记录 |
| **ConfigChange** | 配置文件更改 | (无) | 是（策略除外） | 响应配置更新 |
| **CwdChanged** | 工作目录更改 | (无) | 否 | 目录特定设置 |
| **FileChanged** | 监控的文件更改 | (无) | 否 | 文件监控、重新构建 |
| **PreCompact** | 上下文压缩前 | manual/auto | 否 | 压缩前操作 |
| **PostCompact** | 压缩完成后 | (无) | 否 | 压缩后操作 |
| **WorktreeCreate** | 正在创建工作树 | (无) | 是（路径返回） | 工作树初始化 |
| **WorktreeRemove** | 正在移除工作树 | (无) | 否 | 工作树清理 |
| **Elicitation** | MCP 服务器请求用户输入 | (无) | 是 | 输入验证 |
| **ElicitationResult** | 用户响应请求 | (无) | 是 | 响应处理 |
| **SessionEnd** | 会话终止 | (无) | 否 | 清理、最终日志记录 |

### PreToolUse

在 Claude 创建工具参数后、处理前运行。用于验证或修改工具输入。

**配置：**
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py"
          }
        ]
      }
    ]
  }
}
```

**常见匹配器：** `Task`、`Bash`、`Glob`、`Grep`、`Read`、`Edit`、`Write`、`WebFetch`、`WebSearch`

**输出控制：**
- `permissionDecision`：`"allow"`、`"deny"` 或 `"ask"`
- `permissionDecisionReason`：决策说明
- `updatedInput`：修改后的工具输入参数

### PostToolUse

在工具完成后立即运行。用于验证、日志记录或向 Claude 提供上下文。

**配置：**
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/security-scan.py"
          }
        ]
      }
    ]
  }
}
```

**输出控制：**
- `"block"` 决策会向 Claude 提示反馈
- `additionalContext`：为 Claude 添加的上下文

### UserPromptSubmit

在用户提交提示词时运行，Claude 处理之前。

**配置：**
```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/validate-prompt.py"
          }
        ]
      }
    ]
  }
}
```

**输出控制：**
- `decision`：`"block"` 以阻止处理
- `reason`：如果被阻止，说明原因
- `additionalContext`：添加到提示词的上下文

### Stop 和 SubagentStop

在 Claude 完成响应（Stop）或子智能体完成（SubagentStop）时运行。支持基于提示词的评估，用于智能任务完成检查。

**额外输入字段：** `Stop` 和 `SubagentStop` 钩子都在其 JSON 输入中接收 `last_assistant_message` 字段，包含 Claude 或子智能体在停止前的最终消息。这对于评估任务完成情况很有用。

**配置：**
```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Evaluate if Claude completed all requested tasks.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

### SubagentStart

在子智能体开始执行时运行。匹配器输入是智能体类型名称，允许钩子针对特定的子智能体类型。

**配置：**
```json
{
  "hooks": {
    "SubagentStart": [
      {
        "matcher": "code-review",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/subagent-init.sh"
          }
        ]
      }
    ]
  }
}
```

### SessionStart

在会话启动或恢复时运行。可以持久化环境变量。

**匹配器：** `startup`、`resume`、`clear`、`compact`

**特殊功能：** 使用 `CLAUDE_ENV_FILE` 持久化环境变量（在 `CwdChanged` 和 `FileChanged` 钩子中也可用）：

```bash
#!/bin/bash
if [ -n "$CLAUDE_ENV_FILE" ]; then
  echo 'export NODE_ENV=development' >> "$CLAUDE_ENV_FILE"
fi
exit 0
```

### SessionEnd

在会话结束时运行，执行清理或最终日志记录。无法阻止终止。

**reason 字段值：**
- `clear` - 用户清除了会话
- `logout` - 用户登出
- `prompt_input_exit` - 用户通过提示输入退出
- `other` - 其他原因

**配置：**
```json
{
  "hooks": {
    "SessionEnd": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/session-cleanup.sh\""
          }
        ]
      }
    ]
  }
}
```

### Notification 事件

通知事件的更新匹配器：
- `permission_prompt` - 权限请求通知
- `idle_prompt` - 空闲状态通知
- `auth_success` - 认证成功
- `elicitation_dialog` - 向用户显示的对话框

## 组件作用域钩子

钩子可以附加到特定组件（skills、agents、commands）的 frontmatter 中：

**在 SKILL.md、agent.md 或 command.md 中：**

```yaml
---
name: secure-operations
description: Perform operations with security checks
hooks:
  PreToolUse:
  - matcher: "Bash"
    hooks:
    - type: command
      command: "./scripts/check.sh"
      once: true # 每个会话只运行一次
---
```

**组件钩子支持的事件：** `PreToolUse`、`PostToolUse`、`Stop`

这允许直接在使用钩子的组件中定义钩子，保持相关代码在一起。

### 子智能体 Frontmatter 中的钩子

当在子智能体的 frontmatter 中定义 `Stop` 钩子时，它会自动转换为针对该子智能体的 `SubagentStop` 钩子。这确保停止钩子仅在该特定子智能体完成时触发，而不是在主会话停止时触发。

```yaml
---
name: code-review-agent
description: Automated code review subagent
hooks:
  Stop:
  - hooks:
    - type: prompt
      prompt: "Verify the code review is thorough and complete."
    # 上面的 Stop 钩子会自动转换为该子智能体的 SubagentStop
---
```

## PermissionRequest 事件

使用自定义输出格式处理权限请求：

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PermissionRequest",
    "decision": {
      "behavior": "allow|deny",
      "updatedInput": {},
      "message": "Custom message",
      "interrupt": false
    }
  }
}
```

## 钩子输入和输出

### JSON 输入（通过 stdin）

所有钩子都通过 stdin 接收 JSON 输入：

```json
{
  "session_id": "abc123",
  "transcript_path": "/path/to/transcript.jsonl",
  "cwd": "/current/working/directory",
  "permission_mode": "default",
  "hook_event_name": "PreToolUse",
  "tool_name": "Write",
  "tool_input": {
    "file_path": "/path/to/file.js",
    "content": "..."
  },
  "tool_use_id": "toolu_01ABC123...",
  "agent_id": "agent-abc123",
  "agent_type": "main",
  "worktree": "/path/to/worktree"
}
```

**常见字段：**

| 字段 | 描述 |
|-------|-------------|
| `session_id` | 唯一会话标识符 |
| `transcript_path` | 对话记录文件路径 |
| `cwd` | 当前工作目录 |
| `hook_event_name` | 触发钩子的事件名称 |
| `agent_id` | 运行此钩子的智能体标识符 |
| `agent_type` | 智能体类型（`"main"`、子智能体类型名称等） |
| `worktree` | git 工作树路径（如果智能体在其中运行） |

### 退出码

| 退出码 | 含义 | 行为 |
|-----------|---------|----------|
| **0** | 成功 | 继续，解析 JSON stdout |
| **2** | 阻塞错误 | 阻止操作，stderr 显示为错误 |
| **其他** | 非阻塞错误 | 继续，stderr 在详细模式下显示 |

### JSON 输出（stdout，退出码 0）

```json
{
  "continue": true,
  "stopReason": "Optional message if stopping",
  "suppressOutput": false,
  "systemMessage": "Optional warning message",
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "allow",
    "permissionDecisionReason": "File is in allowed directory",
    "updatedInput": {
      "file_path": "/modified/path.js"
    }
  }
}
```

## 环境变量

| 变量 | 可用性 | 描述 |
|----------|-------------|-------------|
| `CLAUDE_PROJECT_DIR` | 所有钩子 | 项目根目录的绝对路径 |
| `CLAUDE_ENV_FILE` | SessionStart、CwdChanged、FileChanged | 持久化环境变量的文件路径 |
| `CLAUDE_CODE_REMOTE` | 所有钩子 | 如果在远程环境中运行则为 `"true"` |
| `${CLAUDE_PLUGIN_ROOT}` | 插件钩子 | 插件目录路径 |
| `${CLAUDE_PLUGIN_DATA}` | 插件钩子 | 插件数据目录路径 |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEnd 钩子 | SessionEnd 钩子的可配置超时时间（毫秒，覆盖默认值） |

## 基于提示词的钩子

对于 `Stop` 和 `SubagentStop` 事件，可以使用基于 LLM 的评估：

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Review if all tasks are complete. Return your decision.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

**LLM 响应模式：**

```json
{
  "decision": "approve",
  "reason": "All tasks completed successfully",
  "continue": false,
  "stopReason": "Task complete"
}
```