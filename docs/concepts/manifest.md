# Package Manifest

The `Move.toml` is a manifest file that describes the [package](./packages.md) and its dependencies.
It is written in [TOML](https://toml.io/en/) format and contains multiple sections, the most
important of which are `[package]`, `[dependencies]` and `[addresses]`.

```toml
[package]
name = "my_project"
version = "0.0.1"

[dependencies]
MoveStdlib = { git = "https://github.com/rooch-network/rooch.git", subdir = "frameworks/move-stdlib", rev = "main" }
MoveosStdlib = { git = "https://github.com/rooch-network/rooch.git", subdir = "frameworks/moveos-stdlib", rev = "main" }
RoochFramework = { git = "https://github.com/rooch-network/rooch.git", subdir = "frameworks/rooch-framework", rev = "main" }

[addresses]
my_project = "0xff96b781b4a6198bf78008ea96388fb8a76eefdd58f82731d851c4ae236ca18f"
std = "0x1"
moveos_std = "0x2"
rooch_framework = "0x3"
```

## Sections

### Package

The `[package]` section is used to describe the package. None of the fields in this section are
published on chain, but they are used in tooling and release management; they also specify the Move
edition for the compiler.

- `name` - the name of the package when it is imported;
- `version` - the version of the package, can be used in release management;

### Dependencies

The `[dependencies]` section is used to specify the dependencies of the project. Each dependency is
specified as a key-value pair, where the key is the name of the dependency, and the value is the
dependency specification. The dependency specification can be a git repository URL or a path to the
local directory.

```toml
# git repository
MoveStdlib = { git = "https://github.com/rooch-network/rooch.git", subdir = "frameworks/move-stdlib", rev = "main" }
MoveosStdlib = { git = "https://github.com/rooch-network/rooch.git", subdir = "frameworks/moveos-stdlib", rev = "main" }
RoochFramework = { git = "https://github.com/rooch-network/rooch.git", subdir = "frameworks/rooch-framework", rev = "main" }

# local directory
MoveStdlib = { local = "../../frameworks/move-stdlib" }
MoveosStdlib = { local = "../../frameworks/moveos-stdlib" }
RoochFramework = { local = "../../frameworks/rooch-framework" }
```

### Dev-dependencies

It is possible to add `[dev-dependencies]` section to the manifest. It is used to override
dependencies in the dev and test modes. For example, if you want to use a different version of the
package in the dev mode, you can add a custom dependency specification to the
`[dev-dependencies]` section.

### Addresses

The `[addresses]` section is used to add aliases for the addresses. Any address can be specified in
this section, and then used in the code as an alias. For example, if you add `alice = "0xA11CE"` to
this section, you can use `alice` as `0xA11CE` in the code.

### Dev-addresses

The `[dev-addresses]` section is the same as `[addresses]`, but only works for the test and dev
modes. Important to note that it is impossible to introduce new aliases in this section, only
override the existing ones. So in the example above, if you add `alice = "0xB0B"` to this section,
the `alice` address will be `0xB0B` in the test and dev modes, and `0xA11CE` in the regular build.

## Further Reading

- [Packages](/reference/packages.html) in the Move Reference.
