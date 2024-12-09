# 表达式

在编程语言中，表达式是返回一个值的代码单元。在 Move 中，几乎所有东西都是表达式，唯一的例外是 let 语句，它是一个声明。本节将介绍表达式的类型以及作用域的概念。

> 表达式之间使用分号 ; 隔开。如果分号后面没有表达式，编译器会插入一个单位值 () - 一个空表达式。

## 字面量

我们在 原始类型 一节介绍了 Move 的基本类型。为了举例说明，我们使用了字面量。字面量是一种在源代码中表示固定值的符号。字面量用于初始化变量和向函数传递参数。Move 支持以下字面量：

- 布尔值：true 和 false
- 整数：0、1、123123 等数字
- 整数 (十六进制)：0x0、0x1、0x123 等十六进制表示
- 字节向量：b"bytes_vector"
- 字节 (十六进制)：x"0A"

```move
let b = true;     // true is a literal
let n = 1000;     // 1000 is a literal
let h = 0x0A;     // 0x0A is a literal
let v = b"hello"; // b'hello' is a byte vector literal
let x = x"0A";    // x'0A' is a byte vector literal
let c = vector[1, 2, 3]; // vector[] is a vector literal
```

## 运算符

算术运算符、逻辑运算符和位运算符用于对值进行运算。运算的结果是一个值，因此运算符也是表达式。

```move
let sum = 1 + 2;   // 1 + 2 is an expression
let sum = (1 + 2); // the same expression with parentheses
let is_true = true && false; // true && false is an expression
let is_true = (true && false); // the same expression with parentheses
```

## 代码块

代码块是一系列语句和表达式的组合，它返回代码块中最后一个表达式的值。代码块用一对花括号 {} 表示。代码块也是一个表达式，因此它可以在任何需要表达式的场合使用。

```move
// block with an empty expression, however, the compiler will
// insert an empty expression automatically: `let none = { () }`
// let none = {};

// block with let statements and an expression.
let sum = {
    let a = 1;
    let b = 2;
    a + b // last expression is the value of the block
};

// block is an expression, so it can be used in an expression and
// doesn't have to be assigned to a variable.
{
    let a = 1;
    let b = 2;
    a + b; // not returned - semicolon.
    // compiler automatically inserts an empty expression `()`
};
```

## 函数调用

我们将在 函数 一节详细讲解函数。但是我们在之前的章节已经使用过函数调用，所以这里值得一提。函数调用是一种表达式，它调用一个函数并返回函数体中最后一个表达式的值。

```move
fun add(a: u8, b: u8): u8 {
    a + b
}

#[test]
fun some_other() {
    let sum = add(1, 2); // add(1, 2) is an expression with type u8
}
```

## 控制流表达式

控制流表达式用于控制程序的流程。它们也是表达式，因此会返回值。我们将在 控制流 一节介绍控制流表达式。这里是一个非常简短的概述：

```move
// if is an expression, so it returns a value; if there are 2 branches,
// the types of the branches must match.
if (bool_expr) expr1 else expr2;

// while is an expression, but it returns `()`.
while (bool_expr) { expr; };

// loop is an expression, but returns `()` as well.
loop { expr; break };
```
