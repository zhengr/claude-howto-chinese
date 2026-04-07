# 变更日志

## v2.2.0 — 2026-03-26

### 文档

- 将所有教程和参考与 Claude Code v2.1.84 (f78c094) 同步 @luongnv89
  - 更新斜杠命令为 55+ 内置 + 5 个捆绑技能，标记 3 个为已弃用
  - 将钩子事件从 18 个扩展到 25 个，添加 `agent` 钩子类型（现为 4 种类型）
  - 添加 Auto Mode、Channels、Voice Dictation 到进阶功能
  - 添加 `effort`、`shell` 技能前置元数据字段；`initialPrompt`、`disallowedTools` 智能体字段
  - 添加 WebSocket MCP 传输、elicitation、2KB 工具上限
  - 添加插件 LSP 支持、`userConfig`、`${CLAUDE_PLUGIN_DATA}`
  - 更新所有参考文档（CATALOG、QUICK_REFERENCE、LEARNING-ROADMAP、INDEX）
- 将 README 重写为着陆页结构的指南 (32a0776) @luongnv89

### Bug 修复

- 添加缺失的 cSpell 词汇和 README 章节以符合 CI 要求 (93f9d51) @luongnv89
- 将 `Sandboxing` 添加到 cSpell 字典 (b80ce6f) @luongnv89

**完整变更日志**：https://github.com/luongnv89/claude-howto/compare/v2.1.1...v2.2.0

---

## v2.1.1 — 2026-03-13

### Bug 修复

- 移除导致 CI 链接检查失败的失效市场链接 (3fdf0d6) @luongnv89
- 将 `sandboxed` 和 `pycache` 添加到 cSpell 字典 (dc64618) @luongnv89

**完整变更日志**：https://github.com/luongnv89/claude-howto/compare/v2.1.0...v2.1.1

---

## v2.1.0 — 2026-03-13

### 功能

- 添加自适应学习路径，包含自我评估和课程测验技能 (1ef46cd) @luongnv89
  - `/self-assessment` — 涵盖 10 个功能领域的交互式能力测验，附带个性化学习路径
  - `/lesson-quiz [lesson]` — 每节课的知识测验，包含 8-10 个针对性问题

### Bug 修复

- 更新失效 URL、弃用内容和过时的引用 (8fe4520) @luongnv89
- 修复资源和自我评估技能中的失效链接 (7a05863) @luongnv89
- 在概念指南中使用波形符围栏处理嵌套代码块 (5f82719) @VikalpP
- 向 cSpell 字典添加缺失的词汇 (8df7572) @luongnv89

### 文档

- 第 5 阶段 QA — 修复文档间的一致性、URL 和术语问题 (00bbe4c) @luongnv89
- 完成第 3-4 阶段 — 新功能覆盖和参考文档更新 (132de29) @luongnv89
- 将 MCPorter 运行时添加到 MCP 上下文膨胀部分 (ef52705) @luongnv89
- 在 6 个指南中添加缺失的命令、功能和设置 (4bc8f15) @luongnv89
- 基于现有仓库约定添加风格指南 (84141d0) @luongnv89
- 在指南对比表中添加自我评估行 (8fe0c96) @luongnv89
- 在贡献者列表中添加 VikalpP（PR #7）(d5b4350) @luongnv89
- 在 README 和路线图中添加自我评估和课程测验技能引用 (d5a6106) @luongnv89

### 新贡献者

- @VikalpP 在 #7 中完成了首次贡献

**完整变更日志**：https://github.com/luongnv89/claude-howto/compare/v2.0.0...v2.1.0

---

## v2.0.0 — 2026-02-01

### 功能

- 将所有文档与 Claude Code 2026 年 2 月功能同步 (487c96d)
  - 更新全部 10 个教程目录和 7 个参考文档中的 26 个文件
  - 添加 **Auto Memory** 文档 — 每个项目的持久化学习
  - 添加 **Remote Control**、**Web Sessions** 和 **Desktop App** 文档
  - 添加 **Agent Teams**（实验性多智能体协作）文档
  - 添加 **MCP OAuth 2.0**、**Tool Search** 和 **Claude.ai Connectors** 文档
  - 添加 **Persistent Memory** 和子智能体 **Worktree Isolation** 文档
  - 添加 **Background Subagents**、**Task List**、**Prompt Suggestions** 文档
  - 添加 **Sandboxing** 和 **Managed Settings**（企业版）文档
  - 添加 **HTTP Hooks** 和 7 个新钩子事件文档
  - 添加 **Plugin Settings**、**LSP Servers** 和 Marketplace 更新文档
  - 添加 **Summarize from Checkpoint** 回放选项文档
  - 记录 17 个新斜杠命令（`/fork`、`/desktop`、`/teleport`、`/tasks`、`/fast` 等）
  - 记录新 CLI 标志（`--worktree`、`--from-pr`、`--remote`、`--teleport`、`--teammate-mode` 等）
  - 添加自动记忆、努力级别、智能体团队等新环境变量的文档

### 设计

- 重新设计 logo 为罗盘括号标记，使用简约调色板 (20779db)

### Bug 修复 / 更正

- 更新模型名称：Sonnet 4.5 → **Sonnet 4.6**，Opus 4.5 → **Opus 4.6**
- 修正权限模式名称：将虚构的"Unrestricted/Confirm/Read-only"替换为实际的 `default`/`acceptEdits`/`plan`/`dontAsk`/`bypassPermissions`
- 修正钩子事件：移除虚构的 `PreCommit`/`PostCommit`/`PrePush`，添加真实事件（`SubagentStart`、`WorktreeCreate`、`ConfigChange` 等）
- 修正 CLI 语法：将 `claude-code --headless` 替换为 `claude -p`（打印模式）
- 修正检查点命令：将虚构的 `/checkpoint save/list/rewind/diff` 替换为实际的 `Esc+Esc` / `/rewind` 界面
- 修话会话管理：将虚构的 `/session list/new/switch/save` 替换为真实的 `/resume`/`/rename`/`/fork`
- 修正插件清单格式：从 `plugin.yaml` 迁移到 `.claude-plugin/plugin.json`
- 修正 MCP 配置路径：`~/.claude/mcp.json` → `.mcp.json`（项目）/ `~/.claude.json`（用户）
- 修正文档 URL：`docs.claude.com` → `docs.anthropic.com`；移除虚构的 `plugins.claude.com`
- 移除多个文件中的虚构配置字段
- 将所有"最后更新"日期更新为 2026 年 2 月

**完整变更日志**：https://github.com/luongnv89/claude-howto/compare/20779db...v2.0.0
