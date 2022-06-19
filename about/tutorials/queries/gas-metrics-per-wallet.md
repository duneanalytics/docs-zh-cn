# 每个钱包的gas花费

### 总的gas费用(USD)

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

### 总的gas费用(ETH)

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
    block_time, 
    SUM(eth_paid_for_tx) over (ORDER BY block_time ASC) as "ETH Paid"
FROM alltransactions 
ORDER BY block_time DESC
```

### 平均gas费用

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
    AVG(gas_prices) as "Average Gas Prices"
FROM alltransactions
```

### 平均gas费用(USD)

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
    AVG(eth_paid_for_tx*price) AS "AVG Gas Fee per TX Paid in USD"
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
```

### 最高gas费用(USD)

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
    MAX(eth_paid_for_tx*price) AS "Higest Gas Fee Paid (USD)"
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
```

### 每月gas费用(USD)

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
    date_trunc('month', block_time),
    SUM(eth_paid_for_tx*price) AS "Total Gas Fees Paid in USD"
FROM alltransactions
LEFT JOIN 
    (SELECT
        minute,
        price
    FROM prices.usd
    WHERE 
        symbol = 'WETH' AND
        minute > '{{2. Start Date}}') AS prices
ON date_trunc('month', block_time) = minute
GROUP BY 1

```
