# Comments

Comments are a way to add notes or document your code. They are ignored by the compiler and don't
result in the Move bytecode. You can use comments to explain what your code does, to add notes to
yourself or other developers, to temporarily remove a part of your code, or to generate
documentation. There are three types of comments in Move: line comment, block comment, and doc
comment.

## Line comment

```Move
module book::comments_line {
    fun some_function() {
        // this is a comment line
    }
}
```

You can use double slash `//` to comment out the rest of the line. Everything after `//` will be
ignored by the compiler.

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

## Block comment

Block comments are used to comment out a block of code. They start with `/*` and end with `*/`.
Everything between `/*` and `*/` will be ignored by the compiler. You can use block comments to
comment out a single line or multiple lines. You can even use them to comment out a part of a line.

```Move
```

This example is a bit extreme, but it shows how you can use block comments to comment out a part of
a line.

## Doc comment

Documentation comments are special comments that are used to generate documentation for your code.
They are similar to block comments, but they start with three slashes `///` and are placed before
the definition of the item they document.

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
