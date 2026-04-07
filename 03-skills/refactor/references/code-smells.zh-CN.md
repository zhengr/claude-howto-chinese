# 代码异味目录

一份基于 Martin Fowler《重构》（第 2 版）的综合代码异味参考。代码异味是深层问题的症状——它们表明代码设计可能存在问题。

> "代码异味通常是系统中深层问题的一种表面指示。" — Martin Fowler

---

## 膨胀

表示代码已经增长到无法有效处理的代码异味。

### 长方法

**表现：**
- 方法超过 30-50 行
- 需要滚动才能看到整个方法
- 多层嵌套
- 用注释来解释各个代码段做什么

**为什么不好：**
- 难以理解
- 难以隔离测试
- 更改会产生意外的后果
- 重复逻辑隐藏其中

**重构方法：**
- 提取函数
- 用查询取代临时变量
- 引入参数对象
- 用函数对象取代方法
- 分解条件表达式

**示例（重构前）：**
```javascript
function processOrder(order) {
  // 验证订单 (20 行)
  if (!order.items) throw new Error('没有商品');
  if (order.items.length === 0) throw new Error('空订单');
  // ... 更多验证

  // 计算总计 (30 行)
  let subtotal = 0;
  for (const item of order.items) {
    subtotal += item.price * item.quantity;
  }
  // ... 税费、运费、折扣

  // 发送通知 (20 行)
  // ... 邮件逻辑
}
```

**示例（重构后）：**
```javascript
function processOrder(order) {
  validateOrder(order);
  const totals = calculateOrderTotals(order);
  sendOrderNotifications(order, totals);
  return { order, totals };
}
```

---

### 大类

**表现：**
- 类有很多实例变量（>7-10 个）
- 类有很多方法（>15-20 个）
- 类名含糊不清（Manager、Handler、Processor）
- 方法没有使用所有实例变量

**为什么不好：**
- 违反单一职责原则
- 难以测试
- 更改会波及到不相关的功能
- 难以服用各个部分

**重构方法：**
- 提取类
- 提取子类
- 提取接口

**检测方法：**
```
代码行数 > 300
方法数量 > 15
字段数量 > 10
```

---

### 基本类型偏执

**表现：**
- 对领域概念使用基本类型（用字符串表示 email、用整数表示金额）
- 使用基本类型数组而不是对象
- 用字符串常量表示类型码
- 魔术数字/字符串

**为什么不好：**
- 类型级别没有验证
- 逻辑分散在代码库中
- 容易传递错误的值
- 缺少领域概念

**重构方法：**
- 以对象取代基本类型
- 以类取代类型码
- 以子类取代类型码
- 以状态/策略取代类型码

**示例（重构前）：**
```javascript
const user = {
  email: 'john@example.com',     // 只是个字符串
  phone: '1234567890',           // 只是个字符串
  status: 'active',              // 魔术字符串
  balance: 10050                 // 以分为单位的整数
};
```

**示例（重构后）：**
```javascript
const user = {
  email: new Email('john@example.com'),
  phone: new PhoneNumber('1234567890'),
  status: UserStatus.ACTIVE,
  balance: Money.cents(10050)
};
```

---

### 长参数列表

**表现：**
- 方法有 4 个以上参数
- 总是同时出现的参数
- 改变方法行为的布尔标记
- 经常传入 null/undefined

**为什么不好：**
- 难以正确调用
 参数顺序容易混淆
- 表明方法做了太多事情
- 难以添加新参数

**重构方法：**
- 引入参数对象
- 保持对象完整
- 以方法调用取代参数
- 移除标记参数

**示例（重构前）：**
```javascript
function createUser(firstName, lastName, email, phone,
                    street, city, state, zip,
                    isAdmin, isActive, createdBy) {
  // ...
}
```

**示例（重构后）：**
```javascript
function createUser(personalInfo, address, options) {
  // personalInfo: { firstName, lastName, email, phone }
  // address: { street, city, state, zip }
  // options: { isAdmin, isActive, createdBy }
}
```

---

### 数据泥团

**表现：**
- 3 个以上相同字段总是同时出现
- 总是结伴而行的参数
- 类中的字段子集属于一起

**为什么不好：**
- 重复的处理逻辑
- 缺少抽象
- 更难扩展
- 表明有隐藏的类

**重构方法：**
- 提取类
- 引入参数对象
- 保持对象完整

**示例：**
```javascript
// 数据泥团：三维坐标 (x, y, z)
function movePoint(x, y, z, dx, dy, dz) { }
function scalePoint(x, y, z, factor) { }
function distanceBetween(x1, y1, z1, x2, y2, z2) { }

// 提取 Point3D 类
class Point3D {
  constructor(x, y, z) { }
  move(delta) { }
  scale(factor) { }
  distanceTo(other) { }
}
```

