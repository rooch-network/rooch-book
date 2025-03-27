# Primitive Types

Move has several built-in primitive types that form the foundation for all other types. The primitive types are:

- [Booleans](#booleans)
- [Unsigned Integers](#integer-types)
- [Address](./address.md) - covered in the next section

Before diving into the types, let's first look at how to declare and assign variables in Move.

## Variables and Assignment

Variables are declared using the `let` keyword. By default, they are immutable but can be made mutable using the `let mut` keyword. The syntax for these statements is:

```
let <variable_name>[: <type>] = <expression>;
let mut <variable_name>[: <type>] = <expression>;
```

Where:

- `<variable_name>` - the name of the variable
- `<type>` - the type of the variable (optional)
- `<expression>` - the value to be assigned to the variable

```move
let x: bool = true;
let mut y: u8 = 42;
```

A mutable variable can be reassigned using the `=` operator:

```move
y = 43;
```

Variables can also be shadowed by re-declaring them:

```move
let x: u8 = 42;
let x: u16 = 42;
```

## Booleans

The `bool` type represents a boolean value (`true` or `false`). The compiler can infer the type from the value, so there's no need to explicitly specify it.

```move
let x = true;
let y = false;
```

Booleans are often used to store flags and control the flow of the program. Refer to the [Control Flow](./control-flow.md) section for more information.

## Integer Types

Move supports unsigned integers of various sizes: from 8-bit to 256-bit. The integer types are:

- `u8` - 8-bit
- `u16` - 16-bit
- `u32` - 32-bit
- `u64` - 64-bit
- `u128` - 128-bit
- `u256` - 256-bit

```move
let x: u8 = 42;
let y: u16 = 42;
// ...
let z: u256 = 42;
```

Unlike booleans, integer types need to be inferred. The compiler usually defaults to `u64`, but sometimes an explicit type annotation is required. This can be done during assignment or by using a type suffix.

```move
// Both are equivalent
let x: u8 = 42;
let x = 42u8;
```

### Operations

Move supports standard arithmetic operations for integers: addition, subtraction, multiplication, division, and remainder. The syntax for these operations is:

| Syntax | Operation           | Aborts If                                |
| ------ | ------------------- | ---------------------------------------- |
| +      | addition            | Result is too large for the integer type |
| -      | subtraction         | Result is less than zero                 |
| \*     | multiplication      | Result is too large for the integer type |
| %      | modular division    | The divisor is 0                         |
| /      | truncating division | The divisor is 0                         |

> For more operations, including bitwise operations, refer to the [Move Reference](/reference/primitive-types/integers.html#bitwise).

The type of the operands must match, otherwise, the compiler will raise an error. The result of the operation will be of the same type as the operands. To perform operations on different types, the operands need to be cast to the same type.

### Casting with `as`

Move supports explicit casting between integer types. The syntax is:

```move
<expression> as <type>
```

Parentheses may be required around the expression to prevent ambiguity.

```move
let x: u8 = 42;
let y: u16 = x as u16;
let z = 2 * (x as u16); // ambiguous, requires parentheses
```

A more complex example, preventing overflow:

```move
let x: u8 = 255;
let y: u8 = 255;
let z: u16 = (x as u16) + ((y as u16) * 2);
```

### Overflow

Move does not support overflow/underflow. An operation that results in a value outside the range of the type will raise a runtime error. This is a safety feature to prevent unexpected behavior.

```move
let x = 255u8;
let y = 1u8;

// This will raise an error
let z = x + y;
```
