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

Here `contract_address` refers to the contract emmitting the event, i.e. the token address, while `evt_tx_hash` and `evt_index` conveniently lets you join with `ethereum.logs` using a query like

```
SELECT *
FROM erc20."ERC20_evt_Approval" apps
INNER JOIN ethereum.logs 
ON apps.evt_tx_hash = logs.tx_hash AND apps.evt_index = logs.index
LIMIT 100;
```

Also note that you can join these tables with `erc20.tokens` to get human readable token symbols and the number of decimals like

```
SELECT value/10^decimals, tr.*
FROM erc20."ERC20_evt_Transfer" tr 
LEFT JOIN erc20.tokens t
ON t.contract_address = tr.contract_address
WHERE symbol = 'MKR'
LIMIT 10
```

Note though that

**Examples**

**Top token holders**

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

**Token Balances**

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

## Fallback decoding <a href="#fallback-decoding" id="fallback-decoding"></a>

The above tables are generated with a new feature we call “fallback decoding”. Essentially it breaks down to being able to decode logs regardless of the events contract address or contract bytecode. If you know other cases where this decoding can be useful feel free to let us know at [hello@dune.xyz](mailto:hello@dune.xyz)

## Misc <a href="#misc1" id="misc1"></a>

* Data for Gnosis sight, safe and dfusion can now be found in `gnosis_sight`, `gnosis_safe` and `gnosis_dfusion` schemas respectively.
* Synthetix token-contracts now have the correct name `Synthetix` and are found in the `synthetix` schema
* `prices.usd_dai` have been renamed to `prices.usd_sai`, and the symbol changed to `SAI` - we still don’t have `DAI` prices unfortunately, as our price provider has not listed it.
* `prices.usd_rep` now has the correct token address for `REP` for entries after the migration. Previsouly all entries had the old address.
