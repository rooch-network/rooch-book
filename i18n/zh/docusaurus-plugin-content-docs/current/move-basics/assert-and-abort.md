# Aborting Execution

A transaction can either succeed or fail. Successful execution applies all the changes made to
objects and on-chain data, and the transaction is committed to the blockchain. Alternatively, if a
transaction aborts, the changes are not applied. The `abort` keyword is used to abort a transaction
and revert the changes made so far.

> It is important to note that there is no catch mechanism in Move. If a transaction aborts, the
> changes made so far are reverted, and the transaction is considered failed.

## Abort

The `abort` keyword is used to abort the execution of a transaction. It is used in combination with
an abort code, which will be returned to the caller of the transaction. The abort code is an
[integer](./primitive-types.md) of type `u64`.

```move
let user_has_access = true;

// abort with a predefined constant if `user_has_access` is false
if (!user_has_access) {
    abort 0
};

// there's an alternative syntax using parenthesis`
if (user_has_access) {
   abort(1)
};
```

The code above will, of course, abort with abort code `1`.

## assert!

The `assert!` macro is a built-in macro that can be used to assert a condition. If the condition is
false, the transaction will abort with the given abort code. The `assert!` macro is a convenient way
to abort a transaction if a condition is not met. The macro shortens the code otherwise written with
an `if` expression + `abort`. The `code` argument is required and has to be a `u64` value.

```move
// aborts if `user_has_access` is `false` with abort code 0
assert!(user_has_access, 0);

// expands into:
if (!user_has_access) {
    abort 0
};
```

## Error constants

To make error codes more descriptive, it is a good practice to define
[error constants](./constants.md). Error constants are defined as `const` declarations and are
usually prefixed with `E` followed by a camel case name. Error constants are no different from other
constants and don't have special handling, however, they are used to increase the readability of the
code and make it easier to understand the abort scenarios.

```move
/// Error code for when the user has no access.
const ENoAccess: u64 = 0;
/// Trying to access a field that does not exist.
const ENoField: u64 = 1;

/// Updates a record.
public fun update_record(/* ... , */ user_has_access: bool, field_exists: bool) {
    // asserts are way more readable now
    assert!(user_has_access, ENoAccess);
    assert!(field_exists, ENoField);

    /* ... */
}
```
