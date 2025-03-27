# Abilities: Copy

在 Move 中，类型的复制能力表示该类型的实例或值可以被复制。虽然在处理数字或其他简单类型时这种行为可能感觉非常自然，但它并不是 Move 中自定义类型的默认行为。这是因为Move旨在表达数字资产和资源，而无法复制是资源模型的关键要素。

但是，移动类型系统允许您定义具有复制功能的自定义类型。

```move
public struct Copyable has copy {}
```

在上面的示例中，a 被隐式复制到 b，然后使用解引用运算符显式复制到 c。如果 Copyable 不具备复制能力，则代码将无法编译，并且 Move 编译器将引发错误。

```move
let a = Copyable {};
let b = a;   // `a` is copied to `b`
let c = *&b; // explicit copy via dereference operator

let Copyable {} = a; // doesn't have `drop`
let Copyable {} = b; // doesn't have `drop`
let Copyable {} = c; // doesn't have `drop`
```

复制能力与掉落能力密切相关。如果一个类型具有复制能力，那么它很可能也应该具有复制能力。这是因为当不再需要实例时需要删除能力来清理资源。如果类型只有副本，则管理其实例会变得更加复杂，因为不能忽略值。

## Copying and Drop

复制能力与掉落能力密切相关。如果一个类型具有复制能力，那么它很可能也应该具有复制能力。这是因为当不再需要实例时需要删除能力来清理资源。如果类型只有副本，则管理其实例会变得更加复杂，因为不能忽略值。

```move
public struct Value has copy, drop {}
```

Move 中的所有基本类型的行为就好像它们具有复制和放置功能一样。这意味着它们可以被复制和删除，并且 Move 编译器将为它们处理内存管理。

## Types with the `copy` Ability

Move 中的所有本机类型都具有复制功能。这包括：

- [bool](./primitive-type.md#booleans)
- [unsigned integers](./primitive-type.md#integers)
- [vector](./vector.md)
- [address](./address.md)

标准库中定义的所有类型也都具有复制功能。这包括：

- Option
- [String](./string.md)
- TypeName
