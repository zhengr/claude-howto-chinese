# 重构目录

一份精选自 Martin Fowler《重构》（第 2 版）的重构技术目录。每个重构包括动机、逐步骤操作和示例。

> "重构由其操作方法定义——即你遵循的精确步骤序列。" — Martin Fowler

---

## 如何使用此目录

1. 使用代码异味参考资料**识别异味**
2. 在此目录中**查找匹配的重构**
3. 逐步骤**遵循操作方法**
4. **每步之后都要测试**以确保行为不变

**黄金法则**：如果任何一步骤耗时超过 10 分钟，就将其分解为更小的步骤。

---

## 最常用的重构

### 提取函数

**适用场景**：长方法、重复代码、需要为一个概念命名

**动机**：将一个代码段变成一个方法，用名称说明其用途。

**操作方法**：
1. 创建一个新以方法，用其用途（而不是做法）命名
2. 将代码段复制到新方法中
3. 扫描代码段中使用的局部变量
4. 将局部变量作为参数传递（或在方法中声明）
5. 适当处理返回值
6. 用对新方法的调用取代原始的代码段
7. 测试

**重构前**：
```javascript
function printOwing(invoice) {
  let outstanding = 0;

  console.log("***********************");
  console.log("**** Customer Owes ****");
  console.log("***********************");

  // 计算未付款
  for (const order of invoice.orders) {
    outstanding += order.amount;
  }

  // 打印详情
  console.log(`name: ${invoice.customer}`);
  console.log(`amount: ${outstanding}`);
}
```

**重构后**：
```javascript
function printOwing(invoice) {
  printBanner();
  const outstanding = calculateOutstanding(invoice);
  printDetails(invoice, outstanding);
}

function printBanner() {
  console.log("***********************");
  console.log("**** Customer Owes ****");
  console.log("***********************");
}

function calculateOutstanding(invoice) {
  return invoice.orders.reduce((sum, order) => sum + order.amount, 0);
}

function printDetails(invoice, outstanding) {
  console.log(`name: ${invoice.customer}`);
  console.log(`amount: ${outstanding}`);
}
```

---

### 内联函数

**适用场景**：方法体与其名一样清晰、过度委托

**动机**：当方法没有增加价值时，消除不必要的间接层。

**操作方法**：
1. 检查方法不是多态的
2. 找到该方法的所有调用
3. 用方法体替换每个调用
4. 每次替换后测试
5. 删除方法定义

**重构前**：
```javascript
function getRating(driver) {
  return moreThanFiveLateDeliveries(driver) ? 2 : 1;
}

function moreThanFiveLateDeliveries(driver) {
  return driver.numberOfLateDeliveries > 5;
}
```

**重构后**：
```javascript
function getRating(driver) {
  return driver.numberOfLateDeliveries > 5 ? 2 : 1;
}
```

---

### 提取变量

**适用场景**：难以理解的复杂表达式

**动机**：为复杂表达式的某部分命名。

**操作方法**：
1. 确保该表达式没有副作用
2. 声明一个不可变变量
3. 将其设为表达式的结果（或部分）
4. 用该变量取代原始表达式
5. 测试

**重构前**：
```javascript
return order.quantity * order.itemPrice -
  Math.max(0, order.quantity - 500) * order.itemPrice * 0.05 +
  Math.min(order.quantity * order.itemPrice * 0.1, 100);
```

**重构后**：
```javascript
const basePrice = order.quantity * order.itemPrice;
const quantityDiscount = Math.max(0, order.quantity - 500) * order.itemPrice * 0.05;
const shipping = Math.min(basePrice * 0.1, 100);
return basePrice - quantityDiscount + shipping;
```

---

### 内联变量

**适用场景**：变量名表达的意思不如表达式本身

**动机**：移除不必要的间接层。

**操作方法**：
1. 检查右边没有副作用
2. 如果变量不可变，使其不可变然后测试
3. 找到第一处引用并用表达式替换
4. 测试
5. 对所有引用重复以上步骤
6. 删除声明和赋值
7. 测试

---

### 变量更名

**适用场景**：名称不能清楚表达其用途

**动机**：好名称对干净的代码至关重要。

**操作方法**：
1. 如果变量使用广泛，考虑封装
2. 找到所有引用
3. 修改每个引用
4. 测试

**提示**：
- 使用表达意图的名称
- 避免缩写
- 使用领域术语

```javascript
// 差
const d = 30;
const x = users.filter(u => u.a);

// 好
const daysSinceLastLogin = 30;
const activeUsers = users.filter(user => user.isActive);
```

---

### 函数改名

**适用场景**：函数名不能解释其用途、参数需要变更

**动机**：好的函数名使代码自解释。

**操作方法（简单）**：
1. 移除不需要的参数
2. 更改名称
3. 添加需要的参数
4. 测试

**操作方法（迁移 - 适用于复杂变更）**：
1. 如果要移除参数，确保没有使用
2. 用期望的声明创建一个新函数
3. 让旧函数调用新函数
4. 测试
5. 修改调用代码以使用新函数
6. 每次修改后测试
7. 删除旧函数

**重构前**：
```javascript
function circum(radius) {
  return 2 * Math.PI * radius;
}
```

**重构后**：
```javascript
function circumference(radius) {
  return 2 * Math.PI * radius;
}
```

---

### 封装变量

**适用场景**：从多处直接访问数据

**动机**：为数据操作提供清晰的访问点。

**操作方法**：
1. 创建 getter 和 setter 函数
2. 找到所有引用
3. 用 getter 替换读操作
4. 用 setter 替换写操作
5. 每次修改后测试
6. 限制变量的可见性

**重构前**：
```javascript
let defaultOwner = { firstName: "Martin", lastName: "Fowler" };

// 在多处使用
spaceship.owner = defaultOwner;
```

**重构后**：
```javascript
let defaultOwnerData = { firstName: "Martin", lastName: "Fowler" };

function defaultOwner() { return defaultOwnerData; }
function setDefaultOwner(arg) { defaultOwnerData = arg; }

spaceship.owner = defaultOwner();
```

---

### 引入参数对象

**适用场景**：几个参数经常同时出现

**动机**：将自然属于一组的数据聚合在一起。

**操作方法**：
1. 为被分组的参数创建一个新类/结构体
2. 测试
3. 使用"函数改名"来添加新对象
4. 测试
5. 对组中的每个参数，从函数中移除并使用新对象
6. 每次操作后测试

**重构前**：
```javascript
function amountInvoiced(startDate, endDate) { ... }
function amountReceived(startDate, endDate) { ... }
function amountOverdue(startDate, endDate) { ... }
```

**重构后**：
```javascript
class DateRange {
  constructor(start, end) {
    this.start = start;
    this.end = end;
  }
}

function amountInvoiced(dateRange) { ... }
function amountReceived(dateRange) { ... }
function amountOverdue(dateRange) { ... }
```

---

### 将函数组合到类中

**适用场景**：几个函数都对同一数据进行操作

**动机**：将函数与其操作的数据组合在一起。

**操作方法**：
1. 对共同数据应用"封装记录"
2. 将每个函数移入类中
3. 每次移入后测试
4. 将数据参数替换为类字段的使用

**重构前**：
```javascript
function base(reading) { ... }
function taxableCharge(reading) { ... }
function calculateBaseCharge(reading) { ... }
```

**重构后**：
```javascript
class Reading {
  constructor(data) { this._data = data; }

  get base() { ... }
  get taxableCharge() { ... }
  get calculateBaseCharge() { ... }
}
```

---

### 拆分阶段

**适用场景**：代码处理了两件不同的事情

**动机**：将代码分离成具有清晰边界的不同阶段。

**操作方法**：
1. 为第二阶段创建一个第二函数
2. 测试
3. 在阶段之间引入一个中间数据结构
4. 测试
5. 将第一阶段提取为其自己的函数
6. 测试

