# Constants

常量是在模块级别定义的不可变值。它们通常用作为整个模块中使用的静态值命名的方法。例如，如果产品有默认价格，您可以为其定义一个常量。常量存储在模块的字节码中，每次使用它们时，都会复制该值。

Constants are immutable values that are defined at the module level. They often serve as a way to
give names to static values that are used throughout a module. For example, if there's a default
price for a product, you might define a constant for it. Constants are stored in the module's
bytecode, and each time they are used, the value is copied.

```move
module book::shop_price {
    use sui::coin::Coin;
    use sui::sui::SUI;

    /// The price of an item in the shop.
    const ITEM_PRICE: u64 = 100;
    /// The owner of the shop, an address.
    const SHOP_OWNER: address = @0xa11ce;

    /// An item sold in the shop.
    public struct Item { /* ... */ }

    /// Purchase an item from the shop.
    public fun purchase(coin: Coin<SUI>): Item {
        assert!(coin.value() == ITEM_PRICE, 0);

        transfer::public_transfer(coin, SHOP_OWNER);

        Item { /* ... */ }
    }
}
```

## Naming Convention

常量必须以大写字母开头 - 这是在编译器级别强制执行的。对于用作值的常量，约定使用大写字母和下划线来分隔单词。这是一种使常量从代码中的其他标识符中脱颖而出的方法。错误常量有一个例外，它是用 ECamelCase 编写的。

Constants must start with a capital letter - this is enforced at the compiler level. For constants
used as a value, there's a convention to use uppercase letters and underscores to separate words.
It's a way to make constants stand out from other identifiers in the code. One exception is made for
[error constants](./assert-and-abort.md#assert-and-abort), which are written in ECamelCase.

```move
/// Price of the item used at the shop.
const ITEM_PRICE: u64 = 100;

/// Error constant.
const EItemNotFound: u64 = 1;
```

## Constants are Immutable

无法更改常量并为其分配新值。它们是包字节码的一部分，本质上是不可变的。

Constants can't be changed and assigned new values. They are part of the package bytecode, and
inherently immutable.

```move
module book::immutable_constants {
    const ITEM_PRICE: u64 = 100;

    // emits an error
    fun change_price() {
        ITEM_PRICE = 200;
    }
}
```

## Using Config Pattern

应用程序的一个常见用例是定义一组在整个代码库中使用的常量。但由于常量是模块私有的，因此无法从其他模块访问它们。解决这个问题的一种方法是定义一个导出常量的“config”模块。

A common use case for an application is to define a set of constants that are used throughout the
codebase. But due to constants being private to the module, they can't be accessed from other
modules. One way to solve this is to define a "config" module that exports the constants.

```move
module book::config {
    const ITEM_PRICE: u64 = 100;
    const TAX_RATE: u64 = 10;
    const SHIPPING_COST: u64 = 5;

    /// Returns the price of an item.
    public fun item_price(): u64 { ITEM_PRICE }
    /// Returns the tax rate.
    public fun tax_rate(): u64 { TAX_RATE }
    /// Returns the shipping cost.
    public fun shipping_cost(): u64 { SHIPPING_COST }
}
```

这样其他模块就可以导入并读取常量，并且简化了更新过程。如果需要更改常量，则在包升级时只需要更新config模块。

This way other modules can import and read the constants, and the update process is simplified. If
the constants need to be changed, only the config module needs to be updated during the package
upgrade.
