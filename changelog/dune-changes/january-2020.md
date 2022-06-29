# January 2020

## Postgres 12.1 <a href="#postgres-121" id="postgres-121"></a>
我们已将数据库从 postgres 11 升级到 postgres 12.1。这应该会使性能得到全面的改进。



##ERC20 转移和授权表<a href="#erc20-transfer-and-approval-tables" id="erc20-transfer-and-approval-tables"></a>

您现在可以查询`erc20."ERC20_evt_Transfer"`和`erc20."ERC20_evt_Approval"`表以获取解码的Token转移和授权表。这些表回包括可以使用 ERC20 标准的 ABI 解码的所有事件。这意味着所有代币的所有转账都可以通过这张表查询。该表很大（撰写本文时为240M行），因此请告诉我们您对查询性能的体验。


`erc20."ERC20_evt_Transfer"` schema:

| Column             | Type      |
| ------------------ | --------- |
| `"from"`           | `bytea`   |
| `"to"`             | `bytea`   |
| `value`            | `numeric` |
| `contract_address` | `bytea`   |
| `evt_tx_hash`      | `bytea`   |
| `evt_index`        | `bigint`  |

`erc20."ERC20_evt_Approval"` schema:

| Column             | Type      |
| ------------------ | --------- |
| `owner`            | `bytea`   |
| `spender`          | `bytea`   |
| `value`            | `numeric` |
| `contract_address` | `bytea`   |
| `evt_tx_hash`      | `bytea`   |
| `evt_index`        | `bigint`  |

这里的`contract_address`是指发出事件的合约，即代币地址，`evt_tx_hash`和`evt_index`可以方便地让您使用类似的查询关联`ethereum.logs`。


```
SELECT *
FROM erc20."ERC20_evt_Approval" apps
INNER JOIN ethereum.logs 
ON apps.evt_tx_hash = logs.tx_hash AND apps.evt_index = logs.index
LIMIT 100;
```

另请注意，您可以将这些表与`erc20.tokens`连接起来，以获得人类可读的Token符号和小数位数，如

```
SELECT value/10^decimals, tr.*
FROM erc20."ERC20_evt_Transfer" tr 
LEFT JOIN erc20.tokens t
ON t.contract_address = tr.contract_address
WHERE symbol = 'MKR'
LIMIT 10
```

**例子**

**Token头部持有者**

```
WITH transfers AS (
    SELECT
    evt_tx_hash AS tx_hash,
    tr."from" AS address,
    -tr.value AS amount
     FROM erc20."ERC20_evt_Transfer" tr
     WHERE contract_address = '\x6810e776880c02933d47db1b9fc05908e5386b96' --GNO
UNION ALL
    SELECT
    evt_tx_hash AS tx_hash,
    tr."to" AS address,
    tr.value AS amount
     FROM erc20."ERC20_evt_Transfer" tr 
     WHERE contract_address = '\x6810e776880c02933d47db1b9fc05908e5386b96' --GNO
)
SELECT address, sum(amount/1e18) as balance
FROM transfers
GROUP BY 1
ORDER BY 2 desc
LIMIT 10
```

**Token余额**

```
WITH transfers AS (
    SELECT
    evt_tx_hash AS tx_hash,
    contract_address AS token_address,
    -tr.value AS amount
     FROM erc20."ERC20_evt_Transfer" tr
     WHERE tr."from" = '\xdeadbeef' -- insert real address here
UNION ALL
    SELECT
    evt_tx_hash AS tx_hash,
    contract_address AS token_address,
    tr.value AS amount
     FROM erc20."ERC20_evt_Transfer" tr 
     WHERE tr."to" = '\xdeadbeef' -- insert real address here
)
SELECT token_address, sum(amount/1e18) as balance
FROM transfers
GROUP BY 1
ORDER BY 2 desc;
```

## 反向解码 <a href="#fallback-decoding" id="fallback-decoding"></a>

上面的表格是使用我们称为“反向解码”的新功能生成的。本质上，它能拆解为为能够被解码的日志，而无所谓事件的合约地址或合约字节码。如果您知道此解码可能有用的其他情况，请随时通过  [hello@dune.xyz](mailto:hello@dune.xyz)告诉我们

## 其他 <a href="#misc1" id="misc1"></a>

* 现在可以分别在`gnosis_sight`、`gnosis_safe` 和`gnosis_dfusion`模式中找到 Gnosis vision、safe 和 dfusion 的数据。
* Synthetix 代币合约现在有了正确的名称`Synthetix`并且可以在`synthetix`模式中找到
* `prices.usd_dai`已重命名为`prices.usd_sai`，Symbol更改为 SAI - 不幸的是，我们仍然没有 DAI 价格，因为我们的价格提供者没有提供。
* `prices.usd_rep`现在为迁移后的条目提供了正确的`REP`Token地址。以前所有条目都有旧地址。





