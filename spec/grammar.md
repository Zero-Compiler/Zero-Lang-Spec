# 完整 EBNF 语法

本文档包含 Zero 语言的完整形式化语法定义。

---

## EBNF 表示法约定

```zero
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

## 词法语法

```ebnf
(* 标识符 *)
Identifier ::= IdentifierStart IdentifierContinue*
IdentifierStart ::= [a-zA-Z_]
IdentifierContinue ::= [a-zA-Z0-9_]

(* 字面量 *)
IntegerLiteral ::= '0' | [1-9][0-9]*

FloatLiteral ::= DecimalLiteral '.' [0-9]+
DecimalLiteral ::= '0' | [1-9][0-9]*

StringLiteral ::= '"' StringCharacter* '"'
StringCharacter ::= EscapeSequence | [^"\\\n]
EscapeSequence ::= '\\' ('n' | 't' | '\\' | '"')

BooleanLiteral ::= 'true' | 'false'

(* 注释 *)
Comment ::= '//' [^\n]* '\n'

(* 空白符 *)
Whitespace ::= [ \t\n\r]+
```

---

## 语法结构

```ebnf
(* 程序 *)
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

---

## 关键字

```ebnf
Keyword ::= 'let' | 'var' | 'fn' | 'return'
          | 'if' | 'else' | 'while' | 'for' | 'in'
          | 'true' | 'false'
```

---

## 运算符

```ebnf
Operator ::= '+' | '-' | '*' | '/' | '%'
           | '==' | '!=' | '<' | '<=' | '>' | '>='
           | '&&' | '||' | '!'
           | '='
           | '..'
```

---

## 标点符号

```ebnf
Punctuation ::= ';' | ',' | '(' | ')' | '{' | '}'
```

---

## 完整词法单元

```ebnf
Token ::= Identifier
        | IntegerLiteral
        | FloatLiteral
        | StringLiteral
        | BooleanLiteral
        | Keyword
        | Operator
        | Punctuation
```

---

此语法可用于实现 Zero 语言的解析器。
