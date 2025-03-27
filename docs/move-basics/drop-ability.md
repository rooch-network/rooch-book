# Abilities: Drop

The `drop` ability allows an instance of a struct to be _ignored_ or _discarded_. In many programming languages, this behavior is the default. However, in Move, a struct without the `drop` ability cannot be ignored. This is a safety feature that ensures all assets are properly handled. Ignoring a struct without the `drop` ability results in a compilation error.

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
    fun test_ignore() {
        let no_drop = NoDrop {};
        let _ = IgnoreMe { a: 1, b: 2 }; // no need to unpack

        // The value must be unpacked for the code to compile.
        let NoDrop {} = no_drop; // OK
    }
}
```

The `drop` ability is often used in custom collection types to eliminate the need for special handling when the collection is no longer needed. For example, the `vector` type has the `drop` ability, allowing it to be ignored when no longer needed. However, the most significant feature of Move's type system is the ability to not have `drop`, ensuring assets are properly handled and not ignored.

A struct with only the `drop` ability is called a _Witness_. The concept of a _Witness_ is explained in the Witness and Abstract Implementation section.

## Types with the `drop` Ability

All native types in Move have the `drop` ability, including:

- [bool](./primitive-types.md#booleans)
- [unsigned integers](./primitive-types.md#integers)
- [vector](./vector.md)
- [address](./address.md)

All types defined in the standard library also have the `drop` ability, such as:

- [Option]()
- [String](./string.md)
- [TypeName]()