**重构前**：
```javascript
function priceOrder(product, quantity, shippingMethod) {
  const basePrice = product.basePrice * quantity;
  const discount = Math.max(quantity - product.discountThreshold, 0)
    * product.basePrice * product.discountRate;
  const shippingPerCase = (basePrice > shippingMethod.discountThreshold)
    ? shippingMethod.discountedFee : shippingMethod.feePerCase;
  const shippingCost = quantity * shippingPerCase;
  return basePrice - discount + shippingCost;
}
```

**重构后**：
```javascript
function priceOrder(product, quantity, shippingMethod) {
  const priceData = calculatePricingData(product, quantity);
  return applyShipping(priceData, shippingMethod);
}

function calculatePricingData(product, quantity) {
  const basePrice = product.basePrice * quantity;
  const discount = Math.max(quantity - product.discountThreshold, 0)
    * product.basePrice * product.discountRate;
  return { basePrice, quantity, discount };
}

function applyShipping(priceData, shippingMethod) {
  const shippingPerCase = (priceData.basePrice > shippingMethod.discountThreshold)
    ? shippingMethod.discountedFee : shippingMethod.feePerCase;
  const shippingCost = priceData.quantity * shippingPerCase;
  return priceData.basePrice - priceData.discount + shippingCost;
}
```

---

## 搬移特性

### 方法移动

**适用场景**：方法使用另一个类的特性超过自己的类

**动机**：将函数与它最多使用的数据放在一起。

**操作方法**：
1. 检查方法在其类中使用的所有程序元素
2. 检查方法是否为多态
3. 将方法复制到目标类
4. 调整目标类的上下文
5. 让原始方法委托给目标
6. 测试
7. 考虑移除原始方法

---

### 字段移动

**适用场景**：字段被另一个类使用得更多

**动机**：让数据与使用它的函数放在一起。

**操作方法**：
1. 如果尚未封装，先进行封装
2. 测试
3. 在目标中创建字段
4. 更新引用以使用目标字段
5. 测试
6. 移除原始字段

---

### 将函数体移入调用者

**适用场景**：总是有相同的代码出现在函数调用处

**动机**：通过将重复代码移入函数来消除重复。

**操作方法**：
1. 如果尚未提取，将重复代码提取到函数中
2. 将语句移入该函数
3. 测试
4. 如果调用者不再需要独立的语句，将其移除

---

### 将语句移到调用者处

**适用场景**：通用行为在调用者之间有所不同

**动机**：当行为需要有所不同时，将其移出函数。

**操作方法**：
1. 对要移出的代码使用"提取函数"
2. 对原始函数使用"内联函数"
3. 移除现在被内联的调用
4. 将提取的代码移到各个调用者处
5. 测试

---

## 组织数据

### 以对象取代基本类型

**适用场景**：数据项需要比简单值更多的行为

**动机**：将数据与其行为封装在一起。

**操作方法**：
1. 应用"封装变量"
2. 创建一个简单的值对象
3. 修改 setter 以创建新实例
4. 修改 getter 以返回值
5. 测试
6. 为新类添加更丰富的行为

**重构前**：
```javascript
class Order {
  constructor(data) {
    this.priority = data.priority; // 字符串："high"、"rush" 等
  }
}

// 用法
if (order.priority === "high" || order.priority === "rush") { ... }
```

**重构后**：
```javascript
class Priority {
  constructor(value) {
    if (!Priority.legalValues().includes(value))
      throw new Error(`无效优先级：${value}`);
    this._value = value;
  }

  static legalValues() { return ['low', 'normal', 'high', 'rush']; }
  get value() { return this._value; }

  higherThan(other) {
    return Priority.legalValues().indexOf(this._value) >
           Priority.legalValues().indexOf(other._value);
  }
}

// 用法
if (order.priority.higherThan(new Priority("normal"))) { ... }
```

---

### 用查询取代临时变量

**适用场景**：临时变量持有表达式的结果

**动机**：通过将表达式提取到函数中使代码更清晰。

**操作方法**：
1. 检查变量是否只被赋一次值
2. 将赋值的右边提取为方法
3. 用该方法的调用替换对临时变量的引用
4. 测试
5. 移除临时变量的声明和赋值

