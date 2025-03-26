# Abilities: Introduction

Move has a unique type system that allows customizing _type abilities_.
[In the previous section](./struct.md), we introduced the `struct` definition and how to use it.
However, the instances of the `Artist` and `Record` structs had to be unpacked for the code to compile. This is the default behavior of a struct without _abilities_.

> Throughout the book, you will see chapters named `Ability: <name>`, where `<name>` is the name of the ability. These chapters will cover the ability in detail, how it works, and how to use it in Move.

## What are Abilities?

Abilities allow certain behaviors for a type. They are part of the struct declaration and define which behaviors are allowed for the instances of the struct.

## Abilities Syntax

Abilities are set in the struct definition using the `has` keyword followed by a list of abilities. The abilities are separated by commas. Move supports 4 abilities: `copy`, `drop`, `key`, and `store`, each defining a specific behavior for the struct instances.

```move
/// This struct has the `copy` and `drop` abilities.
struct VeryAble has copy, drop {
    // field: Type1,
    // field2: Type2,
    // ...
}
```

## Overview

A quick overview of the abilities:

> All built-in types, except references, have `copy`, `drop`, and `store` abilities.
> References have `copy` and `drop`.

- `copy` - allows the struct to be _copied_. Explained in the [Ability: Copy](./copy-ability.md) chapter.
- `drop` - allows the struct to be _dropped_ or _discarded_. Explained in the [Ability: Drop](./drop-ability.md) chapter.
- `key` - allows the struct to be used as a _key_ in storage. Explained in the [Ability: Key](./../storage/key-ability.md) chapter.
- `store` - allows the struct to be _stored_ in structs with the _key_ ability. Explained in the [Ability: Store](./../storage/store-ability.md) chapter.

While it is important to mention them here, we will go into detail about each ability in the following chapters and provide proper context on how to use them.

## No Abilities

A struct without abilities cannot be discarded, copied, or stored in storage. We call such a struct a _Hot Potato_. It is a joke, but it is also a good way to remember that a struct without abilities is like a hot potato - it can only be passed around and requires special handling. Hot Potato is one of the most powerful patterns in Move, and we will go into detail about it in the [Hot Potato](./../programmability/hot-potato.md) chapter.

## Further Reading

- [Type Abilities](/reference/type-abilities.html) in the Move Reference.

## Understanding Move Resource Abilities

In Move, abilities are represented as bit flags. The value 8 in the abilities field corresponds to specific capability flags. Let me break this down:

Ability Bit Flags

```
abilities: 8 means:
1 = copy  (0b0001)
2 = drop  (0b0010)
4 = store (0b0100)
8 = key   (0b1000)
```