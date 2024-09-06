# Custom Types with Struct

Move 的类型系统在定义自定义类型方面表现出色。用户定义的类型可以根据应用程序的特定需求进行定制。不仅在数据层面，而且在其行为方面。在本节中，我们介绍结构体的定义以及如何使用它。

Move's type system shines when it comes to defining custom types. User defined types can be custom
tailored to the specific needs of the application. Not just on the data level, but also in its
behavior. In this section we introduce the struct definition and how to use it.

## Struct

要定义自定义类型，您可以使用 struct 关键字，后跟类型名称。在名称之后，您可以定义结构体的字段。每个字段均使用 field_name: field_type 语法定义。字段定义必须用逗号分隔。这些字段可以是任何类型，包括其他结构。

Move 不支持递归结构，这意味着结构不能将自身包含为字段。

To define a custom type, you can use the `struct` keyword followed by the name of the type. After
the name, you can define the fields of the struct. Each field is defined with the
`field_name: field_type` syntax. Field definitions must be separated by commas. The fields can be of
any type, including other structs.

> Move does not support recursive structs, meaning a struct cannot contain itself as a field.

```move
{{#include ../../../packages/samples/sources/move-basics/struct.move:def}}
```

在上面的示例中，我们定义了一个具有五个字段的 Record 结构。 title 字段的类型为 String，artist 字段的类型为 Artist，year 字段的类型为 u16，is_debut 字段的类型为 bool，edition 字段的类型为 Option u16。版本字段的类型为Option u16，表示版本是可选的。

默认情况下，结构是私有的，这意味着它们不能在定义它们的模块外部导入和使用。它们的字段也是私有的，不能从模块外部访问。有关不同可见性修饰符的更多信息，请参阅可见性。

结构体的字段是私有的，只能由定义该结构体的模块访问。仅当定义结构的模块提供访问字段的公共函数时，才可以在其他模块中读取和写入结构的字段。

In the example above, we define a `Record` struct with five fields. The `title` field is of type
`String`, the `artist` field is of type `Artist`, the `year` field is of type `u16`, the `is_debut`
field is of type `bool`, and the `edition` field is of type `Option<u16>`. The `edition` field is of
type `Option<u16>` to represent that the edition is optional.

Structs are private by default, meaning they cannot be imported and used outside of the module they
are defined in. Their fields are also private and can't be accessed from outside the module. See
[visibility](./visibility.md) for more information on different visibility modifiers.

> Fields of a struct are private and can only be accessed by the module defining the struct. Reading
> and writing the fields of a struct in other modules is only possible if the module defining the
> struct provides public functions to access the fields.

## Create and use an instance

我们描述了结构定义的工作原理。现在让我们看看如何初始化一个结构体并使用它。可以使用 struct_name { field1: value1, field2: value2, ... } 语法来初始化结构体。这些字段可以按任何顺序初始化，并且必须设置所有字段。

We described how struct _definition_ works. Now let's see how to initialize a struct and use it. A
struct can be initialized using the `struct_name { field1: value1, field2: value2, ... }` syntax.
The fields can be initialized in any order, and all of the fields must be set.

```move
{{#include ../../../packages/samples/sources/move-basics/struct.move:pack}}
```

在上面的示例中，我们创建了 Artist 结构体的实例，并将 name 字段设置为字符串“The Beatles”。

要访问结构体的字段，您可以使用 .运算符后跟字段名称。

In the example above, we create an instance of the `Artist` struct and set the `name` field to a
string "The Beatles".

To access the fields of a struct, you can use the `.` operator followed by the field name.

```move
{{#include ../../../packages/samples/sources/move-basics/struct.move:access}}
```

只有定义该结构的模块才能访问其字段（可变和不可变）。所以上面的代码应该和Artist结构体在同一个模块中。

Only module defining the struct can access its fields (both mutably and immutably). So the above
code should be in the same module as the `Artist` struct.

<!-- ## Accessing Fields

Struct fields are private and can be accessed only by the module defining the struct. To access the fields of a struct, you can use the `.` operator followed by the field name.

```move
# anchor: access
{{#include ../../../packages/samples/sources/move-basics/struct.move:access}}
```
-->

## Unpacking a struct

默认情况下，结构是不可丢弃的，这意味着必须使用初始化的结构值：存储或解包。解包结构意味着将其解构为其字段。这是使用 let 关键字后跟结构名称和字段名称来完成的。

Structs are non-discardable by default, meaning that the initiated struct value must be used: either
stored or _unpacked_. Unpacking a struct means deconstructing it into its fields. This is done using
the `let` keyword followed by the struct name and the field names.

```move
{{#include ../../../packages/samples/sources/move-basics/struct.move:unpack}}
```

在上面的示例中，我们解压 Artist 结构并使用 name 字段的值创建一个新的变量 name。由于未使用该变量，编译器将发出警告。要抑制警告，您可以使用下划线 _ 来指示有意未使用该变量。

In the example above we unpack the `Artist` struct and create a new variable `name` with the value
of the `name` field. Because the variable is not used, the compiler will raise a warning. To
suppress the warning, you can use the underscore `_` to indicate that the variable is intentionally
unused.

```move
{{#include ../../../packages/samples/sources/move-basics/struct.move:unpack_ignore}}
```

## Further reading

- [Structs](/reference/structs.html) in the Move Reference.
