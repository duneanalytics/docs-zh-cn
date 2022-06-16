---
description: >-
  我们对智能合约发出的数据进行解码，并将它们存储在易于使用的表格中。
---

# 已解码数据

## 已解码智能合约数据

Dune 不使用跟踪和日志，而是将智能合约活动解码为易于阅读的表格。

我们为智能合约 ABI 中定义的每个事件（event）和功能（function）创建表格。随后，对该合约的每个事件或函数调用都被解码并作为一行插入到这些表中。

这些表被相应地命名：

**事件（events）:** `projectname."contractName_evt_eventName"`

**函数调用（function calls）:** `projectname."contractName_call_eventName"`

例如，在表 [uniswap\_v2."pair\_swap"](https://dune.xyz/queries/38968) 中可以找到 uniswap V2 交换合约的 `swap` 事件的解码数据。

使用事件表通常就足够了，但在某些情况下，您会想要使用 `call` 表。例如 Maker DAO，它不会给你太多事件，你可以使用像 [maker."SaiTub\_call\_draw](https://dune.xyz/queries/38974)" 这样的表格。

## 哪些合约有已解码数据？

您可以通过我们的数据库查询 “blockchain”.contracts 表或 [这个仪表板](https://dune.xyz/0xBoxer/Is-my-Contract-decoded-yet) 来检查合约是否已解码。

如果合约还没有在我们的数据库中，您可以在此处将其提交给 [解码](../../duneapp/adding-new-contracts.md)：[dune.xyz/decode](http://dune.xyz/decode）.

我们通常需要大约 24-48 小时来解码智能合约。

查看 [本指南](../../duneapp/adding-new-contracts.md) 以了解有关解码过程的更多信息。

## 如何理解已解码数据？

解码数据有时有点难以处理，因为它需要您了解事件/调用在智能合约上下文中的含义。此外，您需要了解智能合约发出什么样的数据，并了解项目的不同智能合约相互交互的复杂性。通常，您要查找的数据分散在 Dune 中的多个智能合约和表格中。

如果您无法通过仅在表格中搜索来理解数据，则使用事务哈希和 etherscan 查看单个交易（transaction）的详情通常会有所帮助。

如果这也不能带来令人满意的结果，那么搜索项目的相关文档和 github 可以引导您找到所需的答案。此外，与项目的开发人员和核心社区交流也可以让你更好地理解合约。

在整个 Dune 中都可以找到有关如何处理解码数据的一些很好的展示，但尤其是我们的 [抽象存储库](https://github.com/duneanalytics/abstractions) 充满了很好的例子。

**总之**:

处理解码数据可以让您深入访问存储在区块链上的信息，并且信息量非常丰富，但了解数据有时需要您付出一些努力，因为您正在以直接的方式与合约数据进行交互。

## 跨合约的可扩展解码

许多 dApp 拥有大量使用相同字节码部署的智能合约。这可以是：收益聚合池、独特的期权、流动性池等。

我们可以自动将这些相似的合约提取到同一个表中，从而让您更轻松地处理这些数据。无需查询所有独特的智能合约，您只需查询一张表，该表将以该特定智能合约的 `contract_address` 作为标识符。

为了能够使用此功能，您必须在将合约提交解码时将其作为两个底部选项之一提交。

![](<../../.gitbook/assets/image (23).png>)

因此，来自单个表的 `SELECT` 可能会产生来自多个合约的数据。在解码表中，`contract_address` 列告诉你事件或调用在哪个智能合约上。如果您只想查看单个合约，则可以按其地址进行过滤。

例如:

```sql
SELECT DISTINCT contract_address FROM uniswap_v2."Pair_evt_swap";
```

将通过代币购买事件为您提供所有独特的 Uniswap 代币对。

[查询实战](https://dune.xyz/queries/39006)

## **探索解码合约的查询**

**查看我们已解码数据的所有项目**

```sql
SELECT DISTINCT namespace FROM ethereum."contracts";
```

如果您直接使用事件或调用表，您可以通过此查询查看该合约是否有多个实例。

```sql
SELECT DISTINCT contract_address FROM projectname."contractName_evt_eventName";
```
