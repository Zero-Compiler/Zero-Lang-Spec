# 函数

本文档定义 Zero 语言的函数系统。

---

## 函数声明

**语法**:

```zero
fn function_name(param1, param2, ...) {
    // 函数体
}
```

**规则**:

- 函数名必须是合法标识符
- 参数列表可以为空
- 参数之间用逗号分隔
- 参数类型通过使用位置推断
- 函数体是一个块
- 函数必须在调用前声明

**示例**:

```zero
// 无参数函数
fn say_hello() {
    print("Hello!");
}

// 单参数函数
fn square(x) {
    return x * x;
}

// 多参数函数
fn add(a, b) {
    return a + b;
}

fn calculate(x, y, z) {
    return (x + y) * z;
}
```

---

## 函数调用

**语法**: `function_name(arg1, arg2, ...)`

**规则**:

- 参数数量必须与函数声明匹配
- 参数类型必须与函数期望的类型匹配
- 函数调用是表达式（如果函数有返回值）

**示例**:

```zero
say_hello();              // 无参数调用
let s = square(5);        // 单参数调用
let sum = add(10, 20);    // 多参数调用
```

---

## 参数传递

### 值传递

Zero 使用**值传递**（pass-by-value）语义：

- 基本类型（Integer, Float, Boolean）按值复制
- String 按值传递（不可变，实现可以优化为引用传递）

**示例**:

```zero
fn modify(x) {
    x = x + 1;  // 仅修改局部副本
    return x;
}

var a = 10;
let b = modify(a);
print(a);  // 输出: 10（a 未改变）
print(b);  // 输出: 11
```

### 参数求值顺序

函数参数从左到右求值：

```zero
fn func(a, b, c) {
    print(a);
    print(b);
    print(c);
}

func(f(), g(), h());  // 求值顺序: f(), g(), h()
```

---

## 返回值

### 有返回值的函数

使用 `return` 语句返回值：

```zero
fn add(a, b) {
    return a + b;
}

fn max(a, b) {
    if a > b {
        return a;
    } else {
        return b;
    }
}
```

### 无返回值的函数

不使用 `return` 或使用 `return;`：

```zero
fn greet(name) {
    print("Hello, ");
    print(name);
    // 无 return 语句
}

fn log(message) {
    print(message);
    return;  // 显式 return，但无返回值
}
```

**TODO**: 未来版本可能引入 `void` 或 `unit` 类型来表示无返回值。

---

## 类型推断

### 参数类型推断

参数类型通过函数的实际使用推断：

```zero
fn square(x) {
    return x * x;
}

// 第一次调用推断类型
let a = square(5);      // x 推断为 Integer
```

**限制**: 当前版本不支持函数重载，同一函数只能有一种参数类型。

### 返回值类型推断

返回值类型通过 `return` 语句推断：

```zero
fn get_number() {
    return 42;  // 返回类型推断为 Integer
}

fn get_name() {
    return "Alice";  // 返回类型推断为 String
}
```

---

## 作用域

### 参数作用域

参数在整个函数体内可见：

```zero
fn func(x, y) {
    print(x);  // 可访问参数
    {
        print(y);  // 内层块也可访问参数
    }
}
```

### 局部变量

函数体内声明的变量是局部的：

```zero
fn func() {
    let local = 10;
    print(local);
}

// print(local);  // 错误：local 不在作用域
```

### 访问外层变量

函数可以访问在其外层声明的变量：

```zero
let global = 100;

fn use_global() {
    print(global);  // 可访问全局变量
}
```

---

## 递归

函数支持递归调用：

**示例**:

```zero
// 阶乘
fn factorial(n) {
    if n <= 1 {
        return 1;
    } else {
        return n * factorial(n - 1);
    }
}

print(factorial(5));  // 120

// 斐波那契
fn fibonacci(n) {
    if n <= 1 {
        return n;
    } else {
        return fibonacci(n - 1) + fibonacci(n - 2);
    }
}

print(fibonacci(10));  // 55
```

**限制**:

- 当前版本没有尾调用优化
- 深度递归可能导致栈溢出
- 建议递归深度不超过数千层

**TODO**: 未来版本可能添加尾调用优化。

---

## 函数作为值

**当前限制**: 函数不能作为值传递或赋值。

**TODO**: 未来版本可能支持：

- 函数作为参数
- 函数作为返回值
- 匿名函数/闭包
- 高阶函数

---

## 函数重载

**当前限制**: 不支持函数重载。

同一作用域中不能有同名函数：

```zero
fn add(a, b) {
    return a + b;
}

// 错误：重复定义
fn add(a, b, c) {
    return a + b + c;
}
```

**TODO**: 未来版本可能支持基于参数数量或类型的重载。

---

## 默认参数

**当前限制**: 不支持默认参数。

所有参数都是必需的：

```zero
fn greet(name) {
    print(name);
}

greet("Alice");  // 正确
// greet();      // 错误：缺少参数
```

**TODO**: 未来版本可能支持默认参数。

---

## 可变参数

**当前限制**: 不支持可变参数。

参数数量是固定的：

```zero
fn sum(a, b) {
    return a + b;
}

sum(1, 2);       // 正确
// sum(1, 2, 3); // 错误：参数太多
```

**TODO**: 未来版本可能支持可变参数。

---

## 函数示例

### 实用函数

```zero
// 绝对值
fn abs(x) {
    if x < 0 {
        return -x;
    } else {
        return x;
    }
}

// 最大值
fn max(a, b) {
    if a > b {
        return a;
    } else {
        return b;
    }
}

// 最小值
fn min(a, b) {
    if a < b {
        return a;
    } else {
        return b;
    }
}
```

### 数学函数

```zero
// 幂运算（整数）
fn power(base, exp) {
    var result = 1;
    var i = 0;
    while i < exp {
        result = result * base;
        i = i + 1;
    }
    return result;
}

// 最大公约数
fn gcd(a, b) {
    while b != 0 {
        let temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```

---

## 函数设计原则

1. **单一职责**: 每个函数应该只做一件事
2. **明确命名**: 函数名应清晰表达功能
3. **参数数量**: 避免过多参数（建议不超过 5 个）
4. **返回一致**: 所有返回路径应返回相同类型
5. **避免副作用**: 尽量避免修改全局状态

---

## TODO 列表

未来版本计划的函数增强：

- [ ] 默认参数
- [ ] 可变参数
- [ ] 函数重载
- [ ] 匿名函数/闭包
- [ ] 函数作为一等公民
- [ ] 高阶函数
- [ ] 尾调用优化
- [ ] 泛型函数
