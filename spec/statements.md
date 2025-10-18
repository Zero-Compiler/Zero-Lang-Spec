# 语句

本文档定义 Zero 语言中的各种语句。

---

## 概述

语句执行动作，不产生值（或产生的值被丢弃）。Zero 中的语句包括：

- 变量声明语句
- 表达式语句
- 块语句
- If 语句
- While 循环
- For 循环
- 函数声明
- Return 语句

---

## 变量声明语句

### Let 声明（不可变变量）

**语法**: `let identifier = expression;`

**特性**:

- 声明不可变变量
- 必须初始化
- 不能重新赋值
- 可以被遮蔽（shadowing）

**示例**:

```zero
let x = 42;
let name = "Alice";
let is_valid = true;

// x = 43;  // 错误：不能重新赋值

// 遮蔽
let x = 100;  // 正确：创建新变量
```

---

### Var 声明（可变变量）

**语法**: `var identifier = expression;`

**特性**:

- 声明可变变量
- 必须初始化
- 可以重新赋值
- 类型不能改变

**示例**:

```zero
var count = 0;
var sum = 0.0;

count = count + 1;  // 正确
sum = sum + 3.14;   // 正确

// count = "hello";  // 错误：类型不匹配
```

---

## 表达式语句

任何表达式后跟分号都是语句。

**语法**: `expression;`

**示例**:

```zero
print("Hello");      // 函数调用
add(10, 20);         // 函数调用（值被丢弃）
x + y;               // 算术表达式（值被丢弃）
x = 10;              // 赋值表达式
```

---

## 块语句

块由花括号包围，包含零个或多个语句。

**语法**: `{ statement* }`

**特性**:

- 创建新的作用域
- 块内声明的变量在块外不可见
- 块可以嵌套
- 空块是合法的

**示例**:

```zero
{
    let x = 10;
    print(x);
}

// print(x);  // 错误：x 不在作用域内

// 嵌套块
{
    let a = 1;
    {
        let b = 2;
        print(a);  // 可访问外层变量
        print(b);
    }
    // print(b);  // 错误：b 不在作用域
}

// 空块
{
}
```

---

## If 语句

条件分支语句。

**语法**:

```zero
if expression {
    statements
}

if expression {
    statements
} else {
    statements
}
```

**特性**:

- 条件表达式必须是 Boolean 类型
- `then` 分支和 `else` 分支都是块
- `else` 分支可选
- 条件表达式不需要括号

**示例**:

```zero
// 简单 if
if x > 10 {
    print("large");
}

// if-else
if x > 10 {
    print("large");
} else {
    print("small");
}

// 嵌套 if（模拟 else if）
if x > 100 {
    print("very large");
} else {
    if x > 10 {
        print("large");
    } else {
        print("small");
    }
}
```

---

## While 循环

条件循环语句。

**语法**:

```zero
while expression {
    statements
}
```

**特性**:

- 条件表达式必须是 Boolean 类型
- 在每次迭代前求值条件
- 条件为 `true` 时执行循环体
- 条件为 `false` 时退出循环
- 当前版本不支持 `break` 和 `continue`

**示例**:

```zero
// 计数循环
var i = 0;
while i < 5 {
    print(i);
    i = i + 1;
}

// 条件循环
var done = false;
while !done {
    // ...
    done = true;
}

// 无限循环（需要 return 或程序终止来退出）
while true {
    print("infinite");
}
```

---

## For 循环

范围迭代循环。

**语法**:

```zero
for identifier in expression {
    statements
}
```

**特性**:

- 表达式必须是范围表达式（`start..end`）
- 循环变量在每次迭代中依次取范围内的值
- 循环变量是不可变的
- 循环变量作用域限于循环体
- 范围是半开区间 `[start, end)`

**示例**:

```zero
// 基本 for 循环
for i in 0..5 {
    print(i);  // 输出 0, 1, 2, 3, 4
}

// 使用循环变量
for i in 1..11 {
    print(i * i);  // 输出平方数
}

// 嵌套 for 循环
for i in 0..3 {
    for j in 0..3 {
        print(i * 10 + j);
    }
}

// 空范围
for i in 5..5 {
    // 不执行
}
```

**等价的 while 循环**:

```zero
for i in 0..5 {
    print(i);
}

// 等价于:
{
    var i = 0;
    let _end = 5;
    while i < _end {
        print(i);
        i = i + 1;
    }
}
```

---

## 函数声明

定义函数的语句。

**语法**:

```zero
fn identifier(parameters) {
    statements
}
```

**特性**:

- 函数名必须是合法标识符
- 参数列表可以为空
- 参数类型通过使用推断
- 函数体是块
- 函数必须在调用前声明

**示例**:

```zero
// 无参数函数
fn greet() {
    print("Hello!");
}

// 带参数函数
fn add(a, b) {
    return a + b;
}

// 多条语句
fn complex_calc(x, y) {
    let temp = x * x;
    let result = temp + y;
    return result;
}
```

---

## Return 语句

从函数返回的语句。

**语法**:

```zero
return;
return expression;
```

**特性**:

- 只能在函数体内使用
- `return;` 不返回值（用于无返回值函数）
- `return expression;` 返回表达式的值
- 执行 return 后立即退出函数

**示例**:

```zero
// 返回值
fn square(x) {
    return x * x;
}

// 无返回值
fn log_message(msg) {
    print(msg);
    return;
}

// 提前返回
fn abs(x) {
    if x < 0 {
        return -x;
    }
    return x;
}

// 条件返回
fn max(a, b) {
    if a > b {
        return a;
    } else {
        return b;
    }
}
```

---

## 语句的执行顺序

语句按照在代码中出现的顺序依次执行：

```zero
let x = 1;      // 1. 声明 x
print(x);       // 2. 输出 x
x = 2;          // 错误：x 是不可变的

var y = 1;      // 1. 声明 y
print(y);       // 2. 输出 y
y = 2;          // 3. 修改 y
print(y);       // 4. 输出新的 y
```

---

## 控制流

### 顺序执行

默认情况下，语句顺序执行。

### 条件执行

使用 `if` 语句根据条件选择性执行。

### 循环执行

使用 `while` 或 `for` 循环重复执行。

### 函数调用

调用函数会跳转到函数体，执行完毕后返回。

### Return

`return` 语句立即退出当前函数。

---

## 语句嵌套

语句可以嵌套在其他语句中：

```zero
// if 嵌套在 while 中
while condition {
    if x > 10 {
        print(x);
    }
}

// for 嵌套在 if 中
if flag {
    for i in 0..5 {
        print(i);
    }
}
```

---

## 总结

Zero 的语句系统：

- **清晰**: 明确的语法和语义
- **简洁**: 最少的关键字和符号
- **一致**: 统一的代码块风格（花括号）
- **可组合**: 语句可以嵌套和组合
