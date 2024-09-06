# Vector

向量是 Move 中存储元素集合的原生方式。它们与其他编程语言中的数组类似，但也有一些差异。在本节中，我们介绍向量类型及其操作。

Vectors are a native way to store collections of elements in Move. They are similar to arrays in
other programming languages, but with a few differences. In this section, we introduce the `vector`
type and its operations.

## Vector syntax

向量类型是使用向量关键字后跟尖括号中的元素类型来定义的。元素的类型可以是任何有效的 Move 类型，包括其他向量。 Move 具有向量文字语法，允许您使用向量关键字创建向量，后跟包含元素的方括号（或者空向量不包含元素）。

The `vector` type is defined using the `vector` keyword followed by the type of the elements in
angle brackets. The type of the elements can be any valid Move type, including other vectors. Move
has a vector literal syntax that allows you to create vectors using the `vector` keyword followed by
square brackets containing the elements (or no elements for an empty vector).

```move
{{#include ../../../packages/samples/sources/move-basics/vector.move:literal}}
```

矢量类型是 Move 中的内置类型，不需要从模块导入。但是，向量运算是在 std::vector 模块中定义的，您需要导入该模块才能使用它们。

The `vector` type is a built-in type in Move, and does not need to be imported from a module.
However, vector operations are defined in the `std::vector` module, and you need to import the
module to use them.

## Vector operations

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

```move
{{#include ../../../packages/samples/sources/move-basics/vector.move:methods}}
```

## Destroying a Vector of non-droppable types

不可丢弃类型的向量不能被丢弃。如果定义不具有丢弃能力的类型向量，则不能忽略向量值。但是，如果向量为空，编译器需要显式调用 destroy_empty 函数。

A vector of non-droppable types cannot be discarded. If you define a vector of types without `drop`
ability, the vector value cannot be ignored. However, if the vector is empty, compiler requires an
explicit call to `destroy_empty` function.

```move
{{#include ../../../packages/samples/sources/move-basics/vector.move:no_drop}}
```

## Further reading

- [Vector](/reference/primitive-types/vector.html) in the Move Reference.
