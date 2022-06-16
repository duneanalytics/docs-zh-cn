---
description: >-
  我们的数据库自动从 Solana 区块链中提取原始数据，
  并根据表格关注的方面将它们解析为不同的表格。
---

# Solana 数据

下面的所有表格都是在 Dune 平台的更新版本中构建的，需要对 wizards 在平台中编写查询的方式进行轻微更改。新的数据平台利用 Spark 来提高查询速度、数据规模并支持多链查询。

在您开始查询 Solana 表时使用的参考文档可在此处获得：

{% embed url="https://spark.apache.org/docs/latest/sql-ref-ansi-compliance.html" %}

## Solana.blocks

该表包含 Solana 区块链中的区块数据。它可用于识别区块活动和随时间的交易变化。

查询示例可在此处找到：[随着时间推移的 Solana 区块](https://dune.xyz/queries/389979) 和 [每日交易](https://dune.xyz/queries/390045)

| 列名              | 数据类型 | 说明                                         |
| ------------------------ | ----------- | --------------------------------------------------- |
| hash                     | string      | 此区块的哈希，base-58 编码      |
| height                   | bigint      | 该区块之下的块数            |
| slot                     | bigint      | 该区块在账本中的槽索引               |
| time                     | timestamp   | 这个区块的（估计）生成时间        |
| date                     | date        | 用于分区                                |
| parent\_slot             | bigint      | 该区块的父区块的槽索引               |
| previous\_block_\__hash  | string      | 此区块的父区块的哈希，base-58 编码    |
| total\_transactions      | bigint      | 该区块的交易总数      |
| successful\_transactions | bigint      | 该区块成功交易的数量 |
| failed\_transactions     | bigint      | 该区块中失败的交易数量    |

## Solana.transactions

该表包含 Solana 区块链中的交易数据。此表中提供了与帐户、协议和程序活动相关的大多数相关数据。

查询示例可以在这里找到：[过去 7 天热门项目的 NFT 交易](https://dune.xyz/queries/390720/745376) 和 [drift-protocol 概述](https://dune.xyz/bigz/drift-\(solana\))

| 列名                    | 数据类型                   | 说明                                                                                             |
| ------------------------------ | ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| block\_slot                    | bigint                        | 该区块在账本中的槽索引                                                                                                                                                                                                 |
| block\_time                    | timestamp                     | 这个区块的（估计）生成时间                                                                                                                                                                                          |
| block\_date                    | date                          | 事件日期                                                                                                                                                                                                                            |
| index                          | bigint                        | 索引区块的交易                                                                                                                                                                                                   |
| fee                            | bigint                        | 此交易收取的费用，由第一个帐户支付                                                                                                                                                                            |
| block\_hash                    | string                        | 此区块的哈希，base-58 编码                                                                                                                                 |
| error                          | STRUCT error                  | 如果 success = true 则为 NULL true.                                                                                                                                                                                                              |
| required\_signatures           | bigint                        | 使交易有效所需的签名总数。                                                                                                                                                               |
| readonly\_signed_\__accounts   | bigint                        | 签名密钥的最后一个 readonly\_signed\_accounts 是只读帐户。                                                                                                                                                       |
| readonly\_unsigned_\__accounts | bigint                        | 未签名密钥的最后一个 readonly\_unsigned\_accounts 是只读帐户。                                                                                                                                                   |
| id                             | string                        | 交易哈希                                                                                                                                                                                                |
| success                        | boolean                       | 交易是否成功                                                                                                                                                                                        |
| recent\_block_\__hash          | string                        | 账本中最近区块的哈希值，用于防止交易重复并赋予交易生命周期                                                                                                                 |
| instructions                   | array\<STRUCT instructions>   | 执行的指令数组（按顺序）                                                                                                                                                                                                    |
| accountKeys                    | array\<string>                | 交易中使用的帐户地址                                                                                                                                                                                              |
| log\_messages                  | array\<string>                | 事务发出的日志消息                                                                                                                                                                                           |
| pre\_balances                  | array\<bigint>                | 处理交易之前的账户余额数组。第 i 个余额是 account\_keys 中第 i 个账户的余额                                                                                              |
| post\_balances                 | array\<bigint>                | 处理交易后的账户余额数组。第 i 个余额是 account\_keys 中第 i 个账户的余额                                                                                              |
| pre\_token\_balance            | array\<STRUCT token\_balance> | 交易处理前的 [代币余额](https://docs.solana.com/developing/clients/jsonrpc-api#token-balances-structure) 列表，如果在此交易期间尚未启用代币余额记录，则省略 |
| post\_token\_balance           | array\<STRUCT token\_balance> | 交易处理后的 [代币余额](https://docs.solana.com/developing/clients/jsonrpc-api#token-balances-structure) 列表，如果在此交易期间尚未启用代币余额记录，则省略  |
| signatures                     | array\<string>                | 应用于交易的 base-58 编码签名列表。总是长度为 numRequiredSignatures                                                                                                                               |
| signer                         | string                        | 发起交易并支付交易费用的 account\_keys 数组的初始值                                                                                                                           |

### 结构定义

在其中的几个列中是一种数据类型 STRUCT，它允许表示嵌套的分层数据并具有键值对。它类似于 python 中的字典，可用于将字段组合在一起以使它们更易于访问。

一个如何用这些结构字段提取数据的示例：[按天统计 DEXes 的 Solana 指令数](https://dune.xyz/queries/416358/794290)

**token\_balance**

| 列名   | 数据类型 | 说明                                                                                                                                                                  |
| ------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account | string    | 提供令牌余额的帐户的帐户地址。                                                                                                      |
| mint    | string    | 代币铸造合约的公钥。这是一个存储有关代币元数据的帐户：供应量、小数位数以及控制铸造的各种权限。|
| amount  | Decimal   | 源自代币余额的原始金额 (ui\_token\_amount.amount) 和小数位数 (ui\_token\_amount.decimals)                                              |

****

**instructions**

| 列名               | 数据类型                         | 说明                                                   |
| ------------------- | ---------------------------------- | -------------------------------------------------------------- |
| account\_arguments  | array\<string>                     | 要传递给程序的帐户的有序列表                |
| data                | string                             | 以 base-58 编码的程序输入数据                        |
| executing\_account  | string                             | 执行此指令的程序的帐户地址 |
| inner\_instructions | array\<STRUCT inner\_instructions> | 该指令调用的指令                 |

****

**inner\_instructions**

| 列名              | 数据类型      | 说明                                                     |
| ------------------ | -------------- | -------------------------------------------------------------- |
| account\_arguments | array\<string> | 要传递给程序的帐户的有序列表               |
| data               | string         | 以 base-58 编码的程序输入数据                        |
| executing\_account | string         | 执行此指令的程序的帐户地址 |

****

**error**

| 列名                  | 数据类型 | 说明                         |
| ---------------------- | --------- | ---------------------------------- |
| **instruction\_index** | int       | 失败的指令号      |
| message                | string    | 错误信息                  |

## Solana.vote\_transactions

该表包含验证者提交的用于对区块进行投票的全部投票交易。它可以与上面的非投票交易表结合起来，以获得所有交易的完整细分。它与主事务表（transactions）具有相同的架构。

此处提供了一个演示示例查询：[Solana 过去 30 天的交易](https://dune.xyz/queries/389976/743760)

## Solana.rewards

此表包含有关在 Solana 上支付的奖励的数据。一个区块可能包含零个或多个奖励，每一行对应一个奖励。

可以在此处找到示例查询：[Solana 每天奖励费用](https://dune.xyz/queries/391421/747012)

| 列名   | 数据类型 | 说明                                                                                       |
| ------------- | ----------- | ------------------------------------------------------------------------------------------------- |
| block\_slot   | bigint      | 该区块在账本中的槽索引                                                             |
| block\_hash   | string      | 此区块的哈希，base-58 编码                                                           |
| block\_time   | timestamp   | 这个区块的（估计）生成时间                                                      |
| block\_date   | date        | 事件日期                                                                                        |
| commission    | string      | 奖励入账时的投票账户佣金，仅用于投票和质押奖励 |
| lamports      | bigint      | 账户贷记或借记的奖励 lamports 数量                                     |
| pre\_balance  | bigint      | 应用奖励前的账户余额（lamports）                                        |
| post\_balance | bigint      | 应用奖励后的账户余额（lamports）                                          |
| recipient     | string      | 收到奖励的账户的公钥，以 base-58 编码的字符串               |
| reward\_type  | string      | 奖励类型："fee", "rent", "voting", "staking"                                                |

## Solana.account\_activity

此表包含来自交易表中的信息，主要关注帐户使用情况。每行包含有关交易中帐户使用情况的所有信息。

| 列名              | 数据类型 | 说明                                                       |
| ------------------------ | ----------- | ---------------------------------------------------------------- |
| block\_slot              | bigint      | 该交易所在区块的槽位。                 |
| block\_hash              | string      | 该交易所在区块的哈希值                   |
| block\_time              | timestamp   | 此帐户使用发生的时间戳                  |
| block\_date              | date        | 此帐户使用发生的日期                            |
| address                  | string      | 账户地址，也称为公钥       |
| tx\_index                | int         | 该交易在区块中的索引                      |
| tx\_id                   | string      | 发生此帐户使用的交易的 ID   |
| tx\_success              | boolean     | 事务是否成功并已提交                     |
| signed                   | boolean     | 此帐户是否签署了此交易                             |
| writeable                | boolean     | 此帐户是否在此交易中被授予读写权限   |
| pre\_balance             | bigint      | 该账户在交易处理前的余额 |
| pre\_token_\__balance    | decimal     | 交易处理前的代币余额           |
| post\_balance            | bigint      | 交易处理后该账户的余额  |
| post\_token_\__balance   | decimal     | 交易处理后的代币余额            |
| balance\_change          | bigint      | 作为交易的一部分发生的余额变化      |
| token\_balance_\__change | decimal     | 作为交易的一部分发生的代币余额变化      |
