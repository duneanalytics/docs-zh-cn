# 结果如何刷新?

当一个可视化结果被查看时，无论是在查询页面还是在仪表盘上，Dune后端将检查最近的结果运行时间。如果结果是比较老的（目前定义为>3小时），Dune将自动执行这个查询并在后台运行。

这意味着仪表盘在被查看时将始终保持最新状态，查询创建者不需要设置刷新时间。

请注意，结果刷新的执行队列与每个用户在查询编辑器中创建和运行查询的队列是分开的。
