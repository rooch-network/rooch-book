# Abilities: Copy

在 Move 中，类型的复制能力表示该类型的实例或值可以被复制。虽然在处理数字或其他简单类型时这种行为可能感觉非常自然，但它并不是 Move 中自定义类型的默认行为。这是因为Move旨在表达数字资产和资源，而无法复制是资源模型的关键要素。

但是，移动类型系统允许您定义具有复制功能的自定义类型。

In Move, the _copy_ ability on a type indicates that the instance or the value of the type can be
copied. While this behavior may feel very natural when working with numbers or other simple types,
it is not the default for custom types in Move. This is because Move is designed to express digital
assets and resources, and inability to copy is a key element of the resource model.

However, Move type system allows you to define custom types with the _copy_ ability.

```move
public struct Copyable has copy {}
```

在上面的示例中，a 被隐式复制到 b，然后使用解引用运算符显式复制到 c。如果 Copyable 不具备复制能力，则代码将无法编译，并且 Move 编译器将引发错误。

In the example above, we define a custom type `Copyable` with the _copy_ ability. This means that
instances of `Copyable` can be copied, both implicitly and explicitly.

```move
let a = Copyable {};
let b = a;   // `a` is copied to `b`
let c = *&b; // explicit copy via dereference operator

let Copyable {} = a; // doesn't have `drop`
let Copyable {} = b; // doesn't have `drop`
let Copyable {} = c; // doesn't have `drop`
```

复制能力与掉落能力密切相关。如果一个类型具有复制能力，那么它很可能也应该具有复制能力。这是因为当不再需要实例时需要删除能力来清理资源。如果类型只有副本，则管理其实例会变得更加复杂，因为不能忽略值。

In the example above, `a` is copied to `b` implicitly, and then explicitly copied to `c` using the
dereference operator. If `Copyable` did not have the _copy_ ability, the code would not compile, and
the Move compiler would raise an error.

## Copying and Drop

复制能力与掉落能力密切相关。如果一个类型具有复制能力，那么它很可能也应该具有复制能力。这是因为当不再需要实例时需要删除能力来清理资源。如果类型只有副本，则管理其实例会变得更加复杂，因为不能忽略值。

The `copy` ability is closely related to [`drop` ability](./drop-ability.md). If a type has the
_copy_ ability, very likely that it should have `drop` too. This is because the _drop_ ability is
required to clean up the resources when the instance is no longer needed. If a type has only _copy_,
then managing its instances gets more complicated, as the values cannot be ignored.

```move
public struct Value has copy, drop {}
```

Move 中的所有基本类型的行为就好像它们具有复制和放置功能一样。这意味着它们可以被复制和删除，并且 Move 编译器将为它们处理内存管理。

All of the primitive types in Move behave as if they have the _copy_ and _drop_ abilities. This
means that they can be copied and dropped, and the Move compiler will handle the memory management
for them.

## Types with the `copy` Ability

Move 中的所有本机类型都具有复制功能。这包括：

All native types in Move have the `copy` ability. This includes:

- [bool](./../move-basics/primitive-types.md#booleans)
- [unsigned integers](./../move-basics/primitive-types.md#integers)
- [vector](./../move-basics/vector.md)
- [address](./../move-basics/address.md)

标准库中定义的所有类型也都具有复制功能。这包括：

All of the types defined in the standard library have the `copy` ability as well. This includes:

- [Option](./../move-basics/option.md)
- [String](./../move-basics/string.md)
- [TypeName](./../move-basics/type-reflection.md#typename)
