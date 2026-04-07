---
description: Analyze code for performance issues and suggest optimizations
---

# 代码优化

按优先级检查以下问题：

1. **性能瓶颈** - 识别 O(n²) 操作、低效循环
2. **内存泄漏** - 查找未释放的资源、循环引用
3. **算法改进** - 建议更好的算法或数据结构
4. **缓存机会** - 识别可缓存的重复计算
5. **并发问题** - 查找竞态条件或线程问题

响应格式：
- 问题严重程度（Critical/High/Medium/Low）
- 代码位置
- 解释说明
- 推荐修复方案（含代码示例）
