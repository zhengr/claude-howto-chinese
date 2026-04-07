---
name: api-documentation-generator
description: Generate comprehensive, accurate API documentation from source code. Use when creating or updating API documentation, generating OpenAPI specs, or when users mention API docs, endpoints, or documentation.
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

### 说明
此端点功能的简要解释

### 参数

| 名称 | 类型 | 必选 | 描述 |
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
