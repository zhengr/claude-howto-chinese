---
description: Clean up code, stage changes, and prepare a pull request
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git diff:*), Bash(npm test:*), Bash(npm run lint:*)
---

# Pull Request 准备检查清单

创建 PR 之前，请执行以下步骤：

1. 运行代码检查：`prettier --write .`
2. 运行测试：`npm test`
3. 检查 git diff：`git diff HEAD`
4. 暂存变更：`git add .`
5. 使用约定式提交格式创建提交信息：
   - `fix:` 用于修复 bug
   - `feat:` 用于新功能
   - `docs:` 用于文档
   - `refactor:` 用于代码重构
   - `test:` 用于添加测试
   - `chore:` 用于维护任务

6. 生成 PR 摘要，包括：
   - 变更内容
   - 变更原因
   - 已进行的测试
   - 潜在影响
