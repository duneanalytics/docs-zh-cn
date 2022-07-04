---
说明： 欢迎使用Dune
---

# Dune介绍

## 简介

**Dune是一款强大的区块链研究工具。通过Dune你可以查询，导出和可视化区块链上浩如烟海的数据。Dune让公共的区块链数据对每个人都触手可及，从而使其有了无限的可能。该文档将会帮你解答类似如下的问题：**

[Uniswap每天的交易资金有多少？](https://dune.xyz/queries/3)

[哪家去中心交易所的交易额最高？](https://dune.xyz/queries/1847)

[重要的稳定币有什么链上行为？](https://dune.xyz/hagaetc/stablecoins)

## Dune基础

#### Dune应用

理解[查询](./#queries), [可视化](duneapp/visualizations/)和 [仪表盘](duneapp/dashboards.md)的概念将对你畅游Dune很有帮助。 他们是你通往区块链信息大门的基础区块。作为一名区块链分析师，你可以查询数据信息，可视化你的结果，然后进一步用仪表盘来讲述你的内容。 

#### Dune的数据可视化

在屏幕背后，Dune将晦涩的数据转换成容易阅读的数据表。使用SQL查询，你能准确得筛选出你需要的信息。

Dune既提供原始区块链数据也提供已解码的数据。只有有用户向我们提出了合约解码的需求，我们才提供对应的解码数据。 你可以在这里提交解码需求[网页](https://dune.xyz/contracts/new).\
\
目前你可以查询以下的链上数据 **Ethereum, Polygon, Binance Smart Chain, Optimism** 和 **Gnosis Chain**.

Dune提取区块链上我们索引的时间和内部调用，我们不规定和存储数据。

## 查询

Dune将区块链的数据聚合到易于检索的数据库中。通过查询，你可以限定输出哪些数据。 

你也许想了解 _今天所有去中心化交易所的交易额_, 或者 _今年稳定币的铸造数量_。 不管是哪个问题，你都可以通过查询来解答。

查询同时返回数据的行和列 (与传统的SQL查询类似)，你可以用来可视化展示。

![Screen Shot 2021-04-22 at 9 56 34 AM](https://user-images.githubusercontent.com/76178256/115726979-357d1380-a351-11eb-83ee-16f0d57c6ecb.png)

一名区块链分析师（比如你）可以有很多途径开始一条查询：

1. 使用Dune的 _抽象表_ 来查询常用数据表。这是最简单的也是最常用的使用Dune的方式。一些常用的抽象表包括`dex.trades`，`lending.borrow`和`stablecoin.transfer` (你可以在这里找到完整的抽象表列表[这里](https://github.com/duneanalytics/abstractions))
2. 查询区块、日志和交易在内的原始以太坊数据。
3. 中心化交易所的数据也是可以查询的。通过使用`prices.usd`可以迅速的返回几乎任意一种加密货币的价格。

## 可视化

表中的数据 (行和列)有时不易阅读。结果的可视化可以更清晰准确地展示信息。

你可以开始通过可视化来讲述你的内容。通过Dune你可以轻松地将:

![Screen Shot 2021-04-22 at 10 59 48 AM](https://user-images.githubusercontent.com/76178256/115737269-fa331280-a359-11eb-9a31-c0dfe4b038e6.png)

转化为:

![Screen Shot 2021-04-22 at 11 01 02 AM](https://user-images.githubusercontent.com/76178256/115737692-5b5ae600-a35a-11eb-8145-bdcf9396cd03.png)

这个柱状图清晰地展示了最高的交易量发生在4月19日，同时帮助观众了解了时间趋势。

Dune提供了可是展示数据的一系列的可视化选择，包括柱状图，面积图，线图，饼图等等。

## 仪表盘

使用精心设计的视觉展示，聪明的区块链分析师可以讲述关于特定数据组的内容。例如，在下文中，[仪表盘](https://dune.xyz/hagaetc/dex-metrics) 顶部展示了'Dex'在增长。观众来可以通过下面的展示发现，哪些去中心化交易所的交易额是较高的，最终通过累积的柱状图来能发现随时间变化的趋势。只需要看这一个仪表盘，观众就能清楚地看到整个DEX市场的情况。

![Screen Shot 2021-04-23 at 10 51 25 AM](https://user-images.githubusercontent.com/76178256/115889404-e7841080-a421-11eb-9e30-8d43e58e28f4.png)

## Dune需要协作

Dune默认所有查询和数据库都是公开的。

这引入了一个有趣的动态，在这个动态中，用户可以轻松地分叉和混合其他创建者的查询，并在他们的知识基础上进行构建。另一方面，每次编写新查询时，你都会参与查询贡献，帮助人们查询Dune上的数据。Dune社区通过不断改进的查询范围获得共同成功，你可以轻松查询所需的统计信息。

如果你对查询的隐私性有需求, 你可以订阅[pro Plan](https://dune.xyz/pricing)。

加入我们 [Discord社区](https://discord.gg/BJBHFR6sdy) 来获得我们团队和社区的帮助。

如果有任何反馈意见，不管是功能需求还是报告错误，你都可以在这里提交[这里](https://feedback.dune.xyz/).
