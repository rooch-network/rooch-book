# 地址类型

在Move中，为了表示地址，使用了一种特殊的类型称为address。它是一个32字节的值，用于表示区块链上的任何地址。地址有两种语法形式：以0x为前缀的十六进制地址和命名地址。

```move
// address literal
let value: address = @0x1;

// named address registered in Move.toml
let value = @std;
let other = @rooch;
```

地址字面量以@符号开头，后面跟着一个十六进制数字或标识符。十六进制数字被解释为一个32字节的值。编译器将在Move.toml文件中查找该标识符，并将其替换为相应的地址。如果在Move.toml文件中找不到该标识符，编译器将抛出错误。

## 转换

Rooch 框架提供了一组辅助函数来处理地址。由于地址类型是一个32字节的值，可以将其转换为u256类型，反之亦然。它还可以转换为 `vector<u8>` 类型和从 `vector<u8>` 类型转换回地址类型。

示例：将地址转换为u256类型，然后再转换回来。

```move
use rooch::address;

let addr_as_u256: u256 = address::to_u256(@0x1);
let addr = address::from_u256(addr_as_u256);
```

示例：将地址转换为 `vector<u8>` 类型，然后再转换回来。

```move
use rooch::address;

let addr_as_u8: vector<u8> = address::to_bytes(@0x1);
let addr = address::from_bytes(addr_as_u8);
```

示例：将地址转换为字符串。

```mvoe
use rooch::address;
use std::string::String;

let addr_as_string: String = address::to_string(@0x1);
```
