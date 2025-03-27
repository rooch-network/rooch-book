# Function

函数是 Move 程序的构建块。它们从用户事务和其他函数中调用，并将可执行代码分组为可重用单元。函数可以接受参数并返回一个值。它们是在模块级别使用 fun 关键字声明的。就像任何其他模块成员一样，默认情况下它们是私有的，只能从模块内部访问。

```move
module book::math {
    /// Function takes two arguments of type `u64` and returns their sum.
    /// The `public` visibility modifier makes the function accessible from
    /// outside the module.
    public fun add(a: u64, b: u64): u64 {
        a + b
    }

    #[test]
    fun test_add() {
        let sum = add(1, 2);
        assert!(sum == 3, 0);
    }
}
```

在此示例中，我们定义了一个函数 add，它接受两个 u64 类型的参数并返回它们的和。该函数是从 test_add 函数调用的，该函数是位于同一模块中的测试函数。在测试中，我们将 add 函数的结果与预期值进行比较，如果结果不同则中止执行。

## Function declaration

> Move 中的函数调用有一个约定，即使用 Snake_case 命名约定。这意味着函数名称应全部小写，单词之间用下划线分隔。例如，do_something、add、get_balance、is_authorized 等。

函数使用 fun 关键字声明，后跟函数名称（有效的 Move 标识符）、括号中的参数列表以及返回类型。函数体是包含一系列语句和表达式的代码块。函数体中的最后一个表达式是函数的返回值。

```move
fun return_nothing() {
    // empty expression, function returns `()`
}
```

## Accessing functions

就像任何其他模块成员一样，可以通过路径导入和访问函数。路径由模块路径和函数名组成，以 :: 分隔。例如，如果 book 包中的 math 模块中有一个名为 add 的函数，则该函数的路径将为 book::math::add，或者，如果导入该模块，则为 math::add。

```move
module book::use_math {
    use book::math;

    fun call_add() {
        // function is called via the path
        let sum = math::add(1, 2);
    }
}
```

## Multiple return values

移动函数可以返回多个值，当您需要从函数返回多个值时，这非常有用。函数的返回类型是类型的元组。返回值是一个表达式元组。

```move
fun get_name_and_age(): (vector<u8>, u8) {
    (b"John", 25)
}
```

带有元组返回的函数调用的结果必须通过 let (tuple) 语法解包到变量中：

```move
// Tuple must be destructured to access its elements.
// Name and age are declared as immutable variables.
let (name, age) = get_name_and_age();
assert!(name == b"John", 0);
assert!(age == 25, 0);
```

如果任何声明的值需要声明为可变的，则将 mut 关键字放在变量名称之前：

```move
// declare name as mutable, age as immutable
let (mut name, age) = get_name_and_age();
```

如果某些参数未使用，可以使用 _ 符号将其忽略：

```move
// ignore the name, only use the age
let (_, age) = get_name_and_age();
```
