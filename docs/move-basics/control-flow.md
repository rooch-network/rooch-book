# Control Flow

Control flow statements are used to control the flow of execution in a program. They help in making decisions, repeating a block of code, and exiting a block of code early. Move has the following control flow statements:

- [`if` and `if-else`](#conditional-statements) - making decisions on whether to execute a block of code
- [`loop` and `while` loops](#repeating-statements-with-loops) - repeating a block of code
- [`break` and `continue` statements`](#exiting-a-loop-early) - exiting a loop early
- [`return`](#early-return) statement - exiting a function early

## Conditional Statements

The `if` expression is used to make decisions in a program. It evaluates a [boolean expression](./expression.md#literals) and executes a block of code if the expression is true. Paired with `else`, it can execute a different block of code if the expression is false.

The syntax for the `if` expression is:

```move
if (<bool_expression>) <expression>;
if (<bool_expression>) <expression> else <expression>;
```

Just like any other expression, `if` requires a semicolon if there are other expressions following it. The `else` keyword is optional, except when the resulting value is assigned to a variable.

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

Let's see how we can use `if` and `else` to assign a value to a variable:

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

Here, we assign the value of the `if` expression to the variable `y`. If `x` is greater than 0, `y` will be assigned the value 1, otherwise 0. The `else` block is necessary because both branches must return a value of the same type. If we omit the `else` block, the compiler will throw an error.

Conditional expressions are one of the most important control flow statements in Move. They can use either user-provided input or some already stored data to make decisions. In particular, they are used in the [`assert!` macro](./assert-and-abort.md) to check if a condition is true, and if not, to abort execution.

## Repeating Statements with Loops

Loops are used to execute a block of code multiple times. Move has two built-in types of loops: `loop` and `while`. In many cases, they can be used interchangeably, but usually `while` is used when the number of iterations is known in advance, and `loop` is used when the number of iterations is not known in advance or there are multiple exit points.

Loops are helpful when dealing with collections, such as vectors, or when we want to repeat a block of code until a certain condition is met. However, it is important to be careful with loops, as they can lead to infinite loops, which can lead to gas exhaustion and the transaction being aborted.

### The `while` loop

The `while` statement is used to execute a block of code as long as a boolean expression is true. Just like with `if`, the boolean expression is evaluated before each iteration of the loop. The `while` loop is an expression and requires a semicolon if there are other expressions following it.

The syntax for the `while` loop is:

```move
while (<bool_expression>) { <expressions>; };
```

Here is an example of a `while` loop with a simple condition:

```move
// This function iterates over the `x` variable until it reaches 10.
// The return value is the number of iterations it took to reach 10.
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

### Infinite `loop`

An infinite `while`, or `while` without a condition, is a `loop`. The syntax for it is simple:

```move
loop { <expressions>; };
```

Let's rewrite the previous example using `loop` instead of `while`:

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

Infinite loops on their own are not very useful in Move, since every operation in Move costs gas, and an infinite loop will lead to gas exhaustion. However, they can be used in combination with `break` and `continue` statements to create more complex loops.

## Exiting a Loop Early

As mentioned, infinite loops are rather useless on their own. This is where the `break` and `continue` statements come in. They are used to exit a loop early and to skip the rest of the current iteration, respectively.

### The `break` statement

The `break` statement is used to stop the execution of a loop and exit it early. It is often used in combination with a conditional statement to exit the loop when a certain condition is met.

Syntax for the `break` statement is (without a semicolon):

```move
break
```

To illustrate this point, let's turn the infinite `loop` from the previous example into something that behaves more like a `while` loop:

```move
#[test]
fun test_break() {
    let mut x = 0;

    loop {
        if (x == 5) {
            break;
        }
        x = x + 1;
    };

    assert!(x == 5, 0);
}
```

### The `continue` statement

The `continue` statement is used to skip the rest of the current iteration and start the next one. Similarly to `break`, it is used in combination with a conditional statement to skip the rest of the iteration when a certain condition is met.

Syntax for the `continue` statement is (without a semicolon):

```move
continue
```

The example below skips odd numbers and prints only even numbers from 0 to 10:

```move
#[test]
fun test_continue() {
    let mut x = 0;

    while (x < 10) {
        x = x + 1;
        if (x % 2 != 0) {
            continue;
        }
        std::debug::print(&x.to_string());
    };
}
```

`break` and `continue` statements can be used in both `while` and `loop` loops.

## Early Return

The `return` statement is used to exit a [function](./function.md) early and return a value. It is often used in combination with a conditional statement to exit the function when a certain condition is met. The syntax for the `return` statement is:

```move
return <expression>
```

Here is an example of a function that returns a value when a certain condition is met:

```move
fun early_return(x: u8): u8 {
    if (x > 10) {
        return x;
    }
    x + 1
}

#[test]
fun test_early_return() {
    assert!(early_return(5) == 6, 0);
    assert!(early_return(15) == 15, 0);
}
```

Unlike in other languages, the `return` statement is not required for the last expression in a function. The last expression in a function block is automatically returned. However, the `return` statement is useful when we want to exit a function early if a certain condition is met.