**重构前**：
```javascript
const basePrice = this._quantity * this._itemPrice;
if (basePrice > 1000) {
  return basePrice * 0.95;
} else {
  return basePrice * 0.98;
}
```

**重构后**：
```javascript
get basePrice() {
  return this._quantity * this._itemPrice;
}

// 在方法中
if (this.basePrice > 1000) {
  return this.basePrice * 0.95;
} else {
  return this.basePrice * 0.98;
}
```

---

## 简化条件逻辑

### 分解条件表达式

**适用场景**：复杂的条件（if-then-else）语句

**动机**：通过提取条件动作为函数，使其意图清晰。

**操作方法**：
1. 对条件应用"提取函数"
2. 对 then 分支应用"提取函数"
3. 对 else 分支应用"提取函数"（如果存在）

**重构前**：
```javascript
if (!aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd)) {
  charge = quantity * plan.summerRate;
} else {
  charge = quantity * plan.regularRate + plan.regularServiceCharge;
}
```

**重构后**：
```javascript
if (isSummer(aDate, plan)) {
  charge = summerCharge(quantity, plan);
} else {
  charge = regularCharge(quantity, plan);
}

function isSummer(date, plan) {
  return !date.isBefore(plan.summerStart) && !date.isAfter(plan.summerEnd);
}

function summerCharge(quantity, plan) {
  return quantity * plan.summerRate;
}

function regularCharge(quantity, plan) {
  return quantity * plan.regularRate + plan.regularServiceCharge;
}
```

---

### 合并条件表达式

**适用场景**：多个条件返回相同的结果

**动机**：清楚地表明条件是单一的判断。

**操作方法**：
1. 验证条件内没有副作用
2. 使用 `and` 或 `or` 合并条件
3. 考虑对组合的条件做"提取函数"

**重构前**：
```javascript
if (employee.seniority < 2) return 0;
if (employee.monthsDisabled > 12) return 0;
if (employee.isPartTime) return 0;
```

**重构后**：
```javascript
if (isNotEligibleForDisability(employee)) return 0;

function isNotEligibleForDisability(employee) {
  return employee.seniority < 2 ||
         employee.monthsDisabled > 12 ||
         employee.isPartTime;
}
```

---

### 以卫语句取代嵌套条件表达式

**适用场景**：深层嵌套的条件表达式使流程更难理解

**动机**：使用卫语句处理特殊情况，保持正常流程清晰。

**操作方法**：
1. 找到特殊情况条件
2. 使用提前返回的卫语句替换它们
3. 每次更改后测试

**重构前**：
```javascript
function payAmount(employee) {
  let result;
  if (employee.isSeparated) {
    result = { amount: 0, reasonCode: "SEP" };
  } else {
    if (employee.isRetired) {
      result = { amount: 0, reasonCode: "RET" };
    } else {
      result = calculateNormalPay(employee);
    }
  }
  return result;
}
```

**重构后**：
```javascript
function payAmount(employee) {
  if (employee.isSeparated) return { amount: 0, reasonCode: "SEP" };
  if (employee.isRetired) return { amount: 0, reasonCode: "RET" };
  return calculateNormalPay(employee);
}
```

---

### 以多态取代条件表达式

**适用场景**：基于类型的 switch/case、根据类型变化的条件逻辑

**动机**：让对象自己处理自己的行为。

**操作方法**：
1. 创建类继承体系（如果不存在）
2. 使用工厂函数进行对象创建
3. 将条件逻辑移入超类方法
4. 为每种情况创建子类方法
5. 移除原始的条件判断

**重构前**：
```javascript
function plumages(birds) {
  return birds.map(b => plumage(b));
}

function plumage(bird) {
  switch (bird.type) {
    case 'EuropeanSwallow':
      return "average";
    case 'AfricanSwallow':
      return (bird.numberOfCoconuts > 2) ? "tired" : "average";
    case 'NorwegianBlueParrot':
      return (bird.voltage > 100) ? "scorched" : "beautiful";
    default:
      return "unknown";
  }
}
```

