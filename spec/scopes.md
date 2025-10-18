# 作用域和生命周期

本文档定义 Zero 语言的作用域和变量生命周期规则。

---

## 词法作用域

Zero 使用**词法作用域**（静态作用域）：

- 变量的可见性由代码结构决定
- 内层作用域可以访问外层作用域的变量
- 外层作用域不能访问内层作用域的变量
- 作用域在编译时确定

---

## 作用域层次

### 1. 全局作用域

文件顶层声明的变量和函数具有全局作用域。

**示例**:

```zero
let global_var = 100;

fn global_func() {
    print(global_var);  // 可访问全局变量
}
```

### 2. 函数作用域

函数参数和函数体内声明的变量具有函数作用域。

**示例**:

```zero
fn my_function(param) {
    let local = 10;
    print(param);  // 参数在整个函数内可见
    print(local);  // 局部变量
}

// print(param);  // 错误：param 不在作用域
// print(local);  // 错误：local 不在作用域
```

### 3. 块作用域

块（`{}`）内声明的变量具有块作用域。

**示例**:

```zero
{
    let block_var = 20;
    print(block_var);
}

// print(block_var);  // 错误：block_var 不在作用域
```

---

## 作用域嵌套

作用域可以嵌套，内层作用域可以访问外层作用域的变量：

```zero
let outer = 1;

fn my_function() {
    let middle = 2;
    
    {
        let inner = 3;
        print(outer);   // 可访问
        print(middle);  // 可访问
        print(inner);   // 可访问
    }
    
    print(outer);   // 可访问
    print(middle);  // 可访问
    // print(inner); // 错误：inner 不在作用域
}
```

---

## 变量遮蔽（Shadowing）

内层作用域可以声明与外层同名的变量，内层变量会遮蔽外层变量。

### Let 遮蔽

使用 `let` 可以遮蔽外层变量：

```zero
let x = 10;
print(x);  // 输出: 10

{
    let x = 20;
    print(x);  // 输出: 20（遮蔽外层 x）
    
    {
        let x = 30;
        print(x);  // 输出: 30（遮蔽外外层 x）
    }
    
    print(x);  // 输出: 20
}

print(x);  // 输出: 10
```

### 遮蔽的特性

- 遮蔽可以改变类型
- 遮蔽不会修改外层变量
- 离开作用域后，遮蔽的变量消失

**示例**:

```zero
let x = 42;          // Integer
{
    let x = "hello"; // String（类型改变）
    print(x);        // 输出: hello
}
print(x);            // 输出: 42
```

### Var 不会遮蔽

使用 `var` 赋值不会创建新变量，而是修改现有变量：

```zero
var x = 10;
{
    x = 20;    // 修改外层的 x，不是遮蔽
    print(x);  // 输出: 20
}
print(x);      // 输出: 20（外层 x 被修改）
```

---

## 变量声明规则

### Let 声明

- 创建不可变变量
- 可以遮蔽同名变量
- 遮蔽的变量可以是不同类型

```zero
let x = 42;
let x = "hello";  // 正确：遮蔽，创建新变量
```

### Var 声明

- 创建可变变量
- 不能在同一作用域重复声明
- 可以重新赋值但不能改变类型

```zero
var y = 42;
// var y = 100;   // 错误：重复声明
y = 100;          // 正确：重新赋值
// y = "hello";   // 错误：类型不匹配
```

---

## 生命周期

变量的生命周期从声明开始，到作用域结束。

### 进入作用域

当执行到变量声明时，变量被创建：

```zero
{
    // x 还不存在
    let x = 10;
    // x 现在存在
}
```

### 离开作用域

当离开包含变量的作用域时，变量被销毁：

```zero
{
    let x = 10;
    print(x);
}  // x 在此处被销毁
```

### 示例

```zero
let a = 1;  // a 的生命周期开始

{
    let b = 2;  // b 的生命周期开始
    print(a);
    print(b);
}  // b 的生命周期结束

print(a);
// print(b);  // 错误：b 已销毁
```  // a 的生命周期结束

---

## 函数中的作用域

### 参数作用域

函数参数的作用域是整个函数体：

```zero
fn func(x, y) {
    print(x);  // 参数在整个函数内可见
    
    {
        print(y);  // 内层块也可访问参数
    }
}
```

### 局部变量作用域

函数内声明的变量作用域是其所在的块：

```zero
fn func() {
    let x = 10;  // 函数作用域
    
    {
        let y = 20;  // 块作用域
        print(x);
        print(y);
    }
    
    print(x);
    // print(y);  // 错误：y 不在作用域
}
```

---

## 循环中的作用域

### For 循环变量

For 循环变量的作用域仅限于循环体：

```zero
for i in 0..5 {
    print(i);  // i 在循环体内可见
}

// print(i);  // 错误：i 不在作用域
```

### While 循环

While 循环没有特殊的作用域规则：

```zero
var i = 0;
while i < 5 {
    let temp = i * 2;
    print(temp);
    i = i + 1;
}

print(i);     // 可访问（i 在外层声明）
// print(temp);  // 错误：temp 不在作用域
```

---

## 作用域与闭包

**当前限制**: Zero 0.1.0 不支持闭包。

**TODO**: 未来版本可能支持闭包，届时需要定义捕获变量的作用域和生命周期规则。

---

## 内存管理

### 值类型

当前版本的 Zero 只有值类型：

- Integer, Float, Boolean 是栈分配的
- String 是不可变的，实现可以优化内存使用

### 自动内存管理

- 变量在离开作用域时自动销毁
- 不需要手动内存管理
- 无垃圾回收（当前版本）

**TODO**: 未来版本引入引用类型时，需要定义内存管理策略（垃圾回收、引用计数或所有权）。

---

## 最佳实践

1. **最小化作用域**: 在尽可能小的作用域内声明变量
2. **避免遮蔽**: 除非有充分理由，避免遮蔽变量以提高可读性
3. **清晰命名**: 使用清晰的变量名避免混淆
4. **使用 let 优先**: 优先使用 `let` 声明不可变变量
5. **控制 var 作用域**: `var` 变量应在尽可能小的作用域内使用

---

## 示例：作用域和生命周期

```zero
// 全局作用域
let global = 100;

fn example(param) {
    // param 的作用域：整个函数
    
    let func_local = 1;
    // func_local 的作用域：整个函数
    
    {
        let block_local = 2;
        // block_local 的作用域：仅此块
        
        print(global);       // 可访问
        print(param);        // 可访问
        print(func_local);   // 可访问
        print(block_local);  // 可访问
    }
    // block_local 生命周期结束
    
    // print(block_local);  // 错误
    
    for i in 0..3 {
        // i 的作用域：仅循环体
        print(i);
    }
    // i 生命周期结束
    
    // print(i);  // 错误
}
// param 和 func_local 生命周期结束
```

---

## 总结

Zero 的作用域系统：

- **词法作用域**: 编译时确定
- **块级作用域**: 花括号定义作用域
- **变量遮蔽**: 内层可遮蔽外层
- **清晰的生命周期**: 进入声明，离开销毁
- **无垃圾回收**: 简单的栈式内存管理
