---
说明: >-
  Flashbots 是一个研发组织，旨在确保 MEV 激励措施不会变得不透明和不民主。
---

# Flashbots

**注意：** mev-inspect-py 是 Flashbots 用于生成 MEV 数据的开源引擎，用于为 mev-explore 和 Dune 的 Flashbots 集成等仪表板提供动力。在我们的社区和贡献者的帮助下，我们一直在寻求改进、修复错误、覆盖边缘情况并尽我们所能增加协议覆盖率。我们鼓励研究人员和开发人员报告并帮助纠正任何发现的错误，或实现任何新功能！ 请随时查阅文档并加入 Flashbots discord，以获取有关我们的数据和 mev-inspect 的更多信息和更新。

**文档：** [https://docs.flashbots.net/](https://docs.flashbots.net)

**Discord：** [https://discord.gg/7hvTycdNcK](https://discord.gg/7hvTycdNcK)

## **flashbots.mev\_summary**

此表包含所有分类交易的摘要。

查询示例可在此处找到：[来自清算和套利的矿工收入](https://dune.com/queries/625974/1167301)

| **列名**                      | **数据类型**  | **说明**                                        |
| ------------------------------------ | --------- | ------------------------------------------------------ |
| block\_timestamp                     | timestamp | 区块时间戳                                        |
| block\_number                        | bigint    | 区块编号                                           |
| base\_fee\_per\_gas                  | bigint    | 每单位 gas 的基本费用                                       |
| coinbase\_transfer                   | bigint    | 直接转账到矿工地址的费用                    |
| error                                | string    | 错误信息（如果存在）                                        |
| gas\_price                           | bigint    | gas 价格                                       |
| gas\_price\_with\_coinbase\_transfer | bigint    | gas 价格+直接转账到矿工地址的费用     |
| gas\_used                            | bigint    | 消耗的 gas 量                                    |
| gross\_profit\_usd                   | double    | 以美元为单位的交易总利润               |
| miner\_address                       | string    | 矿工地址                                 |
| miner\_payment\_usd                  | double    | 矿工收到的美元付款                  |
| protocol                             | string    | 主要交互协议                              |
| protocols                            | string    | 交易中涉及的协议列表          |
| transaction\_hash                    | string    | 交易哈希                                |
| type                                 | string    | MEV的类型（例如arbitrage）                       |
| timestamp                            | timestamp | 文件最新更新的时间戳             |

## **flashbots.arbitrages**

此表包含有关每笔套利交易的附加信息的记录。

查询示例可在此处找到：[总套利协议](https://dune.com/queries/626076/1167481)

| **列名**        | **数据类型**  | **说明**                               |
| ---------------------- | --------- | --------------------------------------------- |
| block\_number          | bigint    | 区块编号                                  |
| account\_address       | string    | 搜索者的地址                       |
| created\_at            | string    | 记录创建时间                   |
| end\_amount            | bigint    | 套利后可用金额          |
| error                  | string    | 错误信息（如果存在）         |
| id                     | string    | 套利的内部id                  |
| profit\_amount         | bigint    | 套利后的利润额             |
| profit\_token\_address | string    | 利润资产地址                  |
| protocols              | string    | 交易中涉及的协议列表 |
| start\_amount          | bigint    | 套利前可用金额         |
| transaction\_hash      | string    | 交易哈希                       |
| timestamp              | timestamp | 文件最新更新的时间戳    |

## **flashbots.liquidations**

清算是另一种 MEV 策略。此表包含与已执行清算相关的详细信息。

查询示例可以在这里找到：[协议清算](https://dune.com/queries/625715/1166880)

| **列名**          | **数据类型**  | **说明**                                                                                                     |
| ------------------------ | --------- | ------------------------------------------------------------------------------------------------------------------- |
| created\_at              | string    | 记录创建时间                                                                                        |
| transaction\_hash        | string    | 交易哈希                                                                                                    |
| trace\_address           | string    | 与 MEV 交易相关的所有交易链中交易位置相关的跟踪模式        |
| debt\_token\_address     | string    | 要支付的债务的基础代币地址                                                                         |
| received\_amount         | bigint    | 从清算中收到的金额                                                                                |
| protocol                 | string    | 协议名称                                                                                                       |
| liquidated\_user         | string    | 被清算用户地址                                                                                      |
| liquidator\_user         | string    | 清算人用户地址                                                                                      |
| received\_token\_address | string    | 收到资产的地址                                                                                       |
| block\_number            | bigint    | 区块编号                                                                                                        |
| debt\_purchase\_amount   | bigint    | 购买的债务金额                                                                                            |
| timestamp                | timestamp | 文件最新更新的时间戳                                                                          |

## **flashbots.sandwiched\_swaps**

Sandwiched\_swaps 表包含有关一个或多个交换的附加数据，这些交换与数据库中的相应三明治夹在中间。

| **列名**   | **数据类型**  | **说明**                                                                                             |
| ----------------- | --------- | ----------------------------------------------------------------------------------------------------------- |
| created\_at       | string    | 记录创建时间                                                                               |
| block\_number     | bigint    | 区块编号                                                                                                |
| sandwich\_id      | string    | 三明治交易的内部 id                                                                         |
| trace\_address    | string    | 与套利交易相关的所有掉期链中掉期位置相关的跟踪模式 |
| transaction\_hash | string    | 交易哈希                                                                                            |
| timestamp         | timestamp | 文件最新更新的时间戳                                                                  |

## **flashbots.sandwiches**

此表包含有关已执行三明治交换的详细信息。

| **列名**                   | **数据类型**  | **说明**                                                 |
| --------------------------------- | --------- | --------------------------------------------------------------- |
| created\_at                       | datetime  | 记录创建时间                                    |
| block\_number                     | bigint    | 区块编号                                                    |
| backrun\_swap\_trace\_address     | string    | 回溯交易中的交换地址                  |
| backrun\_swap\_transaction\_hash  | string    | 指定三明治的backrun代交易的交易哈希  |
| frontrun\_swap\_trace\_address    | string    | 前端交易中的掉期地址                 |
| frontrun\_swap\_transaction\_hash | string    | 指定三明治前端交易的交易哈希 |
| id                                | string    | 三明治交易的内部 id                                     |
| profit\_amount                    | bigint    | 套利后的利润额                              |
| profit\_token\_address            | string    | 利润资产地址                                     |
| sandwicher\_address               | string    | 三明治交易者的地址                                       |
| timestamp                         | timestamp | 文件最新更新的时间戳                      |

## **flashbots.blocks**

此表包含块号和对应的区块时间戳。

| **列名**  | **数据类型**  | **说明** |
| ---------------- | --------- | --------------- |
| block\_number    | bigint    | 区块编号    |
| block\_timestamp | timestamp | 区块时间戳 |