---

## 面向对象滥用者

表示没有完整或正确使用 OOP 原则的异味。

### 条件表达式

**表现：**
- 长 switch/case 或 if/else 链
- 相同条件在多处出现
- 根据类型码做条件分支
- 添加新情况需要在各处修改

**为什么不好：**
- 违反开闭原则
- 更改会波及所有 switch 位置
- 难以扩展
- 通常表明缺少多态

**重构方法：**
- 以多态取代条件表达式
- 以子类取代类型码
- 以状态/策略取代类型码

**示例（重构前）：**
```javascript
function calculatePay(employee) {
  switch (employee.type) {
    case 'hourly':
      return employee.hours * employee.rate;
    case 'salaried':
      return employee.salary / 12;
    case 'commissioned':
      return employee.sales * employee.commission;
  }
}
```

**示例（重构后）：**
```javascript
class HourlyEmployee {
  calculatePay() {
    return this.hours * this.rate;
  }
}

class SalariedEmployee {
  calculatePay() {
    return this.salary / 12;
  }
}
```

---

### 临时字段

**表现：**
- 实例变量只在某些方法中使用
- 有条件地设置字段
- 特定情况下需要复杂的初始化

**为什么不好：**
- 容易困惑——字段存在但可能为 null
- 难以理解对象状态
- 表明隐藏了条件逻辑

**重构方法：**
- 提取类
- 引入空对象
- 以局部变量取代临时字段

---

### 被拒绝的遗赠

**表现：**
- 子类没有使用继承的方法/数据
- 子类将其覆写为不做事
- 继承用于代码复用，而非 IS-A 关系

**为什么不好：**
- 错误的抽象
- 违反里氏替换原则
- 误导性的继承体系

**重构方法：**
- 方法/字段下移
- 以委托取代子类
- 以委托取代继承

---

### 接口不同的等效类

**表现：**
- 两个做类似事情的类
- 同一概念使用了不同的方法名
- 可以互换使用

**为什么不好：**
- 重复的实现
- 没有公共接口
- 难以切换

**重构方法：**
- 方法改名
- 方法移动
- 提取超类
- 提取接口

---

## 变更阻止者

使变更变得困难的异味——改一件事需要改许多事。

### 发散式变化

**表现：**
- 一个类因多种不同的原因被修改
- 不同领域的更改都触发同一个类做变更
- 类变成了"上帝类"

**为什么不好：**
- 违反单一职责
- 高变更频率
- 合并冲突

**重构方法：**
- 提取类
- 提取超类
- 提取子类

**示例：**
一个 `User` 类因以下原因被修改：
- 认证更改
- 个人资料更改
- 计费更改
- 通知更改

→ 提取为：`AuthService`、`ProfileService`、`BillingService`、`NotificationService`

---

### 霰弹式修改

**表现：**
- 一处修改需要在许多类中编辑
- 添加一个小功能需要触碰 10+ 个文件
- 更改分散在各处，很难找到所有修改点

**为什么不好：**
- 容易遗漏
- 高耦合
- 更改容易出错

**重构方法：**
- 方法移动
- 字段移动
- 嵌入式类

**检测方法：**
寻找：添加一个字段需要改动 >5 个文件。

---

### 平行继承体系

**表现：**
- 在一个继承体系创建子时，需要在另一个也创建子类
- 类前缀匹配（例如 `DatabaseOrder`、`DatabaseProduct`）

**为什么不好：**
- 双倍的维护成本
- 继承体系间的耦合
- 容易遗漏一边

**重构方法：**
- 方法移动
- 字段移动
- 消除一个继承体系

---

## 可消除物

本来不应该存在的东西。

### 注释（过度）

**表现：**
- 注释解释代码做什么
- 被注释掉的代码
- TODO/FIXME 永远存在
- 注释中表示歉意

**为什么不好：**
- 注释会撒谎（与代码不同步）
- 代码应该是自解释的
- 死代码会使人困惑

**重构方法：**
- 提取函数（用名称解释意图）
- 重命名（不用注释就清晰）
- 移除被注释掉的代码
- 引入断言

**好注释 vs 差注释：**
```javascript
// 差：解释做什么
// 遍历用户并检查是否活跃
for (const user of users) {
  if (user.status === 'active') { }
}

// 好：解释为什么
// 仅活跃用户 - 不活跃用户由清理任务处理
const activeUsers = users.filter(u => u.isActive);
```

---

### 重复代码

**表现：**
- 相同的代码出现在多处
- 类似代码只有小的差异
- 复制粘贴模式

