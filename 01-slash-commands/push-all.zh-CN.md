---
description: Stage all changes, create commit, and push to remote (use with caution)
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git push:*), Bash(git diff:*), Bash(git log:*), Bash(git pull:*)
---

# 提交并推送所有内容

⚠️ **注意**：暂存**所有**更改，提交并推送到远程。仅在确定所有更改属于同一批次时使用。

## 工作流程

### 1. 分析更改
并行运行：
- `git status` — 显示修改/新增/删除/未跟踪的文件
- `git diff --stat` — 显示更改统计
- `git log -1 --oneline` — 显示最近的提交以参考消息风格

### 2. 安全检查

**❌ 检测到时停止並警告：**
- 密钥文件：`.env*`、`*.key`、`*.pem`、`credentials.json`、`secrets.yaml`、`id_rsa`、`*.p12`、`*.pfx`、`*.cer`
- API 密钥：任何包含真实值（而非占位符如 `your-api-key`、`xxx`、`placeholder`）的 `*_API_KEY`、`*_SECRET`、`*_TOKEN` 变量
- 大文件：`>10MB` 且未使用 Git LFS
- 构建产物：`node_modules/`、`dist/`、`build/`、`__pycache__/`、`*.pyc`、`.venv/`
- 临时文件：`.DS_Store`、`thumbs.db`、`*.swp`、`*.tmp`

**API 密钥验证：**
检查修改的文件中是否存在如下模式：
```bash
OPENAI_API_KEY=sk-proj-xxxxx  # ❌ 检测到真实密钥！
AWS_SECRET_KEY=AKIA...         # ❌ 检测到真实密钥！
STRIPE_API_KEY=sk_live_...    # ❌ 检测到真实密钥！

# ✅ 可接受的占位符：
API_KEY=your-api-key-here
SECRET_KEY=placeholder
TOKEN=xxx
API_KEY=<your-key>
SECRET=${YOUR_SECRET}
```

**✅ 验证：**
- `.gitignore` 已正确配置
- 无合并冲突
- 分支正确（如果在 main/master 上发出警告）
- API 密钥均为占位符

### 3. 请求确认

呈现摘要：
```
📊 更改摘要：
- 已修改 X 个文件，新增 Y 个，删除 Z 个
- 总计：+AAA 行插入，-BBB 行删除

🔒 安全：✅ 无密钥 | ✅ 无大文件 | ⚠️ [警告]
🌿 分支：[名称] → origin/[名称]

我将执行：git add . → 提交 → 推送

输入 'yes' 继续或 'no' 取消。
```

**等待用户明确回复"yes"后再继续。**

### 4. 执行（确认后）

按顺序运行：
```bash
git add .
git status  # 验证暂存区
```

### 5. 生成提交消息

分析更改并创建约定式提交：

**格式：**
```
[类型]: 简要摘要（最多 72 个字符）

- 关键更改 1
- 关键更改 2
- 关键更改 3
```

**类型：** `feat`、`fix`、`docs`、`style`、`refactor`、`test`、`chore`、`perf`、`build`、`ci`

**示例：**
```
docs: Update concept README files with comprehensive documentation

- Add architecture diagrams and tables
- Include practical examples
- Expand best practices sections
```

### 6. 提交并推送

```bash
git commit -m "$(cat <<'EOF'
[Generated commit message]
EOF
)"
git push  # 如果失败：git pull --rebase && git push
git log -1 --oneline --decorate  # 验证
```

### 7. 确认成功

```
✅ 已成功推送到远程！

提交：[哈希] [消息]
分支：[分支] → origin/[分支]
文件变更：X（+插入，-删除）
```

## 错误处理

- **git add 失败**：检查权限、锁定文件、验证仓库是否已初始化
- **git commit 失败**：修复 pre-commit 钩子，检查 git 配置（user.name/email）
- **git push 失败**：
  - 非快进：`git pull --rebase && git push`
  - 无远程分支：`git push -u origin [分支]`
  - 受保护分支：改用 PR 工作流程

## 适用场景

✅ **适合：**
- 多文件文档更新
- 含测试和文档的功能开发
- 跨文件的 bug 修复
- 项目级格式化/重构
- 配置变更

❌ **避免：**
- 不确定提交内容时
- 包含密钥/敏感数据时
- 未经验查的受保护分支
- 存在合并冲突时
- 需要细粒度提交历史时
- pre-commit 钩子失败时

## 替代方案

如果用户需要更多控制，建议：
1. **选择性暂存**：逐个审阅/暂存文件
2. **交互式暂存**：使用 `git add -p` 选择代码块
3. **PR 工作流**：创建分支 → 推送 → PR（使用 `/pr` 命令）

**⚠️ 提醒**：推送到远程前务必审阅更改。如有疑虑，使用单独的 git 命令获取更大控制权。
