# 控制流

控制流语句用于控制程序执行的流程。它们用于做出决策，重复执行代码块，以及提前退出代码块。Move 语言支持以下控制流语句（将在下文详细解释）：

- if 和 if-else - 根据条件决定是否执行一段代码
- loop 和 while 循环 - 重复执行一段代码
- break 和 continue 语句 - 提前退出循环
- return 语句 - 提前退出函数

## 条件语句

if 表达式用于在程序中做出决策。它会计算一个 布尔表达式，如果表达式为真，则执行一段代码块。配合 else 使用，则可以在表达式为假的情况下执行另一段代码块。

if 表达式的语法如下：

```move
if (<bool_expression>) <expression>;
if (<bool_expression>) <expression> else <expression>;
```

与任何其他表达式一样，如果后面还有其他表达式，if 也需要一个分号。else 关键字可选，除非结果值需要赋值给变量。稍后我们将介绍这种情况。

```move
    #[test]
    fun test_if() {
        let x = 5;

        // `x > 0` is a boolean expression.
        if (x > 0) {
            std::debug::print(&b"X is bigger than 0".to_string())
        };
    }
```

让我们看看如何使用 if 和 else 将值赋给变量：

```move
    #[test]
    fun test_if_else() {
        let x = 5;
        let y = if (x > 0) {
            1
        } else {
            0
        };

        assert!(y == 1, 0);
    }
```

这里我们将 if 表达式的值赋给变量 y。如果 x 大于 0，则将值 1 赋给 y，否则为 0。else 块是必需的，因为这两个分支都必须返回相同类型的返回值。如果省略 else 块，编译器会抛出一个错误。

条件表达式是 Move 中最重要的控制流语句之一。它们可以利用用户提供的输入或一些已经存储的数据来做出决策。特别地，它们用于 assert! 宏 中检查条件是否成立，如果不成立则中止执行。我们很快就会讲到这一点！

## 使用循环重复语句

循环用于多次执行一段代码块。Move 具有两种内置的循环类型：loop 和 while。在许多情况下它们可以互换使用，但是通常情况下，当迭代次数提前知道时使用 while 循环，当迭代次数未知或者有多个退出点时使用 loop 循环。

循环在处理集合（例如向量）或在满足某个条件之前重复执行代码块时非常有用。但是，使用循环需要注意，因为它们可能导致无限循环，从而耗尽 Gas 并导致交易中止。


## while 循环

while 语句用于只要布尔表达式为真就执行一段代码块。就像我们在 if 语句中看到的一样，布尔表达式会在循环的每次迭代之前进行计算。与条件语句一样，while 循环也是一个表达式，如果后面还有其他表达式，则需要一个分号。

while 循环的语法如下：

```move
while (<bool_expression>) { <expressions>; };
```

这是一个条件非常简单的 while 循环示例：

```move
    // This function iterates over the `x` variable until it reaches 10, the
    // return value is the number of iterations it took to reach 10.
    //
    // If `x` is 0, then the function will return 10.
    // If `x` is 5, then the function will return 5.
    fun while_loop(mut x: u8): u8 {
        let mut y = 0;

        // This will loop until `x` is 10.
        // And will never run if `x` is 10 or more.
        while (x < 10) {
            y = y + 1;
            x = x + 1;
        };

        y
    }

    #[test]
    fun test_while() {
        assert!(while_loop(0) == 10, 0); // 10 times
        assert!(while_loop(5) == 5, 0);  // 5 times
        assert!(while_loop(10) == 0, 0); // loop never executed
    }
```

## 无限 loop

现在让我们想象一个布尔表达式始终为真的场景。例如，如果我们直接将 true 传递给 while 条件。正如你所料，这将创建一个无限循环，这几乎就是 loop 语句的工作方式。


```move
    #[test, expected_failure(out_of_gas, location=Self)]
    fun test_infinite_while() {
        let mut x = 0;

        // This will loop forever.
        while (true) {
            x = x + 1;
        };

        // This line will never be executed.
        assert!(x == 5, 0);
    }
```

一个无限的 while 循环，或者没有条件的 while 循环，就是一个 loop 循环。它的语法很简单：

```move
loop { <expressions>; };
```

让我们用 loop 而不是 while 重写前面的例子：

```move
    #[test, expected_failure(out_of_gas, location=Self)]
    fun test_infinite_loop() {
        let mut x = 0;

        // This will loop forever.
        loop {
            x = x + 1;
        };

        // This line will never be executed.
        assert!(x == 5, 0);
    }
```

无限循环本身在 Move 中并不是非常有用，因为 Move 中的每个操作都需要 Gas，无限循环会导致 Gas 耗尽。但是，它们可以与 break 和 continue 语句结合使用来创建更复杂的循环。
