# 向量

向量是 Move 中存储元素集合的原生方式。它们与其他编程语言中的数组类似，但也有一些差异。在本节中，我们介绍向量（`vector`）类型及其操作。

## 向量的语法

向量类型使用 `vector` 关键字定义，向量的类型通过后跟的尖括号（`<>`）中的元素类型来确定。元素的类型可以是任何有效的 Move 类型，包括其他向量。Move 提供*向量字面量*语法，允许你使用 `vector` 关键字创建向量，后跟包含元素的方括号（`[]`），也可以是一个空向量，即不包含任何元素。

```move
// An empty vector of bool elements.
let empty: vector<bool> = vector[];

// A vector of u8 elements.
let v: vector<u8> = vector[10, 20, 30];

// A vector of vector<u8> elements.
let vv: vector<vector<u8>> = vector[
    vector[10, 20],
    vector[30, 40]
];
```

向量类型是 Move 中的内置类型，不需要从模块导入。但是，向量运算是在 `std::vector` 模块中定义的，您需要导入该模块才能使用它们。

## 向量的操作

标准库提供了操作向量的方法。以下是一些最常用的操作：

- push_back：将一个元素添加到向量的末尾。
- pop_back：从向量中删除最后一个元素。
- length：返回向量中元素的数量。
- is_empty：如果向量为空，则返回 true。
- remove：删除给定索引处的元素。

The standard library provides methods to manipulate vectors. The following are some of the most
commonly used operations:

- `push_back`: Adds an element to the end of the vector.
- `pop_back`: Removes the last element from the vector.
- `length`: Returns the number of elements in the vector.
- `is_empty`: Returns true if the vector is empty.
- `remove`: Removes an element at a given index.

vector 类型是 Move 中的内置类型，不需要从模块中导入。然而，向量操作定义在 std::vector 模块中，你需要导入该模块才能使用这些操作。

向量操作
标准库提供了许多操作向量的方法。以下是一些常用的操作：

push_back: 在向量末尾添加一个元素。
pop_back: 移除向量的最后一个元素。
length: 返回向量中元素的数量。
is_empty: 如果向量为空则返回 true。
remove: 移除给定索引处的元素。


```move
let mut v = vector[10u8, 20, 30];

assert!(v.length() == 3, 0);
assert!(!v.is_empty(), 1);

v.push_back(40);
let last_value = v.pop_back();

assert!(last_value == 40, 2);
```

## Destroying a Vector of non-droppable types

不可丢弃类型的向量不能被丢弃。如果定义不具有丢弃能力的类型向量，则不能忽略向量值。但是，如果向量为空，编译器需要显式调用 destroy_empty 函数。

A vector of non-droppable types cannot be discarded. If you define a vector of types without `drop`
ability, the vector value cannot be ignored. However, if the vector is empty, compiler requires an
explicit call to `destroy_empty` function.

```move
    /// A struct without `drop` ability.
    public struct NoDrop {}

    #[test]
    fun test_destroy_empty() {
        // Initialize a vector of `NoDrop` elements.
        let v = vector<NoDrop>[];

        // While we know that `v` is empty, we still need to call
        // the explicit `destroy_empty` function to discard the vector.
        v.destroy_empty();
    }
```
