# August 2020



#### 价格数据源切换 <a href="#price-provider-switch" id="price-provider-switch"></a>
我们已将价格数据源从 Coincap 更改为 Coinpaprika，以及现在在 prices.usd 和 prices.layer1_usd 表中拥有 230 多种资产的价格！ 有一点需要注意的是，以前我们有截至当前时间的所有分钟的价格，但我们现在有截至当前时间前 5 分钟的价格。

**新表prices.layer1_usd**

我们已将所有非以太坊代币的资产移至它们的表 prices.layer1_usd。 该表按symbol分区并具有（symbol,minute）


```
 Column │           Type           
────────┼──────────────────────────
 minute │ timestamp with time zone 
 price  │ double precision         
 symbol │ text                     
```

#### Changes to prices schema <a href="#changes-to-prices-schema" id="changes-to-prices-schema"></a>

**New column decimals on prices.usd**

We’ve added a new column `decimals` to `prices.usd` so that you can avoid the additional join with `erc20.tokens` for calculating correct volumes.

**New primary key for prices.usd**

Previously `prices.usd` was partitioned on the token `symbol`, and it’s primary key was `(symbol, minute)`. In this release we’ve made a change to this scheme to allow for multiple `contract_address`es to map to the same `symbol`. This could happen e.g. in times where a token migration is happening. `prices.usd` is now partitioned on `contract_address` and it’s primary key is `(contract_address, minute)`.

Note that this might yield some weird results for queries that rely on doing `SELECT minute, price FROM prices.usd WHERE symbol='x'` in certain cases where there are two contracts mapped to the symbol `x` in a time window. You may then receive several rows per minute. It is better to then use the primary key `contract_address` for filtering and joining.

```
      Column      │           Type          
──────────────────┼─────────────────────────
 minute           │ timestamp with time zone
 price            │ double precision        
 decimals         │ smallint                
 contract_address │ bytea                   
 symbol           │ text                    
```
