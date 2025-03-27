# String

Move does not have a built-in type to represent strings, but it provides two standard implementations in the Standard Library. The `std::string` module defines a `String` type and methods for UTF-8 encoded strings, while the `std::ascii` module provides an ASCII `String` type and its methods.

> The Rooch execution environment automatically converts bytevectors into `String` in transaction inputs. Therefore, in many cases, a String does not need to be constructed in the Transaction Block.

## Strings are Bytes

Regardless of the string type you use, it's important to understand that strings are just bytes. The `string` and `ascii` modules provide wrappers that include safety checks and methods to work with strings, but ultimately, they are vectors of bytes.

```move
module book::custom_string {
    /// Custom string-like type by wrapping a vector.
    public struct MyString {
        bytes: vector<u8>,
    }

    /// Convert a vector of bytes to a string.
    public fun from_bytes(bytes: vector<u8>): MyString {
        MyString { bytes }
    }

    /// Convert a string to a vector of bytes.
    public fun bytes(self: &MyString): &vector<u8> {
        &self.bytes
    }
}
```

## Working with UTF-8 Strings

The `string` module should be considered the default for working with strings. It has native implementations of many common operations, making it more efficient than the `ascii` module, which is fully implemented in Move.

### Definition

The `String` type in the `std::string` module is defined as follows:

```move
// File: move-stdlib/sources/string.move
/// A `String` holds a sequence of bytes guaranteed to be in UTF-8 format.
public struct String has copy, drop, store {
    bytes: vector<u8>,
}
```

### Creating a String

To create a new UTF-8 `String` instance, use the `string::utf8` method. The Standard Library provides an alias `.to_string()` on the `vector<u8>` for convenience.

```move
use std::string::{Self, String};

// Create a string using the `utf8` function
let hello: String = string::utf8(b"Hello");

// Use the `.to_string()` alias on the `vector<u8>`
let hello = b"Hello".to_string();
```

### Common Operations

UTF-8 `String` provides several methods to work with strings, such as concatenation, slicing, and getting the length. For custom string operations, the `bytes()` method can be used to get the underlying byte vector.

```move
let mut str = b"Hello,".to_string();
let another = b" World!".to_string();

// Append content to the end of the string
str.append(another);

// Copy a slice of the string
str.sub_string(0, 5); // "Hello"

// Get the number of bytes in the string
str.length(); // 12 (bytes)

// Chain methods to get the length of a substring
str.sub_string(0, 5).length(); // 5 (bytes)

// Check if the string is empty
str.is_empty(); // false

// Get the underlying byte vector for custom operations
let bytes: &vector<u8> = str.bytes();
```

### Safe UTF-8 Operations

The default `utf8` method may abort if the bytes passed into it are not valid UTF-8. Use the `try_utf8` method if you are unsure whether the bytes are valid. It returns an `Option<String>`, which contains no value if the bytes are not valid UTF-8, and a string otherwise.

> Hint: Functions starting with `try_*` return an `Option` with the expected result or `none` if the operation fails. This naming convention is borrowed from Rust.

```move
// Valid UTF-8 string
let hello = b"Hello".try_to_string();
assert!(hello.is_some(), 0); // Abort if the value is not valid UTF-8

// Invalid UTF-8 string
let invalid = b"\xFF".try_to_string();
assert!(invalid.is_none(), 0); // Abort if the value is valid UTF-8
```

### UTF-8 Limitations

The `string` module does not provide a way to access individual characters in a string because UTF-8 is a variable-length encoding, and the length of a character can be anywhere from 1 to 4 bytes. The `length()` method returns the number of bytes in the string, not the number of characters.

However, methods like `sub_string` and `insert` check character boundaries and will abort if the index is in the middle of a character.

## ASCII Strings

This section is coming soon!
