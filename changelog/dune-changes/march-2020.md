# March 2020



本月在数据方面有较大的更新。

#### `block_time`以及`block_number`的的非规范化 <a href="#denormalization-of-block_time-and-block_number" id="denormalization-of-block_time-and-block_number"></a>

我们为所有解码的事件和调用添加了 `block_time`和`block_number`，分别命名为 `evt_block_time`, `evt_block_number` 和`call_block_time`, `call_block_number` 这将消除平台上最痛苦的关联查询，关联`ethereum.transactions`以便在您的查询中获得时间维度。

过去我们可能需要这样做

```
SELECT date_trunc('day', tx.block_time), count(*)
FROM uniswap."Exchange_ev t_TokenPurchase" tp
INNER JOIN ethereum.transactions tx ON tp.evt_tx_hash = tx.hash
GROUP BY 1;
```

为了获得 Uniswap 上每日 `TokenPurchase`-events 的数量，你现在可以很简单地实现

```
SELECT date_trunc('day', evt_block_time), count(*)
FROM uniswap."Exchange_evt_TokenPurchase"
GROUP BY 1;
```

消除了令人痛苦以及消耗较大的join。通过使用`evt_block_number`或`call_block_number`，也可以在不涉及任何以太坊表的情况下完成两个解码表的连接。



####无参数解码事件 <a href="#decoding-events-without-parameters" id="decoding-events-without-parameters"></a>

We’re also “decoding” events that are emitted without parameters. These events have their own tables, but only include Dune-added fields i.e.:

| Column             | Type          |
| ------------------ | ------------- |
| `contract_address` | `bytea`       |
| `evt_tx_hash`      | `bytea`       |
| `evt_index`        | `bigint`      |
| `evt_block_time`   | `timestamptz` |
| `evt_block_number` | `bigint`      |

#### 解码 Call Output <a href="#decoding-call-outputs" id="decoding-call-outputs"></a>


我们已将函数返回值添加到已解码的`call`table中。 每当一个有命名函数有一个返回值时，它将被解码为 `output_{{name}}`，当它没有命名时，它将被解码为`output_{{i}}`，其中 i 是一个从 0 开始的计数器。参考以下案例，计算调用 Compound CERC20 mint 函数的成功和错误代码：

```
SELECT output_0, count(*) 
FROM compound_v2."cErc20_call_mint" 
WHERE call_success 
GROUP BY 1;
```

#### traces.success <a href="#tracessuccess" id="tracessuccess"></a>

**TLDR**: 我们向 `ethereum.traces`添加了一个`success`字段，您可以使用它来确定确切的调用是否成功。 对例如有用 计算余额。 这是一个例子:


```
SELECT sum(amount)                                                         
FROM (                                                                     
    SELECT address, sum(amount) / 1e18 as amount                           
    FROM (                                                                 
        SELECT "from" AS address, -value AS amount                         
         FROM ethereum.traces                                              
         WHERE "from" = '\x390435245F2f95f7443eBb045357DA743E9A65a4'       
         and success = true                                                
    UNION ALL                                                              
        SELECT "to" AS address, value AS amount                            
         FROM ethereum.traces                                              
         WHERE "to" = '\x390435245F2f95f7443eBb045357DA743E9A65a4'         
         AND success = true                                                
    UNION ALL                                                              
        SELECT "from" AS address, -gas_used * gas_price as amount          
        FROM ethereum.transactions                                         
        WHERE "from" = '\x390435245F2f95f7443eBb045357DA743E9A65a4'        
    ) t                                                                    
    group by 1                                                             
) a                                                                        
;
```

**更长的故事**: Dune 从 Parity OpenEthereum 提取交易traces。 OpenEthereum 返回一个的函数的call traces(树状数据结构)，其中一些可能会有非空`error`字段。以前我们或多或少地提取过traces，今天我们在 ethereum.traces 中添加了一个`success`字段。如果在traces层次结构中有非空`error`字段但是它上面没有traces，则它的success字段为true。
 


我们注意到，如果父级tracese-entry有一个非空`error`字段，则子call用在 EVM 中也被视为失败。以前在 Dune 中为了正确评估给定函数调用的状态更改是否包含在区块链中，您需要编写一个稍微复杂的查询来检查跟踪树的同一分支中的任何traces 是否有错误.随着今天success字段的加入，这变得容易多了。



Note that the field `tx_success` field denotes the success of the transaction as a whole, and that a `true` `tx_success`-field, does not necessarily mean that every function call in the transaction has a `true` `success` field. Here are the potential combinations

| tx\_success | sucess  |
| ----------- | ------- |
| `true`      | `true`  |
| `true`      | `false` |
| `false`     | `false` |

正如你所看到的，一个成功的transaction中的一个call也可能是失败的，但在一个失败的的transaction中call都会是失败的.....

另请注意，以前解码表上的字段 `call_success`计算为`traces.tx_success && !traces.error`，现在直接从`traces.success`复制


#### Postgresql 12.2 <a href="#postgresql-122" id="postgresql-122"></a>

将数据库升级到 postgresql 12.2。 更改日志在[这里](https://www.postgresql.org/docs/current/release-12-2.html)。

#### 其他<a href="#misc" id="misc"></a>

Renamed some curvefi-contracts:

| schema  | name         | Address                                    |
| ------- | ------------ | ------------------------------------------ |
| curvefi | compound     | 0xe5fdbab9ad428bbb469dee4cb6608c0a8895cba5 |
| curvefi | usdt         | 0x52ea46506b9cc5ef470c5bf89f17dc28bb35d85c |
| curvefi | y            | 0x45f783cce6b7ff23b2ab2d70e416cdb7d6055f51 |
| curvefi | compound\_v2 | 0x2e60cf74d81ac34eb21eeff58db4d385920ef419 |
| curvefi | busd         | 0x79a8c46dea5ada233abaffd40f3a0a2b1e5a4f27 |
| curvefi | compound\_v3 | 0xa2b47e3d5c44877cca798226b7b8118f9bfb7a56 |

Moved onesplit contracts to their own schema.

| schema   | name     | address                                                |
| -------- | -------- | ------------------------------------------------------ |
| onesplit | OneSplit | 0x2ad672fda8a042c4c78c411bf9d4f1b320aa915a             |
| onesplit | OneSplit | 0xdff2aa5689fcbc7f479d8c84ac857563798436dd             |
| onesplit | OneSplit | 0x0a236b41add0073df05eac5fc8505ad745c\*\*\*\*\*\*7859d |
