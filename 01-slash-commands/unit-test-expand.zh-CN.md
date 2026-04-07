---
name: Expand Unit Tests
description: Increase test coverage by targeting untested branches and edge cases
tags: testing, coverage, unit-tests
---

# 扩展单元测试

扩展现有单元测试，适配项目的测试框架：

1. **分析覆盖率**：运行覆盖率报告，识别未测试的分支、边界条件和低覆盖率区域
2. **找出缺口**：审查代码中的逻辑分支、错误路径、边界条件、空值/空输入
3. **编写测试**，使用项目对应的框架：
   - Jest/Vitest/Mocha（JavaScript/TypeScript）
   - pytest/unittest（Python）
   - Go testing/testify（Go）
   - Rust test framework（Rust）
4. **针对具体场景**：
   - 错误处理和异常
   - 边界值（最小/最大、空值、null）
   - 边缘情况和角落情况
   - 状态转换和副作用
5. **验证改进**：再次运行覆盖率，确认可测量的提升

仅输出新增测试的代码块。遵循现有测试模式和命名约定。
