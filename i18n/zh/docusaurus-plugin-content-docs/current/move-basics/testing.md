# Testing

任何软件开发（尤其是区块链开发）的一个关键部分是测试。在这里，我们将介绍 Move 测试的基础知识以及如何为 Move 代码编写和组织测试。

A crucial part of any software development, and even more - blockchain development, is testing.
Here, we will cover the basics of testing in Move and how to write and organize tests for your Move
code.

## The `#[test]` attribute

Move 中的测试是用 #[test] 属性标记的函数。该属性告诉编译器该函数是一个测试函数，并且应该在执行测试时运行它。测试函数是常规函数，但它们不能带参数并且没有返回值。它们被排除在字节码之外，并且永远不会发布。

Tests in Move are functions marked with the `#[test]` attribute. This attribute tells the compiler
that the function is a test function, and it should be run when the tests are executed. Test
functions are regular functions, but they must take no arguments and have no return value. They
are excluded from the bytecode, and are never published.

```move
module book::testing {
    // Test attribute is placed before the `fun` keyword. Can be both above or
    // right before the `fun` keyword: `#[test] fun my_test() { ... }`
    // The name of the test would be `book::testing::simple_test`.
    #[test]
    fun simple_test() {
        let sum = 2 + 2;
        assert!(sum == 4, 1);
    }

    // The name of the test would be `book::testing::more_advanced_test`.
    #[test] fun more_advanced_test() {
        let sum = 2 + 2 + 2;
        assert!(sum == 4, 1);
    }
}
```

## Running Tests

要运行测试，您可以使用 rooch move test 命令。此命令将首先在测试模式下构建包，然后运行包中找到的所有测试。在测试模式期间，将处理sources/ 和tests/ 目录中的模块，并执行测试。

To run tests, you can use the `rooch move test` command. This command will first build the package in
the _test mode_ and then run all the tests found in the package. During test mode, modules from both
`sources/` and `tests/` directories are processed, and the tests are executed.

```bash
$ rooch move test
INCLUDING DEPENDENCY MoveStdlib
INCLUDING DEPENDENCY MoveosStdlib
INCLUDING DEPENDENCY RoochFramework
BUILDING quick_start_object_counter
Running Move unit tests
...
```

## Test Fail Cases with `#[expected_failure]`

失败案例的测试可以用 #[expected_failure] 标记。放置在 #[test] 函数上的此属性告诉编译器测试预计会失败。当您想要测试函数在满足特定条件时是否失败时，这非常有用。

该属性只能放置在 #[test] 函数上。

该属性可以采用中止代码参数，这是测试失败时预期的中止代码。如果测试因不同的中止代码而失败，则测试将失败。如果执行没有中止，测试也将失败。

Tests for fail cases can be marked with `#[expected_failure]`. This attribute placed on a `#[test]`
function tells the compiler that the test is expected to fail. This is useful when you want to test
that a function fails when a certain condition is met.

> This attribute can only be placed on a `#[test]` function.

The attribute can take an argument for abort code, which is the expected abort code when the test
fails. If the test fails with a different abort code, the test will fail. If the execution did not
abort, the test will also fail.

```move
module book::testing_failure {

    const EInvalidArgument: u64 = 1;

    #[test]
    #[expected_failure(abort_code = 0)]
    fun test_fail() {
        abort 0 // aborts with 0
    }

    // attributes can be grouped together
    #[test, expected_failure(abort_code = EInvalidArgument)]
    fun test_fail_1() {
        abort 1 // aborts with 1
    }
}
```

abort_code 参数可以使用测试模块中定义的常量以及从其他模块导入的常量。这是可以在其他模块中使用和“访问”常量的唯一情况。

The `abort_code` argument can use constants defined in the tests module as well as imported from
other modules. This is the only case where constants can be used and "accessed" in other modules.

## Utilities with `#[test_only]`

在某些情况下，让测试环境访问某些内部功能或特性是有帮助的。它简化了测试过程并允许更彻底的测试。然而，重要的是要记住这些函数不应该包含在最终的包中。这就是 #[test_only] 属性派上用场的地方。

In some cases, it is helpful to give the test environment access to some of the internal functions
or features. It simplifies the testing process and allows for more thorough testing. However, it is
important to remember that these functions should not be included in the final package. This is
where the `#[test_only]` attribute comes in handy.

```move
module book::testing {
    // Public function which uses the `secret` function.
    public fun multiply_by_secret(x: u64): u64 {
        x * secret()
    }

    /// Private function which is not available to the public.
    fun secret(): u64 { 100 }

    #[test_only]
    /// This function is only available for testing purposes in tests and other
    /// test-only functions. Mind the visibility - for `#[test_only]` it is
    /// common to use `public` visibility.
    public fun secret_for_testing(): u64 {
        secret()
    }

    #[test]
    // In the test environment we have access to the `secret_for_testing` function.
    fun test_multiply_by_secret() {
        let expected = secret_for_testing() * 2;
        assert!(multiply_by_secret(2) == expected, 1);
    }
}
```

标有 #[test_only] 的函数将可用于测试环境，并且可用于其他模块（如果其可见性如此设置）。

Functions marked with the `#[test_only]` will be available to the test environment, and to the other
modules if their visibility is set so.

## Further Reading

- [Unit Testing](https://move-book.com/reference/unit-testing.html) in the Move Reference.
