# 包清单

`Move.toml` 是一个描述包及其依赖项的清单文件。它以 TOML 格式编写，包含多个部分，其中最重要的是 [package]、[dependencies] 和 [addresses]。

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

[package] 部分用于描述包。本节中的字段均未在链上发布，但用于工具和发布管理；他们还指定了编译器的 Move 版本。

- name - 导入时包的名称；
- version - 包的版本，可用于发布管理；

The `[package]` section is used to describe the package. None of the fields in this section are
published on chain, but they are used in tooling and release management; they also specify the Move
edition for the compiler.

- `name` - the name of the package when it is imported;
- `version` - the version of the package, can be used in release management;

### Dependencies

[dependencies] 部分用于指定项目的依赖项。每个依赖项都被指定为一个键值对，其中键是依赖项的名称，值是依赖项规范。依赖项规范可以是 git 存储库 URL 或本地目录的路径。

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

可以将 [dev-dependencies] 部分添加到清单中。它用于覆盖开发和测试模式中的依赖关系。例如，如果您想在开发模式下使用不同版本的包，您可以将自定义依赖项规范添加到 [dev-dependencies] 部分。

It is possible to add `[dev-dependencies]` section to the manifest. It is used to override
dependencies in the dev and test modes. For example, if you want to use a different version of the
package in the dev mode, you can add a custom dependency specification to the
`[dev-dependencies]` section.

### Addresses

[addresses] 部分用于添加地址的别名。可以在此部分中指定任何地址，然后在代码中用作别名。例如，如果在此部分添加 alice = '0xA11CE'，则可以在代码中将 alice 用作 0xA11CE。

The `[addresses]` section is used to add aliases for the addresses. Any address can be specified in
this section, and then used in the code as an alias. For example, if you add `alice = "0xA11CE"` to
this section, you can use `alice` as `0xA11CE` in the code.

### Dev-addresses

[dev-addresses] 部分与 [addresses] 相同，但仅适用于测试和开发模式。需要注意的是，在本节中不可能引入新的别名，只能覆盖现有的别名。因此，在上面的示例中，如果将 alice = '0xB0B' 添加到此部分，则 alice 地址在测试和开发模式中将为 0xB0B，在常规构建中为 0xA11CE。

The `[dev-addresses]` section is the same as `[addresses]`, but only works for the test and dev
modes. Important to note that it is impossible to introduce new aliases in this section, only
override the existing ones. So in the example above, if you add `alice = "0xB0B"` to this section,
the `alice` address will be `0xB0B` in the test and dev modes, and `0xA11CE` in the regular build.

## Further Reading

- [Packages](/reference/packages.html) in the Move Reference.
