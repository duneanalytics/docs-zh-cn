# August 2020



#### 价格数据源切换 <a href="#price-provider-switch" id="price-provider-switch"></a>
我们已将价格数据源从 Coincap 更改为 Coinpaprika，以及现在在 prices.usd 和 prices.layer1_usd 表中拥有 230 多种资产的价格！ 有一点需要注意的是，以前我们有截至当前时间的所有分钟的价格，但我们现在有截至当前时间前 5 分钟的价格。

**新表prices.layer1_usd**

我们已将所有非以太坊代币的资产移至它们的表 prices.layer1_usd。 该表按symbol分区并具有 `(symbol, minute)`


```
 Column │           Type           
────────┼──────────────────────────
 minute │ timestamp with time zone 
 price  │ double precision         
 symbol │ text                     
```

#### prices schema的变化 <a href="#changes-to-prices-schema" id="changes-to-prices-schema"></a>

**price.usd表新增新列decimals**

我们在`prices.usd`中添加了一个新的列`decimals`，这样您就可以避免与 erc20.tokens 进行额外关联来计算正确的交易量。



**New price.usd 的新主键**

以前`prices.usd` 是根据代币`symbol`进行分区的 , 它的主键是 `(symbol, minute)`。在此版本中，我们对此方案进行了更改，以允许多个`contract_address`映射到同一个`symbol`。在某些场景下它会发挥作用例如在Token合约发生迁移的时候。`prices.usd`现在在`contract_address`上进行了分区，它的主键是`(contract_address, minute)`。

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
