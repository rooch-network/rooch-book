# 原始类型

原始类型是一些简单值，Move 内置了许多原始类型，他们是构成所有其他类型的基础。这些原始类型包括：

- 布尔值
- 无符号整数
- 地址

不过，在讨论类型之前，我们首先看看如何在 Move 中声明和赋值变量。

## 变量和赋值

变量使用 let 关键字声明。默认情况下它们是不可变的，但可以使用 let mut 关键字使其可变。let mut 语句的语法如下：

```
let <变量名>[: <类型>] = <表达式>;
let mut <变量名>[: <类型>] = <表达式>;
```

其中：

```
<变量名> - 变量的名称
<类型> - 变量的类型，可选的
<表达式> - 要分配给变量的值
```

```move
let x: bool = true;
let mut y: u8 = 42;
```

可变变量可以使用 = 运算符重新赋值。

```move
y = 43;
```

变量还可以通过重新声明进行隐藏。

```move
let x: u8 = 42;
let x: u16 = 42;
```

## 布尔类型

bool 类型表示布尔值 - 是或否、真或假。它有两个可能的值：true 和 false，它们在 Move 中是关键字。对于布尔值，无需显式指定类型 - 编译器可以从值中推断出来。

```move
let x = true;
let y = false;
```

布尔类型经常用于存储标志和控制程序的流程。更多信息，请参阅[控制流](./control-flow.md)部分。

## 整数类型

Move 支持各种大小的无符号整数：从 8 位到 256 位。整数类型包括：

- u8 - 8 位
- u16 - 16 位
- u32 - 32 位
- u64 - 64 位
- u128 - 128 位
- u256 - 256 位

```move
let x: u8 = 42;
let y: u16 = 42;
// ...
let z: u256 = 42;
```

与布尔类型不同，整数类型需要推断。在大多数情况下，编译器将从值中推断出类型，通常默认为 u64。但有时编译器无法推断类型，将需要显式的类型标注。可以在赋值时提供或使用类型后缀。

```move
// Both are equivalent
let x: u8 = 42;
let x = 42u8;
```

## 运算

Move 支持整数的标准算术运算：加法、减法、乘法、除法和取余。这些操作的语法如下：

| Syntax | Operation           | Aborts If                                |
| ------ | ------------------- | ---------------------------------------- |
| +      | addition            | Result is too large for the integer type |
| -      | subtraction         | Result is less than zero                 |
| \*     | multiplication      | Result is too large for the integer type |
| %      | modular division    | The divisor is 0                         |
| /      | truncating division | The divisor is 0                         |

> For more operations, including bitwise operations, please refer to the
> [Move Reference](/reference/primitive-types/integers.html#bitwise).

操作数的类型必须匹配，否则编译器将报错。操作的结果将与操作数相同类型。要对不同类型执行操作，需要将操作数转换为相同类型。

## 使用 as 进行类型转换

Move 支持整数类型之间的显式转换。其语法为：

```move
<expression> as <type>
```

注意，可能需要在表达式周围加上括号以避免歧义。

```move
let x: u8 = 42;
let y: u16 = x as u16;
let z = 2 * (x as u16); // ambiguous, requires parentheses
```

一个更复杂的例子，防止溢出：

```move
let x: u8 = 255;
let y: u8 = 255;
let z: u16 = (x as u16) + ((y as u16) * 2);
```

### 溢出

Move 不支持溢出/下溢，导致值超出类型范围的操作将引发运行时错误。这是为了防止意外行为的安全特性。

```move
let x = 255u8;
let y = 1u8;

// This will raise an error
let z = x + y;
```
