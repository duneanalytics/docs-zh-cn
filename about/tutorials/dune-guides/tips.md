# 查询技巧

你可以通过我们的界面与数据表进行互动，网址是 [dune.xyz](https://www.dune.xyz/).

要创建一个新的查询，你只需点击右上角的`New Query`。

![](https://i.imgur.com/dMHavC8.png)

在你的左边，你可以在下拉列表中选择你要使用的数据库，然后在窗口中看到数据表。只要搜索你感兴趣的项目就可以了。

## 使用抽象表（abstractions）

使用Dune Analytics进行分析的最简单方法是使用准备好的[abstractions](.../.../data-tables/data-tables/abstractions.md)表，如`dex.trades`。所有的表都是经过清理的，包含了数据和元数据（如人类可读的标记符号），使它们查询会非常便捷。

## 在查询中使用以太坊地址

在Dune中，以太坊地址被存储为postgres字节数，并以`x`前缀进行编码。这与习惯的`0x`前缀不同。如果你想使用一个地址，比如说过滤一个给定的代币，你可以这样做

```sql
WHERE token = '\x6b175474e89094c44da98b954eedeac495271d0f'
```

是下面的简洁版本

```sql
WHERE token = '\x6b175474e89094c44da98b954eedeac495271d0f'::bytea
```

## 引用驼峰字母的列名和表名

列和表的名称大多直接取自智能合约的ABI，没有任何修改。由于大多数智能合约是用Solidity编写的，而且是用驼峰命名惯例编写的，所以Dune的许多表和列名也是如此。Postgres要求你用双引号去区分的列和表的大小写。

```sql
SELECT “columnName”
FROM projectname.”contractName_evt_EventName”
LIMIT 10
```

在Postgres中，双引号是为表和列保留的，而单引号是为值保留的：

```sql
SELECT “columnName”
FROM projectname.”contratName_evt_eventName”
WHERE contract_address = '\x6B175474E89094C44Da98b954EedeAC495271d0F'
LIMIT 10
```

模式(schemas)在Dune中总是小写的。

## Remove decimals

以太币转账和大多数ERC-20代币都是18位数字。为了得到一个更适合人类阅读的数字，你需要去掉所有的代表小数的0。表`erc20.tokens`为你提供了流行代币的合约地址、符号和小数点的数量。然后将代币价值转移除以10的小数点幂。

`transfer_value / 10^erc20.tokens.decimals`

## 使用`date_trunc`获得时间

我们在解码的事件表中加入了`evt_block_time`，以方便你使用。使用它的一个很好的方法是与`date_trunc`函数一起使用，像这样

```sql
SELECT date_trunc('week', evt_block_time) AS time
```

你可以用minute, day, week, month.

## 如何获得美元价格(USD)

To get the USD volume of onchain activity you typically want to join the smart contract event you are looking at with the usd price and join on minute. Also make sure that asset matches asset.
为了获得链上活动的美元交易量，你通常要将你所看的智能合约事件与美元价格就行关联，以分钟为关联的键，还要确保资产与资产相匹配。
```sql
LEFT JOIN prices.usd p 
ON p.minute = date_trunc('minute', evt_block_time)
AND event."asset" = p.contract_address
```

然后，你可以在你的`SELECT`语句中简单地将智能合约事件的价值或金额与美元价格相乘：`* p.price`。

## 代币符号

你经常想按代币地址来分组查询结果。例如，你想看到按代币查询在DEX上的交易量。然而，一大堆代币地址的列表是抽象的，难以理解的。

因此，你经常想用代币符号来代替，简单地将表`erc20.tokens`与你的事件表关联起来，使用asset=token地址。然后你就可以用代币的符号代替地址。

**NB** `erc20.tokens`表包含了一些流行的代币。如果你正在处理更多晦涩难懂的代币，你应该注意与该表的关联，因为不在coincap表中的代币可能被排除在你的结果之外。


## 用参数过滤查询和仪表盘

参数可以把你的查询或仪表板变成区块链数据的应用程序。

在查询编辑器页面上点击SQL编辑器右下方的`Add parameter`。

![](https://i.imgur.com/rYJVSqA.png)

双层大括号将出现在你的查询中`{{}}`。在这些大括号中，你可以放入你的参数名称，如 `token symbol`或`holder address`。

注意，如果你想在你的查询中使用参数`WHERE token = '{{token symbol}}'`，你需要加上单引号。

为了使用户不必为地址输入`\x`，一个有用的地址格式化是这样的。

```sql
WHERE contract_address = CONCAT('\x', substring('{{token address}}' from 3))::bytea
```

这让你的查询用户在过滤时只需粘贴`0xc00e94cb662c3520282e6f5717214004a7f26888`而不是`\xc00e94cb662c3520282e6f5717214004a7f26888`。