**重构后**：
```javascript
class Bird {
  get plumage() { return "unknown"; }
}

class EuropeanSwallow extends Bird {
  get plumage() { return "average"; }
}

class AfricanSwallow extends Bird {
  get plumage() {
    return (this.numberOfCoconuts > 2) ? "tired" : "average";
  }
}

class NorwegianBlueParrot extends Bird {
  get plumage() {
    return (this.voltage > 100) ? "scorched" : "beautiful";
  }
}

function createBird(data) {
  switch (data.type) {
    case 'EuropeanSwallow': return new EuropeanSwallow(data);
    case 'AfricanSwallow': return new AfricanSwallow(data);
    case 'NorwegianBlueParrot': return new NorwegianBlueParrot(data);
    default: return new Bird(data);
  }
}
```

---

### 引入特例（空对象）

**适用场景**：对特殊情况重复检查 null

**动机**：返回一个处理特殊情况的特殊对象。

**操作方法**：
1. 创建具有预期接口的特殊情况类
2. 添加 isSpecialCase 检查
3. 引入工厂方法
4. 用特殊情况对象替换 null 检查
5. 测试

**重构前**：
```javascript
const customer = site.customer;
// ... 许多检查 customer == "unknown" 的地方
if (customer === "unknown") {
  customerName = "occupant";
} else {
  customerName = customer.name;
}
```

**重构后**：
```javascript
class UnknownCustomer {
  get name() { return "occupant"; }
  get billingPlan() { return registry.defaultPlan; }
}

// 工厂方法
function customer(site) {
  return site.customer === "unknown"
    ? new UnknownCustomer()
    : site.customer;
}

// 用法 - 不需要做空检查
const customerName = customer.name;
```

---

## 重构 API

### 将查询从修改中分离

**适用场景**：函数同时返回一个值且有副作用

**动机**：明确哪些操作有副作用。

**操作方法**：
1. 创建一个新的查询函数
2. 复制原始函数的返回逻辑
3. 修改原始函数返回 void
4. 替换返回值的函数调用
5. 测试

**重构前**：
```javascript
function alertForMiscreant(people) {
  for (const p of people) {
    if (p === "Don") {
      setOffAlarms();
      return "Don";
    }
    if (p === "John") {
      setOffAlarms();
      return "John";
    }
  }
  return "";
}
```

**重构后**：
```javascript
function findMiscreant(people) {
  for (const p of people) {
    if (p === "Don") return "Don";
    if (p === "John") return "John";
  }
  return "";
}

function alertForMiscreant(people) {
  if (findMiscreant(people) !== "") setOffAlarms();
}
```

---

### 函数参数化

**适用场景**：几个函数做着类似的事情但使用不同的值

**动机**：通过增加参数消除重复。

**操作方法**：
1. 选择一个函数
2. 为变化的字面量添加参数
3. 修改函数体以使用该参数
4. 测试
5. 修改调用代码以使用参数化版本
6. 移除不再需要的函数

**重构前**：
```javascript
function tenPercentRaise(person) {
  person.salary = person.salary * 1.10;
}

function fivePercentRaise(person) {
  person.salary = person.salary * 1.05;
}
```

**重构后**：
```javascript
function raise(person, factor) {
  person.salary = person.salary * (1 + factor);
}

// 用法
raise(person, 0.10);
raise(person, 0.05);
```

---

### 移除标记参数

**适用场景**：改变函数行为的布尔参数

**动机**：通过独立的函数使行为明确。

**操作方法**：
1. 为每个标记值创建一个独立的函数
2. 将每个调用替换为新的相应函数
3. 每次更改后测试
4. 移除原始函数

**重构前**：
```javascript
function bookConcert(customer, isPremium) {
  if (isPremium) {
    // 高级预订逻辑
  } else {
    // 普通预订逻辑
  }
}

bookConcert(customer, true);
bookConcert(customer, false);
```

**重构后**：
```javascript
function bookPremiumConcert(customer) {
  // 高级预订逻辑
}

function bookRegularConcert(customer) {
  // 普通预订逻辑
}

bookPremiumConcert(customer);
bookRegularConcert(customer);
```

