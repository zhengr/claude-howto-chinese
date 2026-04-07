---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git diff:*)
argument-hint: [message]
description: Create a git commit with context
---

## 上下文

- 当前 git 状态: !`git status`
- 当前 git diff: !`git diff HEAD`
- 当前分支: !`git branch --show-current`
- 最近提交: !`git log --oneline -10`

## 你的任务

根据以上变更，创建一次 git 提交。

如果通过参数提供了信息，请使用它：$ARGUMENTS

否则，分析变更并按照约定式提交格式创建合适的提交信息：
- `feat:` 用于新功能
- `fix:` 用于修复 bug
- `docs:` 用于文档变更
- `refactor:` 用于代码重构
- `test:` 用于添加测试
- `chore:` 用于维护任务
