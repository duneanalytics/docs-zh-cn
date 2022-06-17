---
description: 数据表是 Dune 工作的基础
---

# 数据表



{% embed url="https://www.youtube.com/watch?v=UDu23Eyvo_Y" %}

这个视频涵盖了有关数据表的所有重要主题。

## Dune 的表结构

Dune 将区块链数据聚合到一个可访问的 PostgreSQL 数据库中。可以通过以下方式理解模式：

1\) 底层数据（原始交易数据）提供区块链上所有活动的详细记录。

2\) 解码数据表使用相应智能合约的应用程序二进制接口将编码数据转换为解码数据。您可以通过我们的 [网站] (https://dune.com/contracts/new) 的这一部分向我们发出信号，表明您需要某个智能合约的解码数据。在”[添加新合约](../../duneapp/adding-new-contracts.md)”中阅读有关合约提交的更多信息。

3\) Abstractions（抽象）是返回有关分类/主题的聚合/标准化数据的更高级别的生成表。生成这些表的脚本可在这个 [github 存储库](https://github.com/duneanalytics/abstractions) 中找到。

您目前可以从**Ethereum、Polygon**、**Binance Smart Chain**、**Optimism** 和 **Gnosis Chain** 区块链数据集查询数据。

## 如何找到我需要的数据？

本部分旨在指导您找到适合您项目的数据表。

Dune 的不同用例需要不同的数据表来提取数据，仔细研究这些数据表，您将认识到每种数据表类型对查询和仪表板的重要性。

Dune 上的大多数表都是由 Dune 填充的，只是将区块链数据转换为 SQL 表，但在 [abstractions](abstractions.md) 和 dune\_user\_generated 表的情况下，您实际上可以创建自己的表，根据您的需要聚合或修改数据集。

{% content-ref url="raw-data/" %}
[原始数据](raw-data/)
{% endcontent-ref %}

{% content-ref url="decoded-data.md" %}
[已解码数据](decoded-data.md)
{% endcontent-ref %}

{% content-ref url="abstractions.md" %}
[抽象数据（Abstractions）](abstractions.md)
{% endcontent-ref %}

{% content-ref url="prices.md" %}
[价格数据](prices.md)
{% endcontent-ref %}

{% content-ref url="labels.md" %}
[标签数据](labels.md)
{% endcontent-ref %}
