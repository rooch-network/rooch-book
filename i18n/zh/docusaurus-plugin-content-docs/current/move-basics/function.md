# Function

函数是 Move 程序的构建块。它们从用户事务和其他函数中调用，并将可执行代码分组为可重用单元。函数可以接受参数并返回一个值。它们是在模块级别使用 fun 关键字声明的。就像任何其他模块成员一样，默认情况下它们是私有的，只能从模块内部访问。

Functions are the building blocks of Move programs. They are called from
[user transactions](../concepts/user-interaction.md) and from other functions and group executable
code into reusable units. Functions can take arguments and return a value. They are declared with
the `fun` keyword at the module level. Just like any other module member, by default they're private
and can only be accessed from within the module.

```move
{{#include ../../../packages/samples/sources/move-basics/function.move:math}}
```

在此示例中，我们定义了一个函数 add，它接受两个 u64 类型的参数并返回它们的和。该函数是从 test_add 函数调用的，该函数是位于同一模块中的测试函数。在测试中，我们将 add 函数的结果与预期值进行比较，如果结果不同则中止执行。

In this example, we define a function `add` that takes two arguments of type `u64` and returns their
sum. The function is called from the `test_add` function, which is a test function located in the
same module. In the test we compare the result of the `add` function with the expected value and
abort the execution if the result is different.

## Function declaration

Move 中的函数调用有一个约定，即使用 Snake_case 命名约定。这意味着函数名称应全部小写，单词之间用下划线分隔。例如，do_something、add、get_balance、is_authorized 等。

> There's a convention to call functions in Move with the `snake_case` naming convention. This means
> that the function name should be all lowercase with words separated by underscores. For example,
> `do_something`, `add`, `get_balance`, `is_authorized`, and so on.

函数使用 fun 关键字声明，后跟函数名称（有效的 Move 标识符）、括号中的参数列表以及返回类型。函数体是包含一系列语句和表达式的代码块。函数体中的最后一个表达式是函数的返回值。

A function is declared with the `fun` keyword followed by the function name (a valid Move
identifier), a list of arguments in parentheses, and a return type. The function body is a block of
code that contains a sequence of statements and expressions. The last expression in the function
body is the return value of the function.

```move
{{#include ../../../packages/samples/sources/move-basics/function.move:return_nothing}}
```

## Accessing functions

就像任何其他模块成员一样，可以通过路径导入和访问函数。路径由模块路径和函数名组成，以 :: 分隔。例如，如果 book 包中的 math 模块中有一个名为 add 的函数，则该函数的路径将为 book::math::add，或者，如果导入该模块，则为 math::add。

Just like any other module member, functions can be imported and accessed via a path. The path
consists of the module path and the function name separated by `::`. For example, if you have a
function called `add` in the `math` module in the `book` package, the path to it will be
`book::math::add`, or, if the module is imported, `math::add`.

```move
{{#include ../../../packages/samples/sources/move-basics/function.move:use_math}}
```

## Multiple return values

移动函数可以返回多个值，当您需要从函数返回多个值时，这非常有用。函数的返回类型是类型的元组。返回值是一个表达式元组。

Move functions can return multiple values, which is useful when you need to return more than one
value from a function. The return type of the function is a tuple of types. The return value is a
tuple of expressions.

```move
{{#include ../../../packages/samples/sources/move-basics/function.move:tuple_return}}
```

带有元组返回的函数调用的结果必须通过 let (tuple) 语法解包到变量中：

Result of a function call with tuple return has to be unpacked into variables via `let (tuple)`
syntax:

```move
{{#include ../../../packages/samples/sources/move-basics/function.move:tuple_return_imm}}
```

如果任何声明的值需要声明为可变的，则将 mut 关键字放在变量名称之前：

If any of the declared values need to be declared as mutable, the `mut` keyword is placed before the
variable name:

```move
{{#include ../../../packages/samples/sources/move-basics/function.move:tuple_return_mut}}
```

如果某些参数未使用，可以使用 _ 符号将其忽略：

If some of the arguments are not used, they can be ignored with the `_` symbol:

```move
{{#include ../../../packages/samples/sources/move-basics/function.move:tuple_return_ignore}}
```

## Further reading

- [Functions](/reference/functions.html) in the Move Reference.
