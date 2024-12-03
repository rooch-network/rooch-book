# 注释

注释是为代码添加解释的一种方法。它们会被编译器忽略，并且不会生成 Move 字节码。你可以使用注释来解释代码的功能、为自己或其他开发人员添加注释、暂时删除代码的一部分或生成文档。Move 中的注释分为三种类型：行注释、块注释和文档注释。

## 行注释

```Move
module book::comments_line {
    fun some_function() {
        // this is a comment line
    }
}
```

你可以使用双斜杠 `//` 注释掉该行的其余部分。`//` 之后的所有内容都会被编译器忽略。

```Move
module book::comments_line_2 {
    // let's add a note to everything!
    fun some_function_with_numbers() {
        let a = 10;
        // let b = 10 this line is commented and won't be executed
        let b = 5; // here comment is placed after code
        a + b; // result is 15, not 10!
    }
}
```

## 块注释

块注释用于注释掉一段代码。它们以 `/*` 开头，以 `*/` 结尾。`/*` 和 `*/` 之间的所有内容都将被编译器忽略。你可以使用块注释来注释掉单行或多行。你甚至可以使用它们注释掉一行的一部分。

```Move
module book::comments_block {
    fun /* you can comment everywhere */ go_wild() {
        /* here
           there
           everywhere */ let a = 10;
        let b = /* even here */ 10; /* and again */
        a + b;
    }
    /* you can use it to remove certain expressions or definitions
    fun empty_commented_out() {

    }
    */
}
```

这个例子有点极端，但它展示了如何使用块注释来注释掉一行中的一部分。

## 文档注释

文档注释是用于为代码生成文档的特殊注释。它们与块注释类似，但它们以三个斜杠 `///` 开头，并放置在它们所记录的项目的定义之前。

```Move
/// Module has documentation!
module book::comments_doc {

    /// This is a 0x0 address constant!
    const AN_ADDRESS: address = @0x0;

    /// This is a struct!
    public struct AStruct {
        /// This is a field of a struct!
        a_field: u8,
    }

    /// This function does something!
    /// And it's documented!
    fun do_something() {}
}
```
