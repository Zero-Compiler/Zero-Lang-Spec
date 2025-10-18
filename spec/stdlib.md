# 标准库

本文档定义 Zero 语言的标准库和内置函数。

---

## 内置函数

### print

```zero
print(value)
```

**描述**: 输出值到标准输出

**参数**:

- `value`: 任意类型的值

**返回值**: 无（void）

**行为**:

- **Integer**: 输出十进制表示
- **Float**: 输出十进制表示（至少一位小数）
- **String**: 输出字符串内容（不包含引号）
- **Boolean**: 输出 `true` 或 `false`
- 每次调用输出一行（自动添加换行符）

**示例**:

```zero
print("Hello, World!");  // 输出: Hello, World!
print(42);               // 输出: 42
print(3.14);             // 输出: 3.14
print(true);             // 输出: true

// 多行输出
print("Line 1");
print("Line 2");
// 输出:
// Line 1
// Line 2
```

**输出目标**: 标准输出（stdout）

**编码**: UTF-8

**缓冲**: 实现定义（建议行缓冲）

**错误处理**: 输出错误（如管道关闭）导致程序终止

---

## 标准库（当前版本）

当前版本（0.1.0）仅包含 `print` 函数。

---

## TODO: 未来标准库

以下功能计划在未来版本中添加：

### 输入/输出

```zero
// 读取输入
let input = read_line();

// 格式化输出
printf("x = {}, y = {}", x, y);

// 文件操作
let file = open("file.txt", "r");
let content = file.read();
file.close();
```

### 字符串操作

```zero
// 字符串长度
let len = length("hello");  // 5

// 字符串连接
let s = concat("hello", " ", "world");

// 子字符串
let sub = substring("hello", 0, 3);  // "hel"

// 分割
let parts = split("a,b,c", ",");  // ["a", "b", "c"]
```

### 数学函数

```zero
// 基本数学
let a = abs(-5);       // 5
let p = pow(2, 10);    // 1024
let r = sqrt(16.0);    // 4.0

// 三角函数
let s = sin(3.14);
let c = cos(3.14);
let t = tan(3.14);

// 取整
let f = floor(3.7);    // 3.0
let c = ceil(3.2);     // 4.0
let r = round(3.5);    // 4.0
```

### 类型转换

```zero
// 转换为字符串
let s = to_string(42);       // "42"

// 转换为整数
let i = to_int("42");        // 42
let i = to_int(3.14);        // 3

// 转换为浮点数
let f = to_float(42);        // 42.0
let f = to_float("3.14");    // 3.14
```

### 数组操作

```zero
// 创建数组
let arr = [1, 2, 3, 4, 5];

// 数组长度
let len = length(arr);

// 访问元素
let x = arr[0];

// 追加元素
append(arr, 6);

// 遍历
for item in arr {
    print(item);
}
```

### 错误处理

```zero
// Result 类型
let result = try_parse("42");
if result.is_ok() {
    let value = result.unwrap();
    print(value);
} else {
    print("Parse error");
}
```

---

## 标准库设计原则

1. **简洁性**: 提供常用功能，避免冗余
2. **安全性**: 明确的错误处理
3. **一致性**: 统一的命名和接口风格
4. **高效性**: 优化的实现
5. **可扩展性**: 易于添加新功能