---

## 处理继承关系

### 方法上移

**适用场景**：多个子类中有相同的方法

**动机**：消除继承体系中的重复。

**操作方法**：
1. 检查方法的实现是否相同
2. 检查签名是否相同
3. 在超类中创建新方法
4. 从一个子类复制方法体
5. 删除一个子类方法，测试
6. 删除其他子类方法，每个删除后测试

---

### 方法下移

**适用场景**：行为仅与子集的某个子集相关

**动机**：将方法放在真正需要它的地方。

**操作方法**：
1. 将方法复制到需要它的每个子类
2. 从超类中移除方法
3. 测试
4. 从不需要它的子类中移除
5. 测试

---

### 以委托取代子类

**适用场景**：不正确地使用继承、需要更多灵活性

**动机**：在适当时偏好组合而非继承。

**操作方法**：
1. 为委托创建一个空类
2. 在持有类中添加一个持有委托的字段
3. 创建委托的构造函数，由宿主类调用
4. 将特性移到委托
5. 每次移动后测试
6. 用委托取代继承关系

---

## 提取类

**适用场景**：承担多个职责的大类

**动机**：将类拆分以保证单一职责。

**操作方法**：
1. 决定如何拆分职责
2. 创建新类
3. 从原始类移动字段到新类
4. 测试
5. 从原始类移动方法到新类
6. 每次移动后测试
7. 重命名新类
8. 检查并重命名两个类
9. 决定如何暴露新类

**重构前**：
```javascript
class Person {
  get name() { return this._name; }
  set name(arg) { this._name = arg; }
  get officeAreaCode() { return this._officeAreaCode; }
  set officeAreaCode(arg) { this._officeAreaCode = arg; }
  get officeNumber() { return this._officeNumber; }
  set officeNumber(arg) { this._officeNumber = arg; }

  get telephoneNumber() {
    return `(${this._officeAreaCode}) ${this._officeNumber}`;
  }
}
```

**重构后**：
```javascript
class Person {
  constructor() {
    this._telephoneNumber = new TelephoneNumber();
  }
  get name() { return this._name; }
  set name(arg) { this._name = arg; }
  get telephoneNumber() { return this._telephoneNumber.toString(); }
  get officeAreaCode() { return this._telephoneNumber.areaCode; }
  set officeAreaCode(arg) { this._telephoneNumber.areaCode = arg; }
}

class TelephoneNumber {
  get areaCode() { return this._areaCode; }
  set areaCode(arg) { this._areaCode = arg; }
  get number() { return this._number; }
  set number(arg) { this._number = arg; }
  toString() { return `(${this._areaCode}) ${this._number}`; }
}
```

---

## 快速参考：异味到重构

| 代码异味 | 主要重构 | 备选方案 |
|------------|-------------------|-------------|
| 长方法 | 提取函数 | 用查询取代临时变量 |
| 重复代码 | 提取函数 | 方法上移 |
| 大类 | 提取类 | 提取子类 |
| 长参数列表 | 引入参数对象 | 保持对象完整 |
| 特性依恋 | 方法移动 | 提取函数 + 移动 |
| 数据泥团 | 提取类 | 引入参数对象 |
| 基本类型偏执 | 以对象取代基本类型 | 以类取代类型码 |
| 条件表达式 | 以多态取代条件表达式 | 以子类取代类型码 |
| 临时字段 | 提取类 | 引入空对象 |
| 消息链 | 隐藏委托 | 提取函数 |
| 中间人 | 移除中间人 | 内联方法 |
| 发散式变化 | 提取类 | 拆分阶段 |
| 霰弹式修改 | 方法移动 | 嵌入式类 |
| 死代码 | 移除死代码 | - |
| 过度泛化 | 折叠继承体系 | 嵌入式类 |

---

## 延伸阅读

- Fowler, M. (2018). 《重构：改善既有代码的设计》（第 2 版）
- 在线目录：https://refactoring.com/catalog/
