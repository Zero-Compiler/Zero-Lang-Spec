# 示例程序

本文档包含 Zero 语言的完整示例程序。

---

## Hello World

最简单的 Zero 程序：

```zero
print("Hello, Zero!");
```

**输出**:

```
Hello, Zero!
```

---

## 变量和类型

展示基本数据类型：

```zero
// 整数
let x = 42;
print(x);

// 浮点数
let y = 3.14;
print(y);

// 字符串
let name = "Zero Language";
print(name);

// 布尔值
let flag = true;
print(flag);
```

**输出**:

```
42
3.14
Zero Language
true
```

---

## 算术运算

展示算术运算符：

```zero
let a = 10;
let b = 3;

print(a + b);  // 加法
print(a - b);  // 减法
print(a * b);  // 乘法
print(a / b);  // 除法
print(a % b);  // 取模
```

**输出**:

```
13
7
30
3
1
```

---

## 条件语句

使用 if-else 进行条件判断：

```zero
let x = 15;

if x > 10 {
    print("x is greater than 10");
} else {
    print("x is less than or equal to 10");
}

// 嵌套 if
let age = 20;
if age >= 18 {
    if age >= 65 {
        print("Senior");
    } else {
        print("Adult");
    }
} else {
    print("Minor");
}
```

**输出**:

```
x is greater than 10
Adult
```

---

## While 循环

使用 while 循环：

```zero
// 计数循环
var counter = 0;
while counter < 5 {
    print(counter);
    counter = counter + 1;
}

// 计算阶乘
var n = 5;
var factorial = 1;
var i = 1;
while i <= n {
    factorial = factorial * i;
    i = i + 1;
}
print(factorial);
```

**输出**:

```
0
1
2
3
4
120
```

---

## For 循环

使用 for 循环遍历范围：

```zero
// 简单循环
for i in 0..5 {
    print(i);
}

// 计算和
var sum = 0;
for i in 1..11 {
    sum = sum + i;
}
print(sum);

// 嵌套循环（打印乘法表）
for i in 1..10 {
    for j in 1..10 {
        print(i * j);
    }
}
```

**输出**:

```
0
1
2
3
4
55
1
2
...
```

---

## 函数

定义和调用函数：

```zero
// 简单函数
fn add(a, b) {
    return a + b;
}

let result = add(10, 20);
print(result);

// 无返回值函数
fn greet(name) {
    print("Hello, ");
    print(name);
}

greet("World");

// 多个函数
fn square(x) {
    return x * x;
}

fn cube(x) {
    return x * x * x;
}

print(square(5));
print(cube(3));
```

**输出**:

```
30
Hello,
World
25
27
```

---

## 递归函数

使用递归计算阶乘和斐波那契数列：

```zero
// 阶乘
fn factorial(n) {
    if n <= 1 {
        return 1;
    } else {
        return n * factorial(n - 1);
    }
}

print(factorial(5));   // 120
print(factorial(10));  // 3628800

// 斐波那契数列
fn fibonacci(n) {
    if n <= 1 {
        return n;
    } else {
        return fibonacci(n - 1) + fibonacci(n - 2);
    }
}

print(fibonacci(0));   // 0
print(fibonacci(1));   // 1
print(fibonacci(5));   // 5
print(fibonacci(10));  // 55
```

**输出**:

```
120
3628800
0
1
5
55
```

---

## 作用域和遮蔽

展示变量作用域和遮蔽：

```zero
let x = 10;
print(x);

{
    let x = 20;
    print(x);
    
    {
        let x = 30;
        print(x);
    }
    
    print(x);
}

print(x);
```

**输出**:

```
10
20
30
20
10
```

---

## 逻辑运算

使用逻辑运算符：

```zero
let a = true;
let b = false;

print(a && b);   // false
print(a || b);   // true
print(!a);       // false
print(!b);       // true

// 短路求值示例
fn expensive() {
    print("Function called");
    return true;
}

let x = false && expensive();  // 不输出 "Function called"
let y = true || expensive();   // 不输出 "Function called"
```

**输出**:

```
false
true
false
true
```

---

## 综合示例：猜数字游戏（简化版）

```zero
// 目标数字
let target = 42;

// 猜测
let guess = 30;

if guess == target {
    print("Correct!");
} else {
    if guess < target {
        print("Too low!");
    } else {
        print("Too high!");
    }
}
```

**输出**:

```
Too low!
```

---

## 综合示例：计算素数

判断一个数是否为素数：

```zero
fn is_prime(n) {
    if n <= 1 {
        return false;
    }
    
    if n == 2 {
        return true;
    }
    
    var i = 2;
    while i * i <= n {
        if n % i == 0 {
            return false;
        }
        i = i + 1;
    }
    
    return true;
}

// 测试
print(is_prime(2));    // true
print(is_prime(7));    // true
print(is_prime(10));   // false
print(is_prime(17));   // true
```

**输出**:

```
true
true
false
true
```

---

## 综合示例：最大公约数

使用欧几里得算法：

```zero
fn gcd(a, b) {
    while b != 0 {
        let temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}

print(gcd(48, 18));   // 6
print(gcd(100, 35));  // 5
```

**输出**:

```
6
5
```

---

## 综合示例：排序（冒泡排序概念）

虽然当前版本不支持数组，但可以展示排序逻辑：

```zero
// 比较和交换两个变量
var a = 5;
var b = 3;

if a > b {
    let temp = a;
    a = b;
    b = temp;
}

print(a);  // 3
print(b);  // 5
```

**输出**:

```
3
5
```

---

这些示例展示了 Zero 语言 0.1.0 版本的所有主要特性。
