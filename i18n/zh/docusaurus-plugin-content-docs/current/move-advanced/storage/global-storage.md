# Move 语言全局存储详解

## 概述

在 Move 语言中，全局存储是一个持久化的键值存储系统，用于存储链上的所有状态数据。它具有以下特点：

1. 持久性：数据会永久保存在链上
2. 可访问性：通过地址和类型来访问
3. 类型安全：存储的数据必须符合 Move 的类型系统

## 存储结构

```move
// 全局存储的基本结构
GlobalStorage {
    Address -> {
        ResourceType -> ResourceValue,
        ModuleName -> ModuleCode
    }
}
```

## 主要概念

### 1. Resources（资源）

Resources 是 Move 中最重要的概念之一：

```move
struct Counter has key {
    value: u64
}
```

- `key` 能力表示它可以存储在全局存储中
- 每个地址下每种类型只能存储一个实例
- 必须显式移动或销毁，不能复制或丢弃

### 2. 存储操作

```move
// 移动到全局存储
move_to<T>(account: &signer, resource: T);

// 从全局存储中借用
borrow_global<T>(addr: address): &T;
borrow_global_mut<T>(addr: address): &mut T;

// 从全局存储中移除
move_from<T>(addr: address): T;
```

### 3. Object Model

Rooch 在 Move 基础上增加了对象模型：

```move
struct Object<T> has key {
    id: ObjectID,
    owner: address,
    value: T
}
```

## 使用示例

### 1. 定义资源

```move
module examples::counter {
    struct Counter has key {
        value: u64
    }

    public fun initialize(account: &signer) {
        move_to(account, Counter { value: 0 });
    }
}
```

### 2. 查询状态

```ts
const states = await client.getStates({
    accessPath: '/object/0x1::counter::Counter',
    stateOption: {
        decode: true
    }
});
```

## 注意事项

1. 类型安全

- 只能存储带有 key 能力的类型
- 必须遵循 Move 的所有权规则

2. 访问控制

- 需要正确的能力才能访问资源
- 模块可以控制资源的访问权限

3. 性能考虑

- 读取操作相对较快
- 写入操作会消耗更多 gas

4. 存储成本

- 链上存储需要支付 gas 费用
- 建议合理设计数据结构以优化存储

全局存储是 Move 智能合约的基础设施，理解它对于开发 Move 合约至关重要。