# Abilities: Copy

In Move, the _copy_ ability on a type indicates that instances or values of the type can be copied. While this behavior may feel natural when working with numbers or other simple types, it is not the default for custom types in Move. This is because Move is designed to express digital assets and resources, and the inability to copy is a key element of the resource model.

However, the Move type system allows you to define custom types with the _copy_ ability.

```move
public struct Copyable has copy {}
```

In the example above, we define a custom type `Copyable` with the _copy_ ability. This means that instances of `Copyable` can be copied, both implicitly and explicitly.

```move
let a = Copyable {};
let b = a;   // `a` is copied to `b`
let c = *&b; // explicit copy via dereference operator

let Copyable {} = a; // doesn't have `drop`
let Copyable {} = b; // doesn't have `drop`
let Copyable {} = c; // doesn't have `drop`
```

In the example above, `a` is copied to `b` implicitly, and then explicitly copied to `c` using the dereference operator. If `Copyable` did not have the _copy_ ability, the code would not compile, and the Move compiler would raise an error.

## Copying and Drop

The `copy` ability is closely related to the [`drop` ability](./drop-ability.md). If a type has the _copy_ ability, it is very likely that it should have the `drop` ability too. This is because the _drop_ ability is required to clean up resources when the instance is no longer needed. If a type has only the _copy_ ability, managing its instances becomes more complicated, as the values cannot be ignored.

```move
public struct Value has copy, drop {}
```

All of the primitive types in Move behave as if they have the _copy_ and _drop_ abilities. This means that they can be copied and dropped, and the Move compiler will handle the memory management for them.

## Types with the `copy` Ability

All native types in Move have the `copy` ability. This includes:

- [bool](./primitive-types.md#booleans)
- [unsigned integers](./primitive-types.md#integers)
- [vector](./vector.md)
- [address](./address.md)

All of the types defined in the standard library have the `copy` ability as well. This includes:

- [Option]()
- [String](./string.md)
- [TypeName]()
