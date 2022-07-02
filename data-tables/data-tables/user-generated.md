---
说明: >-
  dune_user_generated 模式是一种在我们的数据库中构建您自己的视图、函数或表的简单方法。
---

# 用户生成表

#### 请注意，这些表不保证包含数据的准确性，如果不是您自己创建的，请谨慎使用。

**请始终为您的视图保存构造函数参数。有时我们必须删除视图以便能够更改一些解码问题或代理依赖关系，您可能必须重新部署您的视图。**

## 用例

您可以通过多种方式在 Dune 中使用自己的视图和表格，从而更轻松地在 Dune 上处理数据。

您自己的表、视图和函数都可以在 Dune 上创建内容方面发挥重要作用，如果使用得当，可以更轻松地维护仪表板和查询。

如果您不熟悉表、视图、物化视图和函数，请查阅 [pgSQL 文档](https://www.postgresqltutorial.com/postgresql-views/) 或查看我们的 [教程](../../about/tutorials/)。

### 存储信息

有时，数据提取所需的某些参考或信息未正确存储在可用表中或存储在许多不同的表中，这会使查询非常难以处理。在这些情况下，您最好将必要的信息存储在视图中并引用该视图。

这方面的一个例子是将某些金库或借贷代币映射到它们各自的基础代币。

```sql
CREATE OR REPLACE VIEW dune_user_generated.view_test (symbol, contract_address, decimals, underlying_token_address) AS VALUES
('iETH'::text, '\x9Dde7cdd09dbed542fC422d18d89A589fA9fD4C0'::bytea, 18::numeric, '\xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2'::bytea),
('yDAI'::text, '\x9D25057e62939D3408406975aD75Ffe834DA4cDd'::bytea, 18::numeric, '\x6B175474E89094C44Da98b954EedeAC495271d0F'::bytea),
('yUSDC'::text, '\xa2609b2b43ac0f5ebe27deb944d2a399c201e3da'::bytea, 6::numeric, '\xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48'::bytea),
('ySUSD'::text, '\x36324b8168f960A12a8fD01406C9C78143d41380'::bytea, 18::numeric, '\x57Ab1ec28D129707052df4dF418D58a2D46d5f51'::bytea),
('yUSDT'::text,'\xa1787206d5b1bE0f432C4c4f96Dc4D1257A1Dd14'::bytea, 6::numeric, '\xdAC17F958D2ee523a2206206994597C13D831ec7'::bytea),
('yCRV'::text,'\x9Ce551A9D2B1A4Ec0cc6eB0E0CC12977F6ED306C'::bytea, 18::numeric, '\x6B175474E89094C44Da98b954EedeAC495271d0F'::bytea),
('yBTC'::text, '\x04EF8121aD039ff41d10029c91EA1694432514e9'::bytea, 8::numeric, '\x2260FAC5E5542a773Aa44fBCfeDf7C193bc2C599'::bytea)
```

此表生成一个视图，您可以在您的查询中将它用于关联（join）。

[看看这个表](https://dune.xyz/queries/41577).

### 聚合数据

视图还可用于将多个智能合约的操作聚合到一个包含所有必要数据的视图中。

如果您一遍又一遍地使用相同的数据集并且只更改显示或聚合数据的方式，这将特别有用。这样，您不必一次又一次地查询数据集，只需将其放入视图一次，然后就可以开始引用该视图。

这将允许您更改构建数据集的基本查询，而无需逐个检查一个查询的多个不同实例。想一想，就像将您的数据收集和您使用该数据所做的实际工作/显示分成两个彼此独立运行的不同部分。

使用它将使仪表板的维护变得更加容易，因为您只需更改 **dune\_user\_generated** 视图，而不必单独处理所有查询。

一个很好的例子就是 [这个仪表板](https://dune.xyz/keeganead/cryptoart\_1) 上的几乎所有查询。创建者在 **dune\_user\_generated** 模式中创建了一个基础数据集，并使用它来作为他所有查询的基础。

请注意，虽然这种方法适用于大多数情况，但视图的算力成本可能会很高，您最好在我们的 [abstractions 抽象](abstractions.md) 中构建物化视图或表。

此示例从 uniswap\_v3 中获取数据，并对 dex.trades 表的数据进行标准化。

```sql
CREATE OR REPLACE view dune_user_generated.uniswap_v3 as 

    SELECT
        dexs.block_time,
        erc20a.symbol AS token_a_symbol,
        erc20b.symbol AS token_b_symbol,
        token_a_amount_raw / 10 ^ erc20a.decimals AS token_a_amount,
        token_b_amount_raw / 10 ^ erc20b.decimals AS token_b_amount,
        project,
        version,
        category,
        coalesce(trader_a, tx."from") as trader_a, -- 子查询依赖此 COALESCE 来避免与 transactions 表的冗余连接
        trader_b,
        token_a_amount_raw,
        token_b_amount_raw,
        coalesce(
            usd_amount,
            token_a_amount_raw / 10 ^ erc20a.decimals * pa.price,
            token_b_amount_raw / 10 ^ erc20b.decimals * pb.price
        ) as usd_amount,
        token_a_address,
        token_b_address,
        exchange_contract_address,
        tx_hash,
        tx."from" as tx_from,
        tx."to" as tx_to,
        trace_address,
        evt_index,
        row_number() OVER (PARTITION BY tx_hash, evt_index, trace_address) AS trade_id
    FROM (

        --Uniswap v3
        SELECT
            t.evt_block_time AS block_time,
            'Uniswap' AS project,
            '3' AS version,
            'DEX' AS category,
            t."recipient" AS trader_a,
            NULL::bytea AS trader_b,
            abs(amount0) AS token_a_amount_raw,
            abs(amount1) AS token_b_amount_raw,
            NULL::numeric AS usd_amount,
            f.token0 AS token_a_address,
            f.token1 AS token_b_address,
            t.contract_address as exchange_contract_address,
            t.evt_tx_hash AS tx_hash,
            NULL::integer[] AS trace_address,
            t.evt_index
        FROM
            uniswap_v3."Pair_evt_Swap" t
        INNER JOIN uniswap_v3."Factory_evt_PoolCreated" f ON f.pool = t.contract_address

        ) dexs
    INNER JOIN ethereum.transactions tx
        ON dexs.tx_hash = tx.hash
    LEFT JOIN erc20.tokens erc20a ON erc20a.contract_address = dexs.token_a_address
    LEFT JOIN erc20.tokens erc20b ON erc20b.contract_address = dexs.token_b_address
    LEFT JOIN prices.usd pa ON pa.minute = date_trunc('minute', dexs.block_time)
        AND pa.contract_address = dexs.token_a_address
    LEFT JOIN prices.usd pb ON pb.minute = date_trunc('minute', dexs.block_time)
        AND pb.contract_address = dexs.token_b_address
```

[https://dune.xyz/queries/42779](https://dune.xyz/queries/42779)

### 测试抽象（Testing Abstractions）

另一个使用“create”函数的好用例是测试你对我们的 abstractions github 存储库发出的拉取请求是否真的产生了预期的结果。只需尝试使用模式 **dune\_user\_generated** 而不是您在 Github 中想要的实际模式运行查询。

如果测试成功，您可以继续进行拉取请求。如果可以，请将“测试表/视图”附加到拉取请求中。

### 视图定义

要了解特定视图是如何创建的，您可以对 pgsql 基础表运行查询。

**某个特定视图**

```sql
select definition from pg_views 
where viewname = 'view_name_here'
```

**所有视图**

```sql
select * from pg_views 
where schemaname = 'dune_user_generated'
```

### 视图依赖

如果您构建多个相互依赖的视图，有时可能会发生您无法更改 `view1` 的情况，因为 `view2` 依赖于 `view1`。这可以通过运行下面的查询来修复任何依赖问题来解决。

```sql
-- 来源: https://stackoverflow.com/a/48770535/1838257

--CREATE OR REPLACE VIEW dune_user_generated.gosuto_view_dependencies AS
SELECT DISTINCT srcobj.oid AS src_oid,
    srcnsp.nspname AS src_schemaname,
    srcobj.relname AS src_objectname,
    tgtobj.oid AS dependent_viewoid,
    tgtnsp.nspname AS dependant_schemaname,
    tgtobj.relname AS dependant_objectname
FROM pg_class srcobj
  JOIN pg_depend srcdep ON srcobj.oid = srcdep.refobjid
  JOIN pg_depend tgtdep ON srcdep.objid = tgtdep.objid
  JOIN pg_class tgtobj ON tgtdep.refobjid = tgtobj.oid AND srcobj.oid <> tgtobj.oid
  LEFT JOIN pg_namespace srcnsp ON srcobj.relnamespace = srcnsp.oid
  LEFT JOIN pg_namespace tgtnsp ON tgtobj.relnamespace = tgtnsp.oid
WHERE tgtdep.deptype = 'i'::"char" AND tgtobj.relkind = 'v'::"char"

-- 像这样过滤:
-- SELECT * FROM dune_user_generated.gosuto_view_dependencies
-- WHERE src_objectname LIKE '%filter_word%'
```

您需要暂时中断依赖关系才能更改 `view1`。

在 [此处](https://dune.xyz/queries/70916) 找到该查询。非常感谢 gosuto 发现了这一点。
