# October 2019

#### 新的数据结构 <a href="#new-data-structure" id="new-data-structure"></a>

如果您要分叉旧查询，则需要注意这些变化，您可以用新的替换旧的表和列名。


#### USD价格表<a href="#usd-price-tables" id="usd-price-tables"></a>
以前美元价格表被命名为`coincap."tokens/usd"`现在改为`prices.usd`。

```
symbol -- unchanged
contract_address -- previously "asset"
price -- previously "average"
minute -- previously "time"
```

#### Dune generated columns <a href="#dune-generated-columns" id="dune-generated-columns"></a>

We add some data fields to all decoded tables for `calls` and `events`. These are now named `evt_index, evt_tx_hash, contract_address`. Previously these were simplt named `index, tx_hash, address` but we wanted to make it clear that these are added by Dune and not extracted directly from the blockchain.
