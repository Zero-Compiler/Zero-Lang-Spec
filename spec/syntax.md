# 语法规范

本文档使用扩展巴科斯-瑙尔范式（EBNF）定义 Zero 语言的语法。

---

## EBNF 表示法

本文档使用以下 EBNF 约定：

```ebnf
symbol ::= definition     规则定义
'literal'                 字面量（终结符）
A | B                     选择（A 或 B）
A B                       连接（A 后跟 B）
A*                        零次或多次重复
A+                        一次或多次重复
A?                        可选（零次或一次）
(...)                     分组
[...]                     字符类
```

---

## 程序结构

```ebnf
Program ::= Statement*
```

**说明**:

- 一个程序由零个或多个语句组成
- 语句按顺序执行

---

## 语句

```ebnf
Statement ::= VariableDecl
            | ExpressionStmt
            | Block
            | IfStatement
            | WhileStatement
            | ForStatement
            | FunctionDecl
            | ReturnStatement
```

### 变量声明

```ebnf
VariableDecl ::= ('let' | 'var') Identifier '=' Expression ';'
```

**说明**:

- `let` 声明不可变变量
- `var` 声明可变变量
- 变量必须初始化
- 以分号结束

### 表达式语句

```ebnf
ExpressionStmt ::= Expression ';'
```

**说明**:

- 任何表达式后跟分号都是语句
- 表达式的值被丢弃

### 块

```ebnf
Block ::= '{' Statement* '}'
```

**说明**:

- 块由花括号包围
- 包含零个或多个语句
- 块创建新的作用域

### If 语句

```ebnf
IfStatement ::= 'if' Expression Block ('else' Block)?
```

**说明**:

- 条件表达式不需要括号
- `then` 分支和 `else` 分支都是块
- `else` 分支可选

### While 循环

```ebnf
WhileStatement ::= 'while' Expression Block
```

**说明**:

- 条件表达式不需要括号
- 循环体是块

### For 循环

```ebnf
ForStatement ::= 'for' Identifier 'in' Expression Block
```

**说明**:

- 循环变量不需要声明
- 范围表达式通常是 `start..end` 形式
- 循环体是块

### 函数声明

```ebnf
FunctionDecl ::= 'fn' Identifier '(' ParameterList? ')' Block

ParameterList ::= Identifier (',' Identifier)*
```

**说明**:

- 参数列表可以为空
- 参数之间用逗号分隔
- 函数体是块
- 不需要显式声明返回类型

### Return 语句

```ebnf
ReturnStatement ::= 'return' Expression? ';'
```

**说明**:

- 返回表达式可选
- 以分号结束

---

## 表达式

表达式使用优先级爬升法解析，从低优先级到高优先级：

```ebnf
Expression ::= Assignment

Assignment ::= Identifier '=' Assignment
             | LogicalOr

LogicalOr ::= LogicalAnd ('||' LogicalAnd)*

LogicalAnd ::= Equality ('&&' Equality)*

Equality ::= Comparison (('==' | '!=') Comparison)*

Comparison ::= Range (('<' | '<=' | '>' | '>=') Range)*

Range ::= Addition ('..' Addition)?

Addition ::= Multiplication (('+' | '-') Multiplication)*

Multiplication ::= Unary (('*' | '/' | '%') Unary)*

Unary ::= ('!' | '-') Unary
        | Call

Call ::= Primary ('(' ArgumentList? ')')*

ArgumentList ::= Expression (',' Expression)*

Primary ::= IntegerLiteral
          | FloatLiteral
          | StringLiteral
          | BooleanLiteral
          | Identifier
          | '(' Expression ')'
```

**优先级说明**（从高到低）:

1. **Primary**: 字面量、标识符、括号表达式
2. **Call**: 函数调用
3. **Unary**: 一元运算符（`!`, `-`）
4. **Multiplication**: 乘法、除法、取模（`*`, `/`, `%`）
5. **Addition**: 加法、减法（`+`, `-`）
6. **Range**: 范围运算符（`..`）
7. **Comparison**: 比较运算符（`<`, `<=`, `>`, `>=`）
8. **Equality**: 相等性运算符（`==`, `!=`）
9. **LogicalAnd**: 逻辑与（`&&`）
10. **LogicalOr**: 逻辑或（`||`）
11. **Assignment**: 赋值（`=`）

