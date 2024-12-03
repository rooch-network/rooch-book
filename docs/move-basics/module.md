# 模块

模块是 Move 中代码组织的基本单元。模块用于分组和隔离代码，默认情况下模块的所有成员都是模块私有的。在本节中，您将学习如何定义模块、如何声明其成员以及如何从其他模块访问它们。

## 模块声明

模块使用 `module` 关键字声明，后跟包地址，模块名称，花括号和模块主体。模块名称应采用蛇形命名法（`snake_case`）来方式，即所有字母小写，单词之间用下划线（`_`）分隔。模块名称在包中必须是唯一的。

通常，`sources` 文件夹中的单个文件包含单个模块。文件名应与模块名称匹配 - 例如，`donut_shop` 模块应存储在 `donut_shop.move` 文件中。你可以在编码约定部分阅读有关编码约定的更多信息。

```move
module book::my_module {
    // module body
}
```

模块的成员包括结构体、函数、常量和导入：

- TODO
- 结构体
- 函数
- 常量
- 导入
- 结构体方法

## 地址与命名地址

模块地址可以指定为地址*字面量*（不需要 `@` 前缀）或在包清单中指定的命名地址。在下面的示例中，两者是相同的，因为在 `Move.toml` 的 `[addresses]` 部分有一个 `book = "0x0"` 记录。

```move
module 0x0::address_literal { /* ... */ }
module book::named_address { /* ... */ }
```

在 `Move.toml` 中的地址部分：

```move
# Move.toml
[addresses]
book = "0x0"
```

## 模块成员

模块成员声明在模块体内部。为了说明这一点，让我们定义一个简单的模块，其中包含一个结构体、一个函数和一个常量：

```move
module book::my_module_with_members {
    // import
    use book::my_module;

    // a constant
    const CONST: u8 = 0;

    // a struct
    public struct Struct {}

    // method alias
    public use fun function as Struct.struct_fun;

    // function
    fun function(_: &Struct) { /* function body */ }
}
```
