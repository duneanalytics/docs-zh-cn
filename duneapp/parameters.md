---
说明: >-
 参数是一种简单而强大的方式，可以将变量添加到你的查询/仪表盘上。你和其他人可以在不接触代码的情况下定制查询和仪表盘。
---

# 参数
## 什么是参数？

**参数是dune的一个专门功能，允许你在查询代码的某些部分实现变量。这个变量可以从仪表盘上改变，因此可以让你做一个互动的仪表盘。**

参数允许你通过几个简单的点击对你的代码的某些定义参数进行修改。例如，代替硬编码“合约_地址”，“符号”或“日期范围”，你可以只使用参数功能来改变你这部分代码。这允许你建立一个交互式仪表盘或可定制的查询，浏览者可以用它来查询他所需要的数据。

参数在查询代码中被定义为`{{参数名}}，并将出现在查询的下方和任何使用了参数的查询可视化的仪表盘中。

你可以在查询下方或仪表盘上的参数字段中传递对参数的输入。\
\
只需运行查询，就可以在查询编辑器中为查询应用该参数。\
\
在仪表盘上，你可以点击顶部的 "全部应用 "或单独改变参数并点击 "回车"。回车 "的提交方式也适用于下拉菜单和日期选择器。

仪表盘中的参数可以在不同的查询之间共享，只要确保在所有的查询之间使用相同的名称、类型和默认值。

![](<../.gitbook/assets/image (30).png>)

![](<../.gitbook/assets/image (27) (1) (2) (2).png>)

![](<../.gitbook/assets/image (29).png>)

## 如何使用参数？

你可以通过写“{{参数名}}” 或使用查询下面的按钮，简单地在你的查询中添加一个参数。&#x20;

你可以通过点击查询编辑器中参数旁边的齿轮来编辑单个参数的属性。你可以设置一个默认值，定义一个可能的参数列表或改变参数的类型。如果你想在一个仪表盘上的不同查询之间共享参数，请确保它们在名称、类型和默认值方面完全匹配。

![](<../.gitbook/assets/2021-12-10 15-41-07.gif>)



## 查询示例

该查询返回以美元计算的已付gas费的总额。

查询作者选择 "钱包地址"、"开始日期 "和 "结束日期"作为参数。

```sql
with alltransactions
AS (
SELECT 
    block_time, 
    success, 
    gas_price/10^9 AS gas_prices, 
    gas_used,
    (gas_price*gas_used)/10^18 AS eth_paid_for_tx,
    hash
FROM ethereum.transactions
WHERE "from" = CONCAT('\x', substring('{{1. Eth Address}}' from 3))::bytea
AND block_time >= '{{2. Start Date}}'
AND block_time < '{{3. End Date}}')

SELECT
    date_trunc('minute', block_time),
    SUM(eth_paid_for_tx*price) over (ORDER BY date_trunc('minute', block_time)) AS "Total Gas Fees Paid in USD"
FROM alltransactions
LEFT JOIN 
    (SELECT
        minute,
        price
    FROM prices.usd
    WHERE 
        symbol = 'WETH' AND
        minute > '{{2. Start Date}}') AS prices
ON date_trunc('minute', block_time) = minute
ORDER BY block_time DESC
```

可以在[这里]找到这个查询。(https://dune.xyz/queries/64430/128463)

## 嵌入参数化仪表盘和查询



## **仪表盘示例**

**通过这个仪表盘找到关于以太坊钱包的有趣统计数据：**\
[https://dune.xyz/kevdnlol/Transaction-Breakdown](https://dune.xyz/kevdnlol/Transaction-Breakdown)

_作者在该仪表盘中包含了 "钱包地址"、"开始日期 "和 "结束日期 "等参数。_

**深入研究Barnbridge's Smart Yield Product:**\
[https://dune.xyz/0xBoxer/Barnbridge-or-Smart-Yield](https://dune.xyz/0xBoxer/Barnbridge-or-Smart-Yield)

_作者选择在这里将参数 "poolsymbol "变成一个下拉列表。这样就可以方便地访问所有相关的池子，并对这些池子进行详细的统计。_

**了解有多少人在参与Yearn Vaults：**\
[https://dune.xyz/msilb7/Yearn-How-Many-Addresses-are-Participating](https://dune.xyz/msilb7/Yearn-How-Many-Addresses-are-Participating)

**了解你的$KLIMA投资:**

****[**https://dune.xyz/0xrusowsky/KLIMA-Wallet-Activity**](https://dune.xyz/0xrusowsky/KLIMA-Wallet-Activity)****

## 总结

参数允许你SQL查询的某一部分成为动态的，从而提供你和查询、仪表盘互动的机会。这样就可以很容易地在仪表盘上让许浏览者根据自己的需要定制仪表盘。\
你可以认为参数就像过滤器，但使用功能的可能性超出了这个范围。