---

## 完整的 EBNF 语法

以下是 Zero 语言的完整形式化语法：

```ebnf
(* 词法 *)
Identifier ::= IdentifierStart IdentifierContinue*
IdentifierStart ::= [a-zA-Z_]
IdentifierContinue ::= [a-zA-Z0-9_]

IntegerLiteral ::= '0' | [1-9][0-9]*
FloatLiteral ::= DecimalLiteral '.' [0-9]+
DecimalLiteral ::= '0' | [1-9][0-9]*
StringLiteral ::= '"' StringCharacter* '"'
StringCharacter ::= EscapeSequence | [^"\\\n]
EscapeSequence ::= '\\' ('n' | 't' | '\\' | '"')
BooleanLiteral ::= 'true' | 'false'

Comment ::= '//' [^\n]* '\n'

(* 程序结构 *)
Program ::= Statement*

(* 语句 *)
Statement ::= VariableDecl
            | ExpressionStmt
            | Block
            | IfStatement
            | WhileStatement
            | ForStatement
            | FunctionDecl
            | ReturnStatement

VariableDecl ::= ('let' | 'var') Identifier '=' Expression ';'
ExpressionStmt ::= Expression ';'
Block ::= '{' Statement* '}'
IfStatement ::= 'if' Expression Block ('else' Block)?
WhileStatement ::= 'while' Expression Block
ForStatement ::= 'for' Identifier 'in' Expression Block
FunctionDecl ::= 'fn' Identifier '(' ParameterList? ')' Block
ParameterList ::= Identifier (',' Identifier)*
ReturnStatement ::= 'return' Expression? ';'

(* 表达式 *)
Expression ::= Assignment
Assignment ::= Identifier '=' Assignment | LogicalOr
LogicalOr ::= LogicalAnd ('||' LogicalAnd)*
LogicalAnd ::= Equality ('&&' Equality)*
Equality ::= Comparison (('==' | '!=') Comparison)*
Comparison ::= Range (('<' | '<=' | '>' | '>=') Range)*
Range ::= Addition ('..' Addition)?
Addition ::= Multiplication (('+' | '-') Multiplication)*
Multiplication ::= Unary (('*' | '/' | '%') Unary)*
Unary ::= ('!' | '-') Unary | Call
Call ::= Primary ('(' ArgumentList? ')')*
ArgumentList ::= Expression (',' Expression)*
Primary ::= IntegerLiteral
          | FloatLiteral
          | StringLiteral
          | BooleanLiteral
          | Identifier
          | '(' Expression ')'
```

---

## 语法约定

### 分号

- 所有语句必须以分号结束（块语句除外）
- 分号不是可选的
- 表达式语句也需要分号

### 花括号

- 代码块必须使用花括号 `{}`
- 单行语句也必须使用花括号
- 花括号不能省略

**示例**:

```zero
// 正确
if x > 10 {
    print(x);
}

// 错误：缺少花括号
if x > 10
    print(x);
```

### 括号

- 条件表达式（if, while）不需要括号
- 函数参数列表需要括号
- 可以使用括号改变表达式求值顺序

---

## 语法歧义消解

### 悬空 else 问题

`else` 总是与最近的 `if` 匹配：

```zero
if a
    if b {
        ...
    } else {    // 与内层 if 匹配
        ...
    }
```

### 赋值表达式

赋值是右结合的：

```zero
a = b = c;  // 等价于 a = (b = c);
```

### 函数调用

函数调用是左结合的：

```zero
f()()  // 先调用 f()，然后调用结果
```

---

## 语法错误

以下情况会导致语法错误：

1. **缺少分号**: 语句未以分号结束
2. **不匹配的括号**: 括号、花括号不配对
3. **无效的表达式**: 表达式不符合语法规则
4. **关键字误用**: 关键字用在错误的位置

**示例**:

```zero
let x = 42          // 错误：缺少分号
if x > 10           // 错误：缺少块
fn add(a b) {       // 错误：参数缺少逗号
    return a + b;
}
```

---

## 总结

Zero 的语法设计原则：

- **明确性**: 使用分号和花括号消除歧义
- **简洁性**: 条件表达式不需要括号
- **一致性**: 所有控制流结构使用块
- **可预测性**: 遵循常见语言的语法惯例
