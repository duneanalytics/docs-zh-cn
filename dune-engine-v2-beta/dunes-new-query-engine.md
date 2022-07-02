# Dune 新版数据库

### **引言**

**Dune Engine V2** 是 Dune 的新版查询引擎，它将 Dune 的核心工具集的性能、可扩展性和功能带到了一个新的水平，使 wizards 能够查询、提取和可视化区块链上的大量数据。

它利用 **Apache Spark** 来提高复杂查询的性能、处理数据规模扩张，并能够在同一个 UI 中实现跨链查询。

本节中包含的所有数据源现在都可用于使用新版查询引擎进行查询。目前我们有以下数据可供查询：

* ****[**原始数据表**](../../data-tables/data-tables/raw-data/)****
  * Ethereum
  * Gnosis Chain
  * BNB Chain
  * Optimism
  * Solana
* ****[**已解码数据表**](../../data-tables/data-tables/decoded-data.md)****
  * 我们仍在优化这个部分，目前只有少数已被解码。
* ****[**Flashbots**](../../data-tables/data-tables/community-data/flashbots.md)****
  * 示例: [https://dune.com/niftytable/MEV](https://dune.com/niftytable/MEV)
* ****[**价格表**](../../data-tables/data-tables/prices.md)****（包括 Solana）

### 新版查询引擎

DuneV2 改变了我们的整个数据库架构。我们正在从 PostgresQL 数据库过渡到托管在 Databricks 上的 Apache Spark 实例。两种系统的区别可以总结如下：

* 我们现在使用 Databricks SQL，而不是 PostgresQL。SQL 关键字的变化很小，但可能与您的某些查询书写习惯有关。&#x20;
* 与 PostgresQL 的面向行的方法相反，Spark 是一个面向列的数据库。
* 传统的索引被列块级别的`最小/最大`值替换。

**您可以在此处阅读有关SQL变化的更多信息：**

{% content-ref url="query-engine.md" %}
[query-engine.md](query-engine.md)
{% endcontent-ref %}

### Abstractions（数据抽象）

DuneV2 中的抽象将在 [dbt](https://docs.getdbt.com/docs/introduction)（数据构建工具）上运行。dbt 使分析工程师能够通过简单地编写选择语句来转换其数据仓库中的数据。dbt 将这些选择语句处理转换为 [数据表](https://docs.getdbt.com/terms/table) 和 [视图](https://docs.getdbt.com/terms/view)。

这将使 abstractions 更加健全、可扩展且更易于使用。

{% content-ref url="abstractions-in-dunev2.md" %}
[abstractions-in-dunev2.md](abstractions-in-dunev2.md)
{% endcontent-ref %}

### 反馈

最后一点，由于查询引擎仍处于测试 **(beta)** 状态，您可能会遇到错误或对如何改进它有反馈，请随时在 [Discord](https://discord.com/invite/ErrzwBz) 和 [Canny](https://dune.canny.io/) 上与我们分享。
