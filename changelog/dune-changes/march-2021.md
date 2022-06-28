---
description: Dune Analytics version 2.0
---

# March 2021

## 更新概览<a href="#overview-of-changes" id="overview-of-changes"></a>

* 查询
  * 保存
  * 执行
* 刷新
* 其他各种更新

新版本的Dune无论是前端上还是底层都对查询和刷新方式做了调整。


## 查询 <a href="#queries" id="queries"></a>

**保存**

默认情况下**新查询不会保存**因此您可以快速尝试以及测试各种想法。如果您想保留您的工作成果，请记住点击 **“保存”按钮**并为您的查询命名。
这意味着，如果您在没有保存的情况下离开页面，您的工作成果就会丢失。如果您在浏览器中允许来自[dune.xyz](http://dune.xyz/) 的通知，当您离开未保存的内容时，该应用程序会通知您。
这同样适用于forks。在您单击 **保存按钮**以及为您的 fork 命名之前，不会将 fork 保存到您的查询中。


**执行**
我们将旧的保存和执行按钮合并为一个按钮，**运行**。当您在已保存的查询上单击**运行**时，您对查询文本所做的任何更改都会被保存，并且查询会排队等待执行。


> _**Useful tip:**_ Press `CMD / CTRL + Enter` to run your query.

**Running query snippets**

If you mark parts of your query, the button changes to **Run selection**, and executes the part of the query that you’ve marked. This will updates the results and visualizations in the process but _**not save**_ the query.

> _**Useful tip:**_ if you want to execute the whole query without saving it, select all of the query (CMD / CTRL + A) and then hit **Run selection**.

## Refreshing <a href="#refreshing" id="refreshing"></a>

There’s also changes to how results are refreshed. Over the last months we’ve been redesigning our entire query execution backend to bring large improvements to how Dune works.

When a visualization is viewed, either on a query page or on a dashboard, the Dune backend will inspect the age of the most recent result. If the result is stale (currently defined as >3 hours old), Dune will automatically queue an execution for this query and run it in the background.

This means that your dashboards will always be kept up to date when they are being viewed.

Query creators does not need to set a refresh scheduele and the scheduling feature has been deprecated as it is no longer needed to keep results fresh.

## Queues <a href="#queues" id="queues"></a>

We’re also bringing changes to how queueing works in Dune. When you manually click **Run** on a query an execution is queued. Every user on Dune can queue 3 queries at the same time (pro users have more, see our [pricing](https://hackmd.io/YOP3YIgaRAejTPE190sOjw?view) page for more info).

Automatically refreshed results (as we talked about above) does not use the query creators queuing credits nor the viewing user’s.

## Other updates <a href="#other-updates" id="other-updates"></a>

**A faster and better looking Dune** - the whole Dune app including all the charts have gotten a big facelift. The app is now also many times faster.

**Query parameters** - what is probably Dune’s most underrated feature has gotten a major upgrade. It’s now super easy to filter any query or dashboard for a token, address or anything really. Meaning you can turn your queries and dashboards into interactive apps for onchain data.

**See all content without having to log in** - simply copy and share the link from the page you are on. The exception is for paid users that make their content private, more details on our [pricing page](https://hackmd.io/YOP3YIgaRAejTPE190sOjw?view).

**Errors show on the relevant line** - when running your queries you will see error messages directly on the relevant line in your code.

**Click query title to go to query from a dashboard**

We’d love to get your feedback on your experience with Dune v2 in the #feedback channel in our [Discord](https://discord.com/invite/ErrzwBz). You can also contact [support@dune.xyz](mailto:support@dune.xyz) if you face any issues.

Happy querying!

**PS.** There are a few visualisation types that are currently not supported in Dune v2: pivot, cohort, funnel, heatmap and bubble. We’ve emailed the users that are affected. If you have not been emailed all your queries and visualisations should work seamlessly in v2.
