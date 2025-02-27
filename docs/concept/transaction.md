# 交易

交易（transaction）是区块链世界中的一个基本概念。交易是向区块链传递信息的方式，通过发送交易可以与区块链进行交互。交易用于改变区块链的状态，这是唯一的方法。在 Move 中，交易用于调用包中的函数、部署新包以及升级现有包。

## 用户如何与程序交互

用户通过调用程序中的公开函数与区块链上的智能合约进行交互。这些公开函数定义了可以在交易中执行的操作。交易是由账户发起的，账户发送交易时指定它要操作的对象。

## 交易的结构

> 每个交易都会明确地指定它要操作的对象！

交易由以下部分组成：

- 发送者 - 签署交易的帐户；
- 命令列表 - 要执行的操作；
- 命令输入 - 命令的参数：
  - `pure` - 简单值，如数字或字符串
  - `object` - 交易将访问的对象；
- Gas 对象 - 用于支付交易的 Coin 对象；
- Gas 价格和预算 - 交易成本；

## 命令

Rooch 交易可能由多个命令组成。每个命令都是一个内置命令（例如发布包）或对已发布包中函数的调用。命令按照它们在交易中列出的顺序执行，并且它们可以使用先前命令的结果，形成一条链。交易具有原子性，要么整体成功，要么整体失败。

从原理上讲，交易如下所示（伪代码）：

```
Inputs:
- sender = 0xa11ce

Commands:
- payment = SplitCoins(Gas, [ 1000 ])
- item = MoveCall(0xAAA::market::purchase, [ payment ])
- TransferObjects(item, sender)
```

在此示例中，交易由三个命令组成：

1. `SplitCoins` - 一个内置命令，用于从传递的对象（在本例中为 Gas 对象）中分割出新硬币；
2. `MoveCall` - 一个调用 `0xAAA` 地址上的 `market` 模块中的 `purchases` 函数的命令，并传入 `payment` 对象作为参数；
3. `TransferObjects` - 一个将对象转移给接收者的内置命令。

## 交易的效果

交易效果是交易对区块链状态所做的改变。更具体地说，交易可以通过以下方式更改状态：

- 使用 gas 对象来支付交易费用；
- 创建、更新或删除对象；
- 发出事件；

执行交易的结果由不同部分组成：

- 交易摘要 - 交易的哈希值，用于识别交易；
- 交易数据 - 交易中使用的输入、命令和 gas 对象；
- 交易效果 - 交易的状态和“效果”，更具体地说：交易的状态、对象及其新版本的更新、使用的 gas 对象、交易的 gas 成本以及交易发出的事件;
- 事件 - 交易发出的自定义事件；
- 对象更改 - 对对象所做的更改，包括所有权的更改；
- 余额更改 - 交易涉及的账户总余额的变化；
