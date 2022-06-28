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
我们将旧的保存和执行按钮合并为一个按钮，**Run**。当您在已保存的查询上单击**Run**时，您对查询文本所做的任何更改都会被保存，并且查询会排队等待执行。


> _**有用的tip:**_ 按住 `CMD / CTRL + Enter`可以快速运行您的查询.

**运行查询部分片段**
如果您标记查询的一部分，按钮将变为**Run selection**，并执行您标记的查询部分。这将更新项目中中的结果和可视化，但**不保存**查询。



> _**有用的tip:**_ 如果你想执行整个查询而**不保存**它，选择所有查询（CMD / CTRL + A）然后点击**Run selection**

## 刷新 <a href="#refreshing" id="refreshing"></a>
结果的刷新方式也发生了变化。 在过去的几个月里，我们一直在重新设计整个查询执行后端，以极大地改进 Dune 。

当在查询页面或仪表板上查看可视化时，Dune 后端将检查最新结果的年龄。 如果结果过时（当前定义为 >3 小时前），Dune 将自动排队执行此查询并在后台运行它。

这意味着您的仪表板在查看时将始终保持最新状态。

查询创建者不需要设置刷新计划，并且计划功能已被弃用，因为它不再需要一直去刷新结果。

## 队列 <a href="#queues" id="queues"></a>

我们还对Dune中的排队方式进行了调整。 当您在查询里手动单击**Run**按钮时，一个查询会进入队列。Dune 上的每个用户都可以同时排队3个查询（专业用户有更多，请参阅我们的[定价](https://hackmd.io/YOP3YIgaRAejTPE190sOjw?view) 页面了解更多信息）

## 其他更新 <a href="#other-updates" id="other-updates"></a>

**一个更快、更好看的Dune** -包括所有图表在内的整个Dune应用程序都进行了重大改造。这个应用比之前快了很多倍。

**查询参数** -这个可能是 Dune 最被低估的功能已经得到了重大升级。现在通过Token、地址或任何东西筛选任何查询或仪表板都非常容易。这意味着您可以将查询和仪表板变成用于链上数据的交互式应用程序。


**无需登录即可查看所有内容** - 只需复制并分享您所在页面的链接即可。将内容设为私有的付费用户除外，详情请参阅我们的[定价](https://hackmd.io/YOP3YIgaRAejTPE190sOjw?view) 页面。

**错误显示在相关行** -运行查询时，您将直接在代码的相关行上看到错误消息。

**单击查询标题以从仪表板进行查询**

我们很乐意在[Discord](https://discord.com/invite/ErrzwBz)的#feedback 频道中获得您对 Dune v2 体验的反馈。如果您遇到任何问题，也可以联系 [support@dune.xyz](mailto:support@dune.xyz)。


Happy querying!

**PS.** There are a few visualisation types that are currently not supported in Dune v2: pivot, cohort, funnel, heatmap and bubble. We’ve emailed the users that are affected. If you have not been emailed all your queries and visualisations should work seamlessly in v2.
