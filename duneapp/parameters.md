---
说明: >-
 参数是一种简单而强大的方式，可以将变量添加到你的查询/仪表板上。你和其他人可以在不接触代码的情况下定制查询和仪表板。
---

# 参数
## 什么是参数？

**参数是dune的一个专门功能，允许你在查询代码的某些部分实现变量。这个变量可以从仪表板上改变，因此可以让你做一个互动的仪表板。**

参数允许你通过几个简单的点击对你的代码的某些定义参数进行修改。例如，代替硬编码“合约_地址”，“符号”或“日期范围”，你可以只使用参数功能来改变你这部分代码。这允许你建立一个交互式仪表板或可定制的查询，浏览者可以用它来查询他所需要的数据。

参数在查询代码中被定义为`{{参数名}}，并将出现在查询的下方和任何使用了参数的查询可视化的仪表板中。

你可以在查询下方或仪表板上的参数字段中传递对参数的输入。\
\
只需运行查询，就可以在查询编辑器中为查询应用该参数。\
\
在仪表板上，你可以点击顶部的 "全部应用 "或单独改变参数并点击 "回车"。回车 "的提交方式也适用于下拉菜单和日期选择器。

仪表板中的参数可以在不同的查询之间共享，只要确保在所有的查询之间使用相同的名称、类型和默认值。

![](<../.gitbook/assets/image (30).png>)

![](<../.gitbook/assets/image (27) (1) (2) (2).png>)

![](<../.gitbook/assets/image (29).png>)

## 如何使用参数？

You can simply add a parameter to your queries by writing `{{parametername}}` or using the button below the query.&#x20;

You can edit the properties of single parameters by clicking on the gear wheel next to the parameter in the query editor. This allows you to set a default value, define a list of possible parameters or change the type of of the parameter. If you want to share parameters between different queries on a dashboard make sure they exactly match in regards to name, type and default value.

![](<../.gitbook/assets/2021-12-10 15-41-07.gif>)



## Example Query

This query returns the running total of Gas Paid in USD.

The Query Author has chosen to include a parameter for `wallet address`, `start date` and `end date`.

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

Find this query [here](https://dune.xyz/queries/64430/128463)

## Embedding parameterized Dashboards and Queries



## **Example Dashboards**

**Find interesting stats on Ethereum Wallets with this dashboard:**\
[https://dune.xyz/kevdnlol/Transaction-Breakdown](https://dune.xyz/kevdnlol/Transaction-Breakdown)

_The author has included the parameters `wallet address`, `start date` and `end date` in this Dashboard._

**Drill down into the single pools of Barnbridge's Smart Yield Product:**\
[https://dune.xyz/0xBoxer/Barnbridge-or-Smart-Yield](https://dune.xyz/0xBoxer/Barnbridge-or-Smart-Yield)

_The Author has chosen to make the parameter `poolsymbol` into a drop down list here. This allows for easy access to all the relevant pools and detailed statistics on those._

**Find out how many people are participating in Yearn Vaults:**\
[https://dune.xyz/msilb7/Yearn-How-Many-Addresses-are-Participating](https://dune.xyz/msilb7/Yearn-How-Many-Addresses-are-Participating)

**Find out how your investment in $KLIMA is doing:**

****[**https://dune.xyz/0xrusowsky/KLIMA-Wallet-Activity**](https://dune.xyz/0xrusowsky/KLIMA-Wallet-Activity)****

## Summary

Parameters allow you to make a certain part of your SQL query dynamic and thereby offer you to make queries and dashboards interactive. That way you can easily display detailed data on your dashboard since it allows the viewer to customize the dashboard for his needs.\
You could think of parameters like filters, but the possibilities of using this feature go beyond that.
