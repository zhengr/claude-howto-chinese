<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# 为 Claude How To 贡献

感谢你对贡献本项目的兴趣！本指南将帮助你了解如何高效地进行贡献。

## 关于本项目

Claude How To 是一份可视化、示例驱动的 Claude Code 指南。我们提供：
- **Mermaid 图表**，解释功能的工作原理
- **可直接投入生产的模板**，你可以立即使用
- **真实世界的示例**，附带上下文和最佳实践
- **渐进式学习路径**，从入门到进阶

## 贡献类型

### 1. 新增示例或模板
为现有功能（斜杠命令、技能、钩子等）添加示例：
- 可直接复制粘贴的代码
- 工作原理的清晰解释
- 使用场景和好处
- 故障排查技巧

### 2. 文档改进
- 澄清令人困惑的章节
- 修复拼写和语法错误
- 补充缺失信息
- 改进代码示例

### 3. 功能指南
为新 Claude Code 功能创建指南：
- 分步教程
- 架构图
- 常见模式和反模式
- 真实工作流

### 4. Bug 报告
报告你遇到的问题：
- 描述你的预期
- 描述实际发生的情况
- 包含复现步骤
- 附上相关 Claude Code 版本和操作系统信息

### 5. 反馈与建议
帮助改进本指南：
- 提出更好的解释方案
- 指出覆盖范围的不足之处
- 推荐新的章节或重组建议

## 入门指南

### 1. Fork 和克隆

```bash
git clone https://github.com/luongnv89/claude-howto.git
cd claude-howto
```

### 2. 创建分支

使用描述性的分支名称：

```bash
git checkout -b add/feature-name
git checkout -b fix/issue-description
git checkout -b docs/improvement-area
```

### 3. 设置开发环境

每次提交前运行的 pre-commit 钩子会执行与 CI 相同的检查。PR 被接受前，所有四项检查必须通过。

**所需依赖：**

```bash
# Python 工具（uv 是本项目的包管理器）
pip install uv
uv venv
source .venv/bin/activate
uv pip install -r scripts/requirements-dev.txt

# Markdown 检查器（Node.js）
npm install -g markdownlint-cli

# Mermaid 图表验证器（Node.js）
npm install -g @mermaid-js/mermaid-cli

# 安装 pre-commit 并激活钩子
uv pip install pre-commit
pre-commit install
```

**验证你的环境：**

```bash
pre-commit run --all-files
```

每次提交时运行的钩子：

| Hook | 检查内容 |
|------|---------------|
| `markdown-lint` | Markdown 格式和结构 |
| `cross-references` | 相对链接、锚点、代码围栏 |
| `mermaid-syntax` | 所有 ` ```mermaid ` 块是否可正确解析 |
| `link-check` | 外部 URL 是否可访问 |
| `build-epub` | EPUB 生成无错误（在 `.md` 变更时） |

## 目录结构

```
├── 01-slash-commands/      # 用户调用的快捷方式
├── 02-memory/              # 持久化上下文示例
├── 03-skills/              # 可复用能力
├── 04-subagents/           # 专业 AI 助手
├── 05-mcp/                 # Model Context Protocol 示例
├── 06-hooks/               # 事件驱动的自动化
├── 07-plugins/             # 捆绑功能
├── 08-checkpoints/         # 会话快照
├── 09-advanced-features/   # 规划、思考、后台功能
├── 10-cli/                 # CLI 参考
├── scripts/                # 构建和工具脚本
└── README.md               # 主指南
```

## 如何贡献示例

### 添加斜杠命令

1. 在 `01-slash-commands/` 中创建 `.md` 文件
2. 包含以下内容：
   - 功能清晰描述
   - 使用场景
   - 安装说明
   - 使用示例
   - 自定义建议
3. 更新 `01-slash-commands/README.md`

### 添加技能

1. 在 `03-skills/` 中创建目录
2. 包含以下内容：
   - `SKILL.md` — 主文档
   - `scripts/` — 辅助脚本（如需）
   - `templates/` — 提示模板
   - README 中的使用示例
3. 更新 `03-skills/README.md`

### 添加子智能体

1. 在 `04-subagents/` 中创建 `.md` 文件
2. 包含以下内容：
   - 智能体用途和能力
   - 系统提示结构
   - 示例用例
   - 集成示例
3. 更新 `04-subagents/README.md`

### 添加 MCP 配置

1. 在 `05-mcp/` 中创建 `.json` 文件
2. 包含以下内容：
   - 配置说明
   - 所需环境变量
   - 安装说明
   - 使用示例
3. 更新 `05-mcp/README.md`

### 添加钩子

1. 在 `06-hooks/` 中创建 `.sh` 文件
2. 包含以下内容：
   - Shebang 和描述
   - 清晰注释，解释逻辑
   - 错误处理
   - 安全注意事项
3. 更新 `06-hooks/README.md`

## 写作指南

### Markdown 风格

- 使用清晰的标题（H2 用于章节，H3 用于子章节）
- 段落简短且聚焦
- 列表使用项目符号
- 代码块需指定语言
- 章节之间加空行

### 代码示例

- 示例可直接复制粘贴
- 对非明显的逻辑添加注释
- 同时包含简单版和进阶版
- 展示真实使用场景
- 指出潜在问题

### 文档

- 解释"为什么"而不仅是"是什么"
- 包含前置条件
- 添加故障排查章节
- 链接到相关主题
- 保持对新手友好

### JSON/YAML

- 缩进一致（统一使用 2 或 4 个空格）
- 添加注释解释配置
- 包含验证示例

### 图表

- 尽可能使用 Mermaid
- 图表简洁易读
- 图表下方添加描述
- 链接到相关章节

## 提交规范

遵循规范提交格式：

```
type(scope): description