**为什么不好：**
- Bug 修复需要在多处进行
- 不一致性风险
- 代码库膨胀

**重构方法：**
- 提取函数
- 提取类
- 方法上移（在继承体系中）
- 形成模板方法

**检测规则：**
任何重复 3 次以上的代码应被提取。

---

### 懒惰类

**表现：**
- 类的功能不足以证明其存在的合理性
- 没有增加任何价值的包装类
- 过度工程化的结果

**为什么不好：**
- 维护成本
- 不必要的间接层
- 没有收益的复杂性

**重构方法：**
- 嵌入式类
- 折叠继承体系

---

### 死代码

**表现：**
- 不可达的代码
- 未使用的变量/方法/类
- 被注释掉的代码
- 在不可能条件下的代码

**为什么不好：**
- 使人困惑
- 维护负担
- 减缓理解速度

**重构方法：**
- 移除死代码
- 安全删除

**检测方法：**
```bash
# 查找未使用的导出
# 查找未引用的函数
# IDE 的"未使用"警告
```

---

### 过度泛化

**表现：**
- 只有一个子类的抽象类
- "未来使用"的未使用参数
- 只做委托的方法
- 给一个用例做的"框架"

**为什么不好：**
- 没有收益的复杂性
- YAGNI（你不会需要它）
- 更难理解

**重构方法：**
- 折叠继承体系
- 嵌入式类
- 移除参数
- 方法改名

---

## 耦合者

表示类之间过度耦合的异味。

### 特性依恋

**表现：**
- 方法使用来自另一个类的数据比自己的还多
- 大量调用另一个对象的 getter
- 数据与行为被分离

**为什么不好：**
- 行为放错了位置
- 封装不良
- 难以维护

**重构方法：**
- 方法移动
- 字段移动
- 提取函数（然后移动）

**示例（重构前）：**
```javascript
class Order {
  getDiscountedPrice(customer) {
    // 大量使用 customer 数据
    if (customer.loyaltyYears > 5) {
      return this.price * customer.discountRate;
    }
    return this.price;
  }
}
```

**示例（重构后）：**
```javascript
class Customer {
  getDiscountedPriceFor(price) {
    if (this.loyaltyYears > 5) {
      return price * this.discountRate;
    }
    return price;
  }
}
```

---

### 不恰当的亲昵关系

**表现：**
- 类之间互相访问对方的私有部分
- 双向引用
- 子类对父级了解太多

**为什么不好：**
- 高耦合
- 更改会级联
- 难以单独修改其中之一

**重构方法：**
- 方法移动
- 字段移动
- 将双向改为单向
- 提取类
- 隐藏委托

---

### 消息链

**表现：**
- 长的方法调用链：`a.getB().getC().getD().getValue()`
- 客户端依赖导航结构
- "火车残骸"式的代码

**为什么不好：**
- 脆弱——任何更改都会破坏链
- 违反迪米特法则
- 耦合到结构中

**重构方法：**
- 隐藏委托
- 提取函数
- 方法移动

**示例：**
```javascript
// 差：消息链
const managerName = employee.getDepartment().getManager().getName();

// 更好：隐藏委托
const managerName = employee.getManagerName();
```

---

### 中间人

**表现：**
- 只做委托的类
- 一半的方法都是委托
- 没有增加价值

**为什么不好：**
- 不必要的间接层
- 维护成本
- 架构混乱

**重构方法：**
- 移除中间人
- 内联方法

---

## 异味严重性指南

| 严重性 | 描述 | 行动 |
|----------|-------------|--------|
| **关键** | 阻塞开发，导致 Bug | 立即修复 |
| **高** | 重大维护成本 | 在当前冲刺期间修复 |
| **中** | 明显但可管理 | 安排在不久的将来修复 |
| **低** | 轻微不便 | 适时修复 |

---

## 快速检测清单

在扫描代码时使用此清单：

- [ ] 是否有超过 30 行的方法？
- [ ] 是否有超过 300 行的类？
- [ ] 是否有超过 4 个参数的方法？
- [ ] 是否有重复的代码块？
- [ ] 是否有基于类型码的 switch/case？
- [ ] 是否有未使用的代码？
- [ ] 是否有方法大量使用另一个类的数据？
- [ ] 是否有长的调用链？
- [ ] 是否有解释"做什么"而非"为什么"的注释？
- [ ] 是否有应该是对象的基本类型？

---

## 延伸阅读

- Fowler, M. (2018). 《重构：改善既有代码的设计》（第 2 版）
- Kerievsky, J. (2004). 《重构与模式》
- Feathers, M. (2004). 《修改代码的艺术》
