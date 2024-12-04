# String

虽然 Move 没有内置类型来表示字符串，但它在标准库中有两种标准的字符串实现。 std::string 模块定义了 UTF-8 编码字符串的 String 类型和方法，第二个模块 std::ascii 提供了 ASCII String 类型及其方法。

Rooch执行环境自动将交易输入中的字节向量转换为字符串。所以很多情况下，Transaction Block中不需要构造String。

While Move does not have a built-in type to represent strings, it does have two standard
implementations for strings in the [Standard Library](./standard-library.md). The `std::string`
module defines a `String` type and methods for UTF-8 encoded strings, and the second module,
`std::ascii`, provides an ASCII `String` type and its methods.

> Rooch execution environment automatically converts bytevector into `String` in transaction inputs.
> So in many cases, a String does not need to be constructed in the
> [Transaction Block](./../concepts/what-is-a-transaction.md).


## Strings are bytes

无论您使用哪种类型的字符串，重要的是要知道字符串只是字节。 string 和 ascii 模块提供的包装器就是：包装器。它们确实提供了安全检查和处理字符串的方法，但归根结底，它们只是字节向量。

No matter which type of string you use, it is important to know that strings are just bytes. The
wrappers provided by the `string` and `ascii` modules are just that: wrappers. They do provide
safety checks and methods to work with strings, but at the end of the day, they are just vectors of
bytes.

```move
module book::custom_string {
    /// Anyone can implement a custom string-like type by wrapping a vector.
    public struct MyString {
        bytes: vector<u8>,
    }

    /// Implement a `from_bytes` function to convert a vector of bytes to a string.
    public fun from_bytes(bytes: vector<u8>): MyString {
        MyString { bytes }
    }

    /// Implement a `bytes` function to convert a string to a vector of bytes.
    public fun bytes(self: &MyString): &vector<u8> {
        &self.bytes
    }
}
```

## Working with UTF-8 Strings

虽然标准库中有两种类型的字符串，但 string 模块应被视为默认模块。它具有许多常见操作的本机实现，因此比在 Move 中完全实现的 ascii 模块更高效。

While there are two types of strings in the standard library, the `string` module should be
considered the default. It has native implementations of many common operations, and hence is more
efficient than the `ascii` module, which is fully implemented in Move.

### Definition

std::string模块中的String类型定义如下：

The `String` type in the `std::string` module is defined as follows:

```move
// File: move-stdlib/sources/string.move
/// A `String` holds a sequence of bytes which is guaranteed to be in utf8 format.
public struct String has copy, drop, store {
    bytes: vector<u8>,
}
```

### Creating a String

要创建新的 UTF-8 String 实例，可以使用 string::utf8 方法。为了方便起见，标准库在向量 u8 上提供了一个别名 .to_string()。

To create a new UTF-8 `String` instance, you can use the `string::utf8` method. The
[Standard Library](./standard-library.md) provides an alias `.to_string()` on the `vector<u8>` for
convenience.

```move
// the module is `std::string` and the type is `String`
use std::string::{Self, String};

// strings are normally created using the `utf8` function
// type declaration is not necessary, we put it here for clarity
let hello: String = string::utf8(b"Hello");

// The `.to_string()` alias on the `vector<u8>` is more convenient
let hello = b"Hello".to_string();
```

### Common Operations

UTF8 String 提供了许多处理字符串的方法。对字符串最常见的操作是：连接、切片和获取长度。此外，对于自定义字符串操作，可以使用 bytes() 方法来获取底层字节向量。

UTF8 String provides a number of methods to work with strings. The most common operations on strings
are: concatenation, slicing, and getting the length. Additionally, for custom string operations, the
`bytes()` method can be used to get the underlying byte vector.

```move
let mut str = b"Hello,".to_string();
let another = b" World!".to_string();

// append(String) adds the content to the end of the string
str.append(another);

// `sub_string(start, end)` copies a slice of the string
str.sub_string(0, 5); // "Hello"

// `length()` returns the number of bytes in the string
str.length(); // 12 (bytes)

// methods can also be chained! Get a length of a substring
str.sub_string(0, 5).length(); // 5 (bytes)

// whether the string is empty
str.is_empty(); // false

// get the underlying byte vector for custom operations
let bytes: &vector<u8> = str.bytes();
```

### Safe UTF-8 Operations

如果传入的字节不是有效的 UTF-8，默认的 utf8 方法可能会中止。如果您不确定传递的字节是否有效，则应使用 try_utf8 方法。它返回一个 Option String ，如果字节不是有效的 UTF-8，则该字符串不包含任何值，否则返回一个字符串。

提示：以 try_* 开头的名称表示该函数返回一个包含预期结果的 Option，如果操作失败则返回 None。这是从 Rust 借用的常见命名约定。

The default `utf8` method may abort if the bytes passed into it are not valid UTF-8. If you are not
sure that the bytes you are passing are valid, you should use the `try_utf8` method instead. It
returns an `Option<String>`, which contains no value if the bytes are not valid UTF-8, and a string
otherwise.

> Hint: the name that starts with `try_*` indicates that the function returns an Option with the
> expected result or `none` if the operation fails. It is a common naming convention borrowed from
> Rust.

```move
// this is a valid UTF-8 string
let hello = b"Hello".try_to_string();

assert!(hello.is_some(), 0); // abort if the value is not valid UTF-8

// this is not a valid UTF-8 string
let invalid = b"\xFF".try_to_string();

assert!(invalid.is_none(), 0); // abort if the value is valid UTF-8
```

### UTF-8 Limitations

字符串模块不提供访问字符串中各个字符的方法。这是因为 UTF-8 是一种变长编码，字符的长度可以是 1 到 4 个字节之间的任意长度。同样，length() 方法返回字符串中的字节数，而不是字符数。

但是，像 sub_string 和 insert 这样的方法会检查字符边界，并且当索引位于字符中间时会中止。

The `string` module does not provide a way to access individual characters in a string. This is
because UTF-8 is a variable-length encoding, and the length of a character can be anywhere from 1 to
4 bytes. Similarly, the `length()` method returns the number of bytes in the string, not the number
of characters.

However, methods like `sub_string` and `insert` check character boundaries and will abort when the
index is in the middle of a character.

## ASCII Strings

This section is coming soon!
