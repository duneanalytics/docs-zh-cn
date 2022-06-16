# 代币标准

## 接口

当我们对触发的事件子集感兴趣时，无论原始合约如何，Dune 都会使用接口解码。值得注意的例子包括 erc20、erc721 和 erc1155 转移事件。此方法保留用于特殊情况。这些表格让跟踪 ERC20 代币和 NFT 流入和流出合约和钱包变得轻松，在 Dune 上得到广泛使用。

**erc20."ERC20\_evt\_Transfer"**

| **列名**        | **数据类型** | **描述**                                                                                                |
| ------------------ | ------------- | -------------------------------------------------------------------------------------------------------------- |
| from               | bytea         | 交易发送者                                                                                       |
| to                 | bytea         | 交易接收者                                                                                      |
| value              | numeric       | 发送的 erc20 代币数量。请注意，您已将其除以 erc20 代币的相关小数位。|
| contract\_address  | bytea         | erc20 代币的合约地址                                                                      |
| evt\_tx\_hash      | bytea         | 交易哈希                                                                                           |
| evt\_index         | numeric       | 交易索引                                                                                          |
| evt\_block\_time   | timestamptz   | 交易发生时间                                                                    |
| evt\_block\_number | int8          | 区块链的长度（区块编号）                                                                                  |

[查看此表的实际操作](https://dune.xyz/queries/39012)

**erc721."ERC721\_evt\_Transfer"**

| **列名**    | **数据类型** | **描述**                                 |
| ------------------ | ------------- | ----------------------------------------------- |
| from               | bytea         | 交易发送者                         |
| to                 | bytea         | 交易接收者                        |
| tokenID            | numeric       | 唯一标识此 NFT 的 Token ID |
| contract\_address  | bytea         | erc721 代币的合约地址       |
| evt\_tx\_hash      | bytea         | 交易哈希                            |
| evt\_index         | numeric       | 交易索引                            |
| evt\_block\_time   | timestamptz   | 交易发生时间      |
| evt\_block\_number | int8          | 区块链的长度（区块编号）                    |

[查看此表的实际操作](https://dune.xyz/queries/38974)

您可以使用此查询查询特殊表：

**已根据“接口”解码的合约列表**

```sql
SELECT * FROM ethereum."contracts" WHERE address IS NULL;
```
