# 包

Move 是一种用于编写智能合约的语言，这些合约被存储和运行在区块链上。一个程序通常被组织成一个包（package）。包发布在区块链上并由地址标识。已发布的包可以通过发送交易来调用其函数进行交互。它还可以作为其他包的依赖项。

> 要创建新包，请使用 rooch move new 命令。要了解有关该命令的更多信息，请运行 rooch move new --help。

包由模块组成 - 包含函数、类型和其他项的单独作用域。

```
package 0x...
    module a
        struct A1
        fun hello_world()
    module b
        struct B1
        fun hello_package()
```

## 包的结构

在本地，包是一个包含 `Move.toml` 文件和 `sources` 目录的目录。`Move.toml` 文件（称为“包清单”）包含有关包的元数据，`sources` 目录包含模块的源代码。Rooch 中 Move 包的结构：

```
$ tree -F
./
├── Move.toml
└── sources/
```

## 已发布的包

在开发过程中，包没有地址，需要设置为 `0x0`。一旦发布包，它就会在区块链上获得一个唯一的地址，其中包含其模块的字节码。已发布的包变得*不可变*，并且可以通过发送交易进行交互。

```
0x...
    my_module: <bytecode>
    another_module: <bytecode>
```

## 相关链接

- [包清单](./manifest.md)
- [地址](./address.md)
