# 价格查询

### 中心化交易所价格数据

```sql
Select 
    minute, 
    price, 
    symbol 
from prices.usd

WHERE symbol = '{{1. Symbol}}'

and minute > now() - interval '1hour'
```

### 去中心化交易所价格数据

```sql
Select 
    hour, 
    median_price, 
    d."contract_address"
from dex."view_token_prices" d
left join erc20.tokens e on d.contract_address = e.contract_address
WHERE symbol = '{{1. Symbol}}'

and hour > now() - interval '1000hour'
```

这个查询以来erc20.tokens表，当该小时内没有提交的交易时，可能会出现空隙。

对于一个更好但更复杂的版本，使用这个：

```sql
with dex_trades AS (
        SELECT 
            token_a_address as contract_address, 
            usd_amount/token_a_amount as price,
            block_time
        FROM dex.trades
        WHERE 1=1
        AND usd_amount  > 0
        AND category = 'DEX'
        AND token_a_amount > 0
        AND token_a_address  = CONCAT('\x', substring('{{1. token_address}}' from 3))::bytea
        
        UNION ALL 
        
        SELECT 
            token_b_address as contract_address, 
            usd_amount/token_b_amount as price,
            block_time
        FROM dex.trades
        WHERE 1=1
        AND usd_amount  > 0
        AND category = 'DEX'
        AND token_b_amount > 0
        AND token_b_address  = CONCAT('\x', substring('{{1. token_address}}' from 3))::bytea
        
    ),
    
    
    rawdata as (

    SELECT 
        date_trunc('hour', block_time) as hour,
        d.contract_address,
        e.symbol as asset,
        (PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY price)) AS price,
        count(1) AS sample_size
    FROM dex_trades d
    left join erc20.tokens e on e.contract_address = d.contract_address
    GROUP BY 1, 2, 3

    )

    ,leaddata as 
    (
    SELECT
    hour,
    contract_address,
    asset,
    price,
    sample_size,
    lead(hour, 1, now() ) OVER (PARTITION BY contract_address ORDER BY hour asc) AS next_hour
    from rawdata
    where sample_size > 3
    )
    
    ,hours AS
    (
    SELECT
    generate_series('2020-01-01'::TIMESTAMP, date_trunc('hour', NOW()), '1 hour') AS hour
    )


    
    SELECT
    d.hour as hour,
    contract_address,
    asset,
    price,
    sample_size
    from leaddata b
    INNER JOIN hours d ON b.hour <= d.hour
    AND d.hour < b.next_hour -- Yields an observation for every hour after the first transfer until the next hour with transfer
```

这将根据dex交易数据按小时返回价格，并填补没有进行过交易的空白行。你可以在第53行调整所需的交易样本量，以便获取价更好的格数据。这可以帮助修正arbritrage机器人在dexes上获得奇怪价格的异常情况。如果你有这方面的问题，请在Discord上与我们联系。