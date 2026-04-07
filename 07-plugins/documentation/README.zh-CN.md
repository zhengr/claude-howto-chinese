<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../../resources/logos/claude-howto-logo.svg">
</picture>

# 文档生成插件

为项目生成和维护全面的文档。

## 功能

✅ API 文档生成
✅ README 创建和更新
✅ 文档同步
✅ 代码注释改进
✅ 示例生成

## 安装

```bash
/plugin install documentation
```

## 包含内容

### 斜杠命令（Slash Commands）
- `/generate-api-docs` - 生成 API 文档
- `/generate-readme` - 创建或更新 README
- `/sync-docs` - 将文档与代码变更同步
- `/validate-docs` - 验证文档

### 子代理（Subagents）
- `api-documenter` - API 文档专家
- `code-commentator` - 代码注释改进
- `example-generator` - 代码示例创建

### 模板（Templates）
- `api-endpoint.md` - API 端点文档模板
- `function-docs.md` - 函数文档模板
- `adr-template.md` - 架构决策记录（ADR）模板

### MCP 服务器
- 用于文档同步的 GitHub 集成

## 使用方法

### 生成 API 文档
```
/generate-api-docs
```

### 创建 README
```
/generate-readme
```

### 同步文档
```
/sync-docs
```

### 验证文档
```
/validate-docs
```

## 要求

- Claude Code 1.0+
- GitHub 访问权限（可选）

## 工作流示例

```
User: /generate-api-docs

Claude:
1. 扫描 /src/api/ 下的所有 API 端点
2. 委托给 api-documenter 子代理
3. 提取函数签名和 JSDoc
4. 按模块/端点组织
5. 使用 api-endpoint.md 模板
6. 生成全面的 Markdown 文档
7. 包含 curl、JavaScript 和 Python 示例

Result:
✅ API 文档已生成
📄 已创建文件：
   - docs/api/users.md
   - docs/api/auth.md
   - docs/api/products.md
📊 覆盖率：23/23 端点已记录
```

## 模板使用

### API 端点模板
用于记录包含完整示例的 REST API 端点。

### 函数文档模板
用于记录单独的函数/方法。

### ADR 模板
用于记录架构决策。

## 配置

设置 GitHub Token 用于文档同步：
```bash
export GITHUB_TOKEN="your_github_token"
```

## 最佳实践

- 将文档放在靠近代码的位置
- 随代码变更同步更新文档
- 包含实用示例
- 定期验证
- 使用模板保持一致性
