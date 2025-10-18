# 错误示例

本文档展示 Zero 语言中常见的错误和非法代码。

---

## 词法错误

### 未闭合字符串

```zero
// 错误
let s = "unclosed string

// 错误
let s = "missing quote
```

**错误信息**: 未闭合的字符串字面量

---

### 无效转义序列

```zero
// 错误：不支持的转义
let s = "invalid \x escape";

// 错误：不支持的转义
let s = "bad \u1234 escape";
```

**错误信息**: 无效的转义序列

---

### 前导零

```zero
// 错误
let x = 007;

// 错误
let y = 00;

// 正确
let z = 0;
```

**错误信息**: 整数字面量不允许前导零

---

## 语法错误

### 缺少分号

```zero
// 错误
let x = 42
let y = 10

// 正确
let x = 42;
let y = 10;
```

**错误信息**: 期望分号

---

### 缺少花括号

```zero
// 错误
if x > 10
    print(x);

// 错误
while x < 5
    x = x + 1;

// 正确
if x > 10 {
    print(x);
}
```

**错误信息**: 期望 `{`

---

### 函数参数缺少逗号

```zero
// 错误
fn add(a b) {
    return a + b;
}

// 正确
fn add(a, b) {
    return a + b;
}
```

**错误信息**: 期望 `,` 或 `)`

---

### 无效的表达式

```zero
// 错误：连续的运算符
let x = 10 + + 20;

// 错误：缺少操作数
let y = 10 + ;

// 正确
let x = 10 + 20;
```

**错误信息**: 无效的表达式

---

## 类型错误

### 类型不匹配

```zero
// 错误：Integer + Float
let x = 10 + 3.14;

// 错误：Integer + String
let y = 42 + "hello";

// 错误：Boolean 算术
let z = true + false;
```

**错误信息**: 类型不匹配，期望相同类型

---

### 不可变变量赋值

```zero
// 错误
let x = 42;
x = 43;

// 正确
var x = 42;
x = 43;
```

**错误信息**: 不能对不可变变量赋值

---

### 改变变量类型

```zero
// 错误
var x = 42;
x = "hello";

// 错误
let y = 10;
let y = "string";  // 即使遮蔽也不能在同一作用域
```

**错误信息**: 类型不匹配

---

### 未初始化变量

```zero
// 错误
let x;

// 错误
var y;

// 正确
let x = 0;
var y = 0;
```

**错误信息**: 变量必须初始化

---

## 语义错误

### 使用未声明的变量

```zero
// 错误
print(undefined_var);

// 错误
let x = y + 10;  // y 未声明
```

**错误信息**: 未声明的变量

---

### 函数参数数量不匹配

```zero
fn add(a, b) {
    return a + b;
}

// 错误：参数太多
let x = add(1, 2, 3);

// 错误：参数太少
let y = add(1);
```

**错误信息**: 函数期望 2 个参数，但提供了 N 个

---

### 条件表达式非布尔类型

```zero
// 错误
if 10 {
    print("hello");
}

// 错误
while "hello" {
    print("loop");
}

// 正确
if 10 > 5 {
    print("hello");
}
```

**错误信息**: 条件表达式必须是 Boolean 类型

---

### 在函数外使用 return

```zero
// 错误
let x = 10;
return x;

// 正确
fn get_value() {
    return 10;
}
```

**错误信息**: `return` 只能在函数内使用

---

### 范围运算符用于非整数

```zero
// 错误
for i in 0.0..10.0 {
    print(i);
}

// 错误
for i in "a".."z" {
    print(i);
}

// 正确
for i in 0..10 {
    print(i);
}
```

**错误信息**: 范围运算符要求 Integer 类型

---

### 取模运算用于浮点数

```zero
// 错误
let x = 3.14 % 2.0;

// 正确（仅用于整数）
let y = 10 % 3;
```

**错误信息**: `%` 运算符仅适用于 Integer 类型

---

## 运行时错误

### 除以零（整数）

```zero
let x = 10 / 0;   // 运行时错误
let y = 10 % 0;   // 运行时错误
```

**错误信息**: 除以零

---

### 栈溢出（深度递归）

```zero
fn infinite_recursion() {
    infinite_recursion();
}

infinite_recursion();  // 运行时错误
```

**错误信息**: 栈溢出

---

## 关键字误用

### 关键字作为标识符

```zero
// 错误
let if = 10;
let while = 20;
let fn = 30;

// 正确
let value = 10;
```

**错误信息**: 期望标识符，但得到关键字

---

### 使用保留字

```zero
// 错误（虽然 print 不是关键字，但在某些上下文中不能重新定义）
let let = 10;
let var = 20;
```

---

## 作用域错误

### 访问块外变量

```zero
{
    let x = 10;
}
print(x);  // 错误：x 不在作用域内
```

**错误信息**: 未声明的变量

---

### 在声明前使用变量

```zero
// 错误
let y = x + 10;
let x = 5;

// 正确
let x = 5;
let y = x + 10;
```

**错误信息**: 未声明的变量

---

## 字符串错误

### 跨行字符串

```zero
// 错误
let s = "This is
a multi-line string";

// 正确（使用转义）
let s = "This is\na multi-line string";
```

**错误信息**: 字符串不能跨行

---

### 单引号字符串

```zero
// 错误
let s = 'single quote';

// 正确
let s = "double quote";
```

**错误信息**: 期望 `"` 或标识符

---

## 表达式错误

### 赋值给表达式

```zero
// 错误
10 = x;

// 错误
a + b = c;

// 正确
x = 10;
```

**错误信息**: 赋值目标必须是变量

---

### 调用非函数

```zero
let x = 42;

// 错误
let y = x();
```

**错误信息**: 不能调用非函数值

---

这些示例展示了 Zero 语言中需要避免的常见错误。在编写代码时，请遵循语法规则和类型系统的约束。
