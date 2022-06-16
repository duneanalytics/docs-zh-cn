# xDai 数据

### xDai.Blocks

| **列名**   | **数据类型** | **说明**                                                        |
| ----------------- | ------------ | ---------------------------------------------------------------------- |
| time              | timestamptz  | 区块被开采的时间。                                   |
| number            | numeric      | 区块链的长度（以块为单位）                                 |
| hash              | bytea        | 该区块的唯一标识符                                     |
| parent hash       | bytea        | 前一个区块的唯一标识符                              |
| gas\_limit        | numeric      | 当前区块的gas限制                                     |
| gas\_used         | numeric      | 此块中使用的gas数量                                             |
| miner             | bytea        | 矿工的地址                                               |
| difficulty        | numeric      | 开采区块所需的难度值                                  |
| total\_difficulty | numeric      | 直到这个区块链的总难度值                         |
| nonce             | bytea        | 区块随机数，用于展示挖矿过程中的工作量证明 |
| size              | numeric      | 此块的大小（以字节为单位）（受限于gas limit）                     |

###

### xDai.Logs

该表存储了智能合约生成的所有日志。它有时对于查询尚未解码或抗解码的合约很有用（我们需要一个 abi 密钥才能解码）。

日志是一种以低廉的价格在以太坊区块链上存储少量数据的优雅方式。具体来说，事件日志有助于让其他人知道发生了什么事情，而不必单独查询合约。

有关此主题的更多信息，请阅读 [这篇文章](https://medium.com/mycrypto/understanding-event-logs-on-the-ethereum-blockchain-f4ae7ba50378)。

| **列名**   | **数据类型** | **说明**                                  |
| ----------------- | ------------ | ------------------------------------------------ |
| block\_hash       | bytea        | 该区块的唯一标识符              |
| block\_number     | int8         | 区块链的长度（以块为单位）           |
| block\_time       | timestamptz  | 区块被开采的时间                |
| contract\_address | bytea        | 发出日志的合约地址 |
| data              | bytea        | 事件数据                                      |
| topic1            | bytea        | 事件的索引主题1                     |
| topic2            | bytea        | 事件的索引主题2                     |
| topic3            | bytea        | 事件的索引主题3                     |
| topic4            | bytea        | 事件的索引主题4                     |
| index             | numeric      | 区块中的交易索引位置     |
| tx\_hash          | bytea        | 事件的交易哈希                |
| tx\_index         | numeric      | 交易索引                     |

### xDai.Transactions

| **列名** | **数据类型** | **说明**                                                                                  |
| --------------- | ------------ | ------------------------------------------------------------------------------------------------ |
| block\_time     | timestamptz  | 区块被开采的时间                                                                |
| nonce           | numeric      | 该钱包独有的交易随机数                                                    |
| index           | numeric      | 区块中的交易索引位置                                                     |
| success         | boolean      | 显示事务是否成功的真/假值                                       |
| from            | bytea        | 发送者的地址                                                                            |
| to              | bytea        | 接收者的地址。当是合约创建交易时为空（NULL）                           |
| value           | numeric      | 在此交易中发送的以 wei 为单位的以太币数量。请注意，erc20 代币不会出现在这里。|
| block\_number   | int8         | 区块链的长度（以块为单位）                                                          |
| block\_hash     | bytea        | 该区块的唯一标识符                                                               |
| gas\_limit      | numeric      | 以 wei 为单位的 gas 限制                                                                            |
| gas\_price      | numeric      | 以 wei 为单位的 gas 价格                                                                             |
| gas\_used       | numeric      | 以 wei 为单位的交易中消耗的 gas 数量                                                     |
| data            | bytea        | 事件数据                                                                                      |
| hash            | bytea        | 交易哈希                                                                     |

### xDai.Traces

事务可以触发修改以太坊虚拟机内部状态的更小的原子操作。有关这些操作执行的信息会被记录下来，并且可以存储为 EVM 执行跟踪，或者简称_跟踪（trace）_。

在[这里](https://medium.com/chainalysis/ethereum-traces-not-transactions-3f0533d26aa) 阅读更多信息.

| **列名** | **数据类型** | **说明**                                                                                  |
| --------------- | ------------ | ------------------------------------------------------------------------------------------------ |
| block\_time     | timestamptz  | 区块被开采的时间                                                                |
| tx\_success     | boolean      | 显示事务是否成功的真/假值                                      |
| success         | boolean      | 显示跟踪（trace）是否成功的真/假值                                      |
| block\_hash     | bytea        | 该区块的唯一标识符                                                               |
| block\_number   | int8         | 区块链的长度（以块为单位）                                                           |
| tx\_hash        | bytea        | 事件的交易哈希                                                               |
| from            | bytea        | 发送者的地址                                                                            |
| to              | bytea        | 接收者的地址。当是合约创建交易时为空（NULL）                           |
| value           | numeric      | 在此交易中发送的以 wei 为单位的以太币数量。请注意，erc20 代币不会出现在这里。|
| gas             | numeric      | gas 限制                                                                                  |
| gas\_used       | numeric      | 以 wei 为单位的交易中消耗的 gas 数量                                                       |
| tx\_index       | numeric      | 交易索引                                                                     |
| trace\_address  |              | 跟踪地址                                                                                      |
| sub\_traces     |              | 子跟踪的数量                                                                                        |
| type            | text         | 操作类型                                                                                   |
| address         | bytea        | 当类型为自毁或创建时调用的合约地址                                  |
| code            | bytea        | 代码                                                                                           |
| call\_type      | bytea        | 调用类型                                                                             |
| input           | bytea        | 输入数据                                                                                         |
| output          | bytea        | 输出数据                                                                                         |
| refund\_address | bytea        | 退款地址                                                                                          |

**在使用 Dune 进行分析时，您可能不会过多地使用这些（请参阅** [**已解码数据**](../decoded-data.md)**），但拥有它们以防万一总是好的。**
