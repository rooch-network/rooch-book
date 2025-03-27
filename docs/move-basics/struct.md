# Custom Types with Struct

Move's type system excels at defining custom types tailored to the specific needs of an application, both in data and behavior. This section introduces struct definitions and their usage.

## Struct Definition

To define a custom type, use the `struct` keyword followed by the type name. Then, define the fields of the struct using the `field_name: field_type` syntax, separated by commas. Fields can be of any type, including other structs.

> Move does not support recursive structs, meaning a struct cannot contain itself as a field.

```move
/// A struct representing an artist.
public struct Artist {
    /// The name of the artist.
    name: String,
}

/// A struct representing a music record.
public struct Record {
    /// The title of the record.
    title: String,
    /// The artist of the record. Uses the `Artist` type.
    artist: Artist,
    /// The year the record was released.
    year: u16,
    /// Whether the record is a debut album.
    is_debut: bool,
    /// The edition of the record.
    edition: Option<u16>,
}
```

In the example above, the `Record` struct has five fields: `title` (String), `artist` (Artist), `year` (u16), `is_debut` (bool), and `edition` (`Option<u16>`), where `edition` is optional.

Structs are private by default, meaning they cannot be imported and used outside the module they are defined in. Their fields are also private and can't be accessed from outside the module. See visibility for more information on visibility modifiers.

> Fields of a struct are private and can only be accessed by the module defining the struct. Reading and writing the fields of a struct in other modules is only possible if the module defining the struct provides public functions to access the fields.

## Creating and Using an Instance

We described how struct definitions work. Now let's see how to initialize and use a struct. A struct can be initialized using the `struct_name { field1: value1, field2: value2, ... }` syntax. Fields can be initialized in any order, but all fields must be set.

```move
let mut artist = Artist {
    name: b"The Beatles".to_string()
};
```

In the example above, we create an instance of the `Artist` struct and set the `name` field to "The Beatles".

To access the fields of a struct, use the `.` operator followed by the field name.

```move
// Access the `name` field of the `Artist` struct.
let artist_name = artist.name;

// Access a field of the `Artist` struct.
assert!(artist.name == string::utf8(b"The Beatles"), 0);

// Mutate the `name` field of the `Artist` struct.
artist.name = string::utf8(b"Led Zeppelin");

// Check that the `name` field has been mutated.
assert!(artist.name == string::utf8(b"Led Zeppelin"), 1);
```

Only the module defining the struct can access its fields (both mutably and immutably). So the above code should be in the same module as the `Artist` struct.

## Unpacking a Struct

Structs are non-discardable by default, meaning that the initialized struct value must be used: either stored or unpacked. Unpacking a struct means deconstructing it into its fields using the `let` keyword followed by the struct name and the field names.

```move
// Unpack the `Artist` struct and create a new variable `name`
// with the value of the `name` field.
let Artist { name } = artist;
```

In the example above, we unpack the `Artist` struct and create a new variable `name` with the value of the `name` field. If the variable is not used, the compiler will raise a warning. To suppress the warning, use the underscore `_` to indicate that the variable is intentionally unused.

```move
// Unpack the `Artist` struct and ignore the `name` field.
let Artist { name: _ } = artist;
```
