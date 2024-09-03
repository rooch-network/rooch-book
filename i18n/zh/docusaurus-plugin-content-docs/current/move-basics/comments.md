# Comments

注释是添加注释或记录代码的一种方式。它们会被编译器忽略，并且不会生成 Move 字节码。您可以使用注释来解释代码的用途、为自己或其他开发人员添加注释、暂时删除代码的一部分或生成文档。 Move中的注释分为三种类型：行注释、块注释和文档注释。

Comments are a way to add notes or document your code. They are ignored by the compiler and don't
result in the Move bytecode. You can use comments to explain what your code does, to add notes to
yourself or other developers, to temporarily remove a part of your code, or to generate
documentation. There are three types of comments in Move: line comment, block comment, and doc
comment.

## Line comment

```Move
{{#include ../../../packages/samples/sources/move-basics/comments.move:line}}
```

您可以使用双斜杠 // 注释掉该行的其余部分。 // 之后的所有内容都会被编译器忽略。

You can use double slash `//` to comment out the rest of the line. Everything after `//` will be
ignored by the compiler.

```Move
{{#include ../../../packages/samples/sources/move-basics/comments.move:line_2}}
```

## Block comment

块注释用于注释掉一段代码。它们以 /* 开头，以 */ 结尾。 /* 和 */ 之间的所有内容都将被编译器忽略。您可以使用块注释来注释掉单行或多行。您甚至可以使用它们注释掉一行的一部分。

Block comments are used to comment out a block of code. They start with `/*` and end with `*/`.
Everything between `/*` and `*/` will be ignored by the compiler. You can use block comments to
comment out a single line or multiple lines. You can even use them to comment out a part of a line.

```Move
{{#include ../../../packages/samples/sources/move-basics/comments.move:block}}
```

这个例子有点极端，但它展示了如何使用块注释来注释掉一行的一部分。
This example is a bit extreme, but it shows how you can use block comments to comment out a part of
a line.

## Doc comment

文档注释是用于为代码生成文档的特殊注释。它们与块注释类似，但它们以三个斜杠 /// 开头，并放置在它们所记录的项目的定义之前。

Documentation comments are special comments that are used to generate documentation for your code.
They are similar to block comments, but they start with three slashes `///` and are placed before
the definition of the item they document.

```Move
{{#include ../../../packages/samples/sources/move-basics/comments.move:doc}}
```

<!-- TODO: docgen, which members are in the documentation -->
