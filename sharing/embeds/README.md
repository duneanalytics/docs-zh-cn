---
说明: 嵌入功能使你能跨网站地使用实时更新的Dune图表！
---

# 嵌入功能

## 使用嵌入功能

**截图是无趣和过时的。**

Dune自带可以跨平台使用的嵌入功能，而不是使用形式不一的截图功能。&#x20;

你可以在查询标题生成嵌入链接或者选择右上角的嵌入功能。

![generating an embedd link](<../../.gitbook/assets/2021-11-01 14-30-33.gif>)



### 参数化嵌入

参数化的查询也是可以实现的，但这需要一点技巧：

生成的嵌入链接并不包含必要的参数，尽管查询已经被执行过。我们正在致力于实现自动化的生成链接，但是目前仅有这一条途径能够做到。

现在你需要在链接前手动输入参数。

输入参数的写法是这样的：

`link?name_of_parameter1=xxxx&?name_of_parameter2=yyyy&...` __&#x20;

一个示例：

`>https://dune.xyz/embeds/118220/238460/aa002dd3-f9e2-4d63-86c8-b765569306c6NFT?address=0xff9c1b15b16263c61d017ee9f65c50e4ae0113d7&rolling_n_trades=500`\
``

### 在不同平台上嵌入

这里有几个使用Dune嵌入的示例：

{% content-ref url="discord.md" %}
[discord.md](discord.md)
{% endcontent-ref %}

{% content-ref url="twitter.md" %}
[twitter.md](twitter.md)
{% endcontent-ref %}

{% content-ref url="mirror.xyz.md" %}
[mirror.xyz.md](mirror.xyz.md)
{% endcontent-ref %}

{% content-ref url="webpages.md" %}
[webpages.md](webpages.md)
{% endcontent-ref %}



### 已知问题

然而嵌入功能在一些比较流行的网站上还不能实现，这包括：

* substack
* medium
* gitbook
