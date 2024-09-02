# 地址

地址是区块链上位置的唯一标识符。它用于识别包、帐户和对象。地址的大小固定为 32 字节，通常表示为前缀为 0x 的十六进制字符串。地址不区分大小写。

```move
0xe51ff5cd221a81c3d6e22b9e670ddf99004d71de4f769b0312b68c7c4872e2f1
```

上面的地址是有效地址的示例。它的长度为 64 个字符（32 个字节），并以 `0x` 为前缀。

Rooch 还保留了用于标识标准包和对象的地址。保留地址通常是易于记忆和输入的简单值。例如，标准库的地址是0x1。小于 32 字节的地址在左侧用零填充。

```move
0x1 = 0x0000000000000000000000000000000000000000000000000000000000000001
```

以下是保留地址的一些示例：

- `0x1` - Move 标准库的地址（别名 `std`）
- `0x2` - MoveOS 标准库的地址（别名 `moveos_std`）
- `0x3` - Rooch框架的地址（别名`rooch_framework`）

# Address

Address is a unique identifier of a location on the blockchain. It is used to identify
[packages](./packages.md), [accounts](./what-is-an-account.md), and [objects](./object-storage.md).
Address has a fixed size of 32 bytes and is usually represented as a hexadecimal string prefixed
with `0x`. Addresses are case insensitive.

```move
0xe51ff5cd221a81c3d6e22b9e670ddf99004d71de4f769b0312b68c7c4872e2f1
```

The address above is an example of a valid address. It is 64 characters long (32 bytes) and prefixed
with `0x`.

Sui also has reserved addresses that are used to identify standard packages and objects. Reserved
addresses are typically simple values that are easy to remember and type. For example, the address
of the Standard Library is `0x1`. Addresses, shorter than 32 bytes, are padded with zeros to the
left.

```move
0x1 = 0x0000000000000000000000000000000000000000000000000000000000000001
```

Here are some examples of reserved addresses:

- `0x1` - address of the Move Standard Library (alias `std`)
- `0x2` - address of the MoveOS Standard Library (alias `moveos_std`)
- `0x3` - address of the Rooch Framework (alias `rooch_framework`)

