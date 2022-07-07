# Dune V2 Intro

### **Introduction**

**Dune Engine V2** is Dune's new query engine that brings a new level of performance, scalability and functionality to Dune's core set of tools that enables wizards to query, extract, and visualize the vast amounts of data on the blockchain.

It leverages **Apache Spark** to enable increased performance of complex queries, handle data scale, and enable cross-chain queries all within the same UI.

All of the data sources contained in this section are available for querying with the new query engine today. Currently we have the following data available to query:

* ****[**Raw tables**](../data-tables/data-tables/raw-data/)****
  * Ethereum
  * Gnosis Chain
  * BNB Chain
  * Optimism
  * Solana
* [**Decoded tables**](../data-tables/data-tables/decoded-data.md)****
  * we are still optimizing this, only a few are decoded for now
* ****[**Flashbots**](../data-tables/data-tables/community-data/flashbots.md)****
  * Example: [https://dune.com/niftytable/MEV](https://dune.com/niftytable/MEV)
* ****[**Prices**](../data-tables/data-tables/prices.md) (including Solana)

### New query engine

DuneV2 changes our entire database architecture. We are transitioning away from a PostgresQL database to an Instance of Apache Spark hosted on Databricks. The difference between the two systems can be summarized as follows:

* Instead of PostgresQL, we will now use DatabricksSQL. The change in SQL keywords is minimal but might be relevant for some of your querying habits.&#x20;
* Spark is a column oriented database in contrast to PostgresQL’s row oriented approach.
* traditional indexes are replaced by column chunk level `min/max` values

**You can read more about the changes here:**

{% content-ref url="query-engine.md" %}
[query-engine.md](query-engine.md)
{% endcontent-ref %}

### Abstractions

Abstraction in DuneV2 will run on [dbt](https://docs.getdbt.com/docs/introduction) (data build tool). dbt enables analytics engineers to transform data in their warehouses by simply writing select statements. dbt handles turning these select statements into [tables](https://docs.getdbt.com/terms/table) and [views](https://docs.getdbt.com/terms/view).

This will make abstractions more robust, scaleable and easier to work with.

{% content-ref url="abstractions-in-dunev2.md" %}
[abstractions-in-dunev2.md](abstractions-in-dunev2.md)
{% endcontent-ref %}

### Feedback

One final note, as the query engine is still in in **beta** you may run into bugs or have feedback on how it can be improved, feel free to share it with us on [Discord](https://discord.com/invite/ErrzwBz) and [Canny](https://dune.canny.io/).
