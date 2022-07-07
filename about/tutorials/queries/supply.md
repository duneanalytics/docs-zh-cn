# 代币在一段时间内的总供应量



你可以使用该token的合约表，只要他们被解码([decoded](../../../data-tables/data-tables/decoded-data.md)):

```sql
SELECT
week,
SUM(transfer) over (order by week)
FROM 
 (
    SELECT
    date_trunc('week', evt_block_time) as week,
    sum(amount/1e18) as transfer
    FROM ptokens."pBTC_evt_Minted" tr
    GROUP BY 1
UNION
    SELECT
    date_trunc('week', evt_block_time) as week,
    sum(-amount/1e18) as transfer
    FROM ptokens."pBTC_evt_Burned" tr
    GROUP BY 1
) as net;
```

或者你可以使用一些更通用的方式查询，就像这个：
（请注意，虽然这对大多数ERC20代币都有效，但有些代币的结构有细微变化，这个查询就会不起作用。）

```sql
Select 
   sum(amount),
    day
   
from erc20."view_token_balances_daily"
where token_address = '\x429881672B9AE42b8EbA0E26cD9C73711b891Ca5'
and wallet_address != '\x0000000000000000000000000000000000000000' --mint address
and wallet_address != '\x000000000000000000000000000000000000dead' --burn address
group by 2

--mint and burn address are not standardized, make sure to query for the right ones
```

