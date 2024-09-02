# Package

Move 是一种用于编写智能合约的语言 - 在区块链上存储和运行的程序。单个程序被组织成一个包。包发布在区块链上并由地址标识。可以通过发送调用其函数的事务来与已发布的包进行交互。它还可以充当其他包的依赖项。

> 要创建新包，请使用 rooch move new 命令。要了解有关该命令的更多信息，请运行 rooch move new --help。

包由模块组成 - 包含函数、类型和其他项目的单独范围。

Move is a language for writing smart contracts - programs that are stored and run on the blockchain. A
single program is organized into a package. A package is published on the blockchain and is
identified by an [address](./address.md). A published package can be interacted with by sending
[transactions](./what-is-a-transaction.md) calling its functions. It can also act as a dependency
for other packages.

> To create a new package, use the `rooch move new` command. To learn more about the command, run
> `rooch move new --help`.

Package consists of modules - separate scopes that contain functions, types, and other items.

```
package 0x...
    module a
        struct A1
        fun hello_world()
    module b
        struct B1
        fun hello_package()
```

## Package Structure

在本地，包是一个包含 Move.toml 文件和源目录的目录。 Move.toml 文件（称为“包清单”）包含有关包的元数据，sources 目录包含模块的源代码。包通常看起来像这样：

Locally, a package is a directory with a `Move.toml` file and a `sources` directory. The `Move.toml`
file - called the "package manifest" - contains metadata about the package, and the `sources`
directory contains the source code for the modules. Package usually looks like this:

```
sources/
    my_module.move
    another_module.move
    ...
tests/
    ...
examples/
    using_my_module.move
Move.toml
```

测试目录是可选的，包含包的测试。放置到测试目录中的代码不会在链上发布，并且仅在测试中可用。示例目录可用于代码示例，并且也不发布到链上。

The `tests` directory is optional and contains tests for the package. Code placed into the `tests`
directory is not published on-chain and is only availably in tests. The `examples` directory can be
used for code examples, and is also not published on-chain.

## Published Package

在开发过程中，包没有地址，需要设置为0x0。一旦发布包，它就会在区块链上获得一个唯一的地址，其中包含其模块的字节码。已发布的包变得不可变，并且可以通过发送事务进行交互。

During development, package doesn't have an address and it needs to be set to `0x0`. Once a package
is published, it gets a single unique [address](./address.md) on the blockchain containing its
modules' bytecode. A published package becomes _immutable_ and can be interacted with by sending
transactions.

```
0x...
    my_module: <bytecode>
    another_module: <bytecode>
```

## Links

- [Package Manifest](./manifest.md)
- [Address](./address.md)
