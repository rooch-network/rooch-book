# 交易

交易是区块链世界的基本概念。这是一种与区块链交互的方式。交易用于改变区块链的状态，这是唯一的方法。在 Move 中，事务用于调用包中的函数、部署新包以及升级现有包。

Transaction is a fundamental concept in the blockchain world. It is a way to interact with a
blockchain. Transactions are used to change the state of the blockchain, and they are the only way
to do so. In Move, transactions are used to call functions in a package, deploy new packages, and
upgrade existing ones.

## Transaction Structure

> 每个事务都明确指定它操作的对象！

交易包括：

- 发送者 - 签署交易的帐户；
- 命令列表（或链）- 要执行的操作；
- 命令输入 - 命令的参数：纯 - 简单值，如数字或字符串，或对象 - 交易将访问的对象；
- Gas 对象 - 用于支付交易的 Coin 对象；
- Gas 价格和预算 - 交易成本；

> Every transaction explicitly specifies the objects it operates on!

Transactions consist of:

- a sender - the [account](./what-is-an-account.md) that _signs_ the transaction;
- a list (or a chain) of commands - the operations to be executed;
- command inputs - the arguments for the commands: either `pure` - simple values like numbers or
  strings, or `object` - objects that the transaction will access;
- a gas object - the `Coin` object used to pay for the transaction;
- gas price and budget - the cost of the transaction;

## Commands

Rooch 事务可能由多个命令组成。每个命令都是一个内置命令（例如发布包）或对已发布包中函数的调用。命令按照它们在事务中列出的顺序执行，并且它们可以使用先前命令的结果，形成一条链。交易整体上要么成功，要么失败。

从原理上讲，交易如下所示（伪代码）：

Rooch transactions may consist of multiple commands. Each command is a single built-in command (like
publishing a package) or a call to a function in an already published package. The commands are
executed in the order they are listed in the transaction, and they can use the results of the
previous commands, forming a chain. Transaction either succeeds or fails as a whole.

Schematically, a transaction looks like this (in pseudo-code):

```
Inputs:
- sender = 0xa11ce

Commands:
- payment = SplitCoins(Gas, [ 1000 ])
- item = MoveCall(0xAAA::market::purchase, [ payment ])
- TransferObjects(item, sender)
```

在此示例中，事务由三个命令组成：

1. SplitCoins - 一个内置命令，用于从传递的对象（在本例中为 Gas 对象）中分割出新硬币；
2. MoveCall - 调用包 0xAAA 中的函数 buy 的命令，带有给定参数的模块 market - 支付对象；
3. TransferObjects - 将对象传输给接收者的内置命令。

In this example, the transaction consists of three commands:

1. `SplitCoins` - a built-in command that splits a new coin from the passed object, in this case,
   the `Gas` object;
2. `MoveCall` - a command that calls a function `purchase` in a package `0xAAA`, module `market`
   with the given arguments - the `payment` object;
3. `TransferObjects` - a built-in command that transfers the object to the recipient.


## Transaction Effects

交易效果是交易对区块链状态所做的改变。更具体地说，事务可以通过以下方式更改状态：

- 使用gas对象来支付交易；
- 创建、更新或删除对象；
- 发出事件；

执行交易的结果由不同部分组成：

- 交易摘要 - 交易的哈希值，用于识别交易；
- 交易数据 - 交易中使用的输入、命令和气体对象；
- 交易效果 - 交易的状态和“效果”，更具体地说：交易的状态、对象及其新版本的更新、使用的气体对象、交易的气体成本以及交易发出的事件;
- 事件 - 交易发出的自定义事件；
- 对象更改 - 对对象所做的更改，包括所有权的更改；
- 余额变化——交易涉及的账户总余额的变化；

Transaction effects are the changes that a transaction makes to the blockchain state. More
specifically, a transaction can change the state in the following ways:

- use the gas object to pay for the transaction;
- create, update, or delete objects;
- emit events;

The result of the executed transaction consists of different parts:

- Transaction Digest - the hash of the transaction which is used to identify the transaction;
- Transaction Data - the inputs, commands and gas object used in the transaction;
- Transaction Effects - the status and the "effects" of the transaction, more specifically: the
  status of the transaction, updates to objects and their new versions, the gas object used, the gas
  cost of the transaction, and the events emitted by the transaction;
- Events - the custom [events](./../programmability/events.md) emitted by the transaction;
- Object Changes - the changes made to the objects, including the _change of ownership_;
- Balance Changes - the changes made to the aggregate balances of the account involved in the
  transaction;
