# Comments

Comments are a way to add notes or document your code. They are ignored by the compiler and don't result in the Move bytecode. You can use comments to explain what your code does, add notes to yourself or other developers, temporarily remove a part of your code, or generate documentation. There are three types of comments in Move: line comments, block comments, and doc comments.

## Line Comments

Line comments start with `//` and comment out the rest of the line. Everything after `//` is ignored by the compiler.

```Move
module book::comments_line {
    fun some_function() {
        // this is a comment line
    }
}
```

You can use `//` to add notes or explanations to your code.

```Move
module book::comments_line_2 {
    // let's add a note to everything!
    fun some_function_with_numbers() {
        let a = 10;
        // let b = 10; this line is commented and won't be executed
        let b = 5; // here comment is placed after code
        a + b; // result is 15, not 10!
    }
}
```

## Block Comments

Block comments are used to comment out a block of code. They start with `/*` and end with `*/`. Everything between `/*` and `*/` is ignored by the compiler. You can use block comments to comment out single or multiple lines, or even parts of a line.

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

This example shows how you can use block comments to comment out parts of a line.

## Doc Comments

Documentation comments are special comments used to generate documentation for your code. They start with three slashes `///` and are placed before the definition of the item they document.

```Move
/// This function adds two numbers.
fun add(a: u64, b: u64): u64 {
    a + b
}
```