[optional body]
```

类型：
- `feat`：新功能或示例
- `fix`：Bug 修复或更正
- `docs`：文档变更
- `refactor`：代码重构
- `style`：格式变更
- `test`：测试补充或变更
- `chore`：构建、依赖等

示例：

```
feat(slash-commands): Add API documentation generator
docs(memory): Improve personal preferences example
fix(README): Correct table of contents link
docs(skills): Add comprehensive code review skill
```

## 提交前检查

### 检查清单

- [ ] 代码遵循项目风格和约定
- [ ] 新示例包含清晰的文档
- [ ] README 文件已更新（本地和根目录）
- [ ] 无敏感信息（API 密钥、凭证）
- [ ] 示例已测试且能正常工作
- [ ] 链接已验证且正确
- [ ] 文件权限正确（脚本可执行）
- [ ] 提交信息清晰且描述准确

### 本地测试

```bash
# 运行所有 pre-commit 检查（与 CI 相同的检查）
pre-commit run --all-files

# 查看你的变更
git diff
```

## Pull Request 流程

1. **创建带有清晰描述的 PR**：
   - 这新增/修复了什么？
   - 为什么需要？
   - 相关问题（如有）

2. **包含相关细节**：
   - 新功能？包含使用场景
   - 文档？解释改进内容
   - 示例？展示前后对比

3. **链接到 issue**：
   - 使用 `Closes #123` 自动关闭相关 issue

4. **耐心等待审查**：
   - 维护者可能会提出改进建议
   - 根据反馈迭代
   - 最终决定权归维护者所有

## 代码审查流程

审查者将检查：
- **准确性**：是否按描述工作？
- **质量**：是否可投入生产？
- **一致性**：是否遵循项目模式？
- **文档**：是否清晰且完整？
- **安全性**：是否存在漏洞？

## 报告问题

### Bug 报告

包含：
- Claude Code 版本
- 操作系统
- 复现步骤
- 预期行为
- 实际行为
- 如适用，提供截图

### 功能建议

包含：
- 使用场景或待解决的问题
- 提出的解决方案
- 已考虑的替代方案
- 附加上下文

### 文档问题

包含：
- 令人困惑或缺失的内容
- 建议的改进方案
- 示例或参考资料

## 项目政策

### 敏感信息

- 永远不要提交 API 密钥、令牌或凭证
- 示例中使用占位值
- 配置文件包含 `.env.example`
- 记录所需的环境变量

### 代码质量

- 示例专注且易读
- 避免过度工程化
- 对非明显逻辑添加注释
- 提交前充分测试

### 知识产权

- 原创内容归作者所有
- 项目使用教育许可证
- 尊重现有版权
- 在需要时提供署名

## 获取帮助

- **问题**：在 GitHub Issues 中发起讨论
- **常规帮助**：查看现有文档
- **开发帮助**：查看类似示例
- **代码审查**：在 PR 中标记维护者

## 致谢

贡献者将在以下位置被认可：
- README.md 的 Contributors 区域
- GitHub 贡献者页面
- 提交历史

## 安全

在贡献示例和文档时，请遵循安全编码实践：

- **永远不要硬编码机密或 API 密钥** — 使用环境变量
- **警告安全影响** — 指出潜在风险
- **使用安全默认值** — 默认启用安全功能
- **验证输入** — 展示正确的输入验证和清理
- **包含安全说明** — 记录安全考虑

有关安全问题，请参阅 [SECURITY.md](SECURITY.md) 了解我们的漏洞报告流程。

## 行为准则

我们致力于提供包容和友好的社区。请阅读 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) 了解我们完整的社区标准。

简而言之：
- 尊重且包容
- 乐于接受反馈
- 帮助他人学习和成长
- 避免骚扰或歧视
- 向维护者报告问题

所有贡献者都应遵守本准则，并以善意和尊重对待彼此。

## 许可证

通过为本项目贡献内容，您同意您的贡献将在 MIT 许可证下授权。详见 [LICENSE](LICENSE) 文件。

## 有问题？

- 查看 [README](README.md)
- 查看 [LEARNING-ROADMAP.md](LEARNING-ROADMAP.md)
- 浏览现有示例
- 发起 issue 讨论

感谢你的贡献！ 🙏
