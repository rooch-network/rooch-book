# 终止执行

区块链上的交易可以成功，也可以失败。成功执行的交易会将所有对对象和链上数据的改动应用，并且该交易会被提交到区块链上。如果交易失败（abort），则不会应用这些改动。abort 关键字用于中止交易，并撤销到目前为止进行的所有更改。

> 需要注意的是，Move 语言中没有类似其他语言的捕获机制 (catch mechanism)。如果交易中止，到目前为止进行的所有更改都将被撤销，并且该交易被视为失败。

## 终止

abort 关键字用于中止交易的执行。它与中止代码 (abort code) 一起使用，该代码会返回给交易的调用者。中止代码是一个无符号 64 位整数 (u64)。

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

上面的代码肯定会中止执行，并返回中止代码 1。

## 断言 (assert!)

assert! 是一个内置宏，用于断言一个条件是否成立。如果条件为假，则交易会中止，并返回给定的中止代码。assert! 宏提供了一种方便的方法，可以在条件不满足时中止交易。该宏可以替代使用 if 语句和 abort 来编写的代码。code 参数是必需的，它必须是一个 u64 值。

```move
// aborts if `user_has_access` is `false` with abort code 0
assert!(user_has_access, 0);

// expands into:
if (!user_has_access) {
    abort 0
};
```

## 错误代码

为了使中止代码更具描述性，定义 错误代码 是一个好习惯。错误代码使用 const 关键字声明，通常以 E 开头，后面跟驼峰式命名。错误代码与其他常量没有什么不同，没有特殊的处理方式，但是它们可以提高代码的可读性，并使人们更容易理解中止场景。

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
