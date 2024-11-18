# 包清单

`Move.toml` 是一个描述包及其依赖项的清单文件。它以 TOML 格式编写，包含多个部分，其中最重要的是 `[package`]、`[dependencies]` 和 `[addresses]`。

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

[dev-addresses]
my_project = "0x42"
```

## 各部分详解

### Package

`[package]` 部分用于描述包。这部分的字段不会发布到链上，但会用于工具和版本管理。

- name - 导入时包的名称；
- version - 包的版本，可用于版本管理；

### Dependencies

`[dependencies]` 部分用于指定项目的依赖关系。每个依赖关系都以键值对的形式指定，键是依赖的名称，值是依赖的规范。依赖规范可以是 Git 存储库的 URL 或本地目录的路径。

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

### Addresses

`[addresses]` 部分用于为地址添加别名。可以在这部分中指定任何地址，然后在代码中使用别名。例如，如果在这部分添加 `alice = '0xA11CE'`，则可以在代码中将 `alice` 代替 `0xA11CE`。

### Dev-addresses

`[dev-addresses]` 部分与 `[addresses]` 相同，但仅适用于测试和开发模式。需要注意的是，在这个部分中不能引入新的别名，只能覆盖现有的别名。因此，在上面的示例中，如果将 `alice = '0xB0B'` 添加到这部分，则 `alice` 地址在测试和开发模式中将为 `0xB0B`，在常规构建中为 `0xA11CE`。

## 相关链接

- [包](./packages.md)
