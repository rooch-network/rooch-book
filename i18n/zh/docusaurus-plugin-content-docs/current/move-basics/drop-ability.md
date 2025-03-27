# Abilities: Drop

删除功能 - 其中最简单的 - 允许忽略或丢弃结构的实例。在许多编程语言中，这种行为被视为默认行为。然而，在Move中，不具备drop能力的结构体是不允许被忽略的。这是 Move 语言的一项安全功能，可确保所有资产得到正确处理。尝试忽略没有删除功能的结构将导致编译错误。

```move
module book::drop_ability {

    /// This struct has the `drop` ability.
    public struct IgnoreMe has drop {
        a: u8,
        b: u8,
    }

    /// This struct does not have the `drop` ability.
    public struct NoDrop {}

    #[test]
    // Create an instance of the `IgnoreMe` struct and ignore it.
    // Even though we constructed the instance, we don't need to unpack it.
    fun test_ignore() {
        let no_drop = NoDrop {};
        let _ = IgnoreMe { a: 1, b: 2 }; // no need to unpack

        // The value must be unpacked for the code to compile.
        let NoDrop {} = no_drop; // OK
    }
}
```

删除功能通常用于自定义集合类型，以便在不再需要集合时无需对其进行特殊处理。例如，向量类型具有丢弃能力，允许在不再需要向量时将其忽略。然而，Move的类型系统最大的特点就是能够不掉落。这可确保资产得到正确处理而不是被忽视。

具有单一掉落能力的结构称为见证人。我们在见证和抽象实现部分解释了见证的概念。

## Types with the `drop` Ability

Move 中的所有原生类型都具有放置能力。这包括：

- [bool](./primitive-type.md#booleans)
- [unsigned integers](./primitive-type.md#integers)
- [vector](./vector.md)
- [address](./address.md)

标准库中定义的所有类型也都具有删除功能。这包括：

- Option
- [String](./string.md)
- TypeName

