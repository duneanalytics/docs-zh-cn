---
说明: Dune的仪表盘（dashboard）让你轻松汇总
---

# 仪表盘

### 仪表盘是存放汇总信息并供人参考的地方

Dune的仪表盘由小组件组成。小组件可以是可视化结果，也可以是一个文本框，也可以在文本框内嵌入图片或者gif。

你可以自由地调整每个部件的大小，以符合你想要创建的布局。

### 创建一个仪表盘

你可以通过浏览我们的 "发现 "页面并点击右侧的 "新仪表盘 "按钮来创建一个新的仪表盘。

你给仪表盘最初起的名字也将是它URL的一部分，之后就不可以改变URL的这部分后缀名了，所以一开始要注意。不过改变仪表盘的名字，总是会正确显示的。

![creating a dashboard](<../.gitbook/assets/2021-11-03 16-59-42 (1) (1) (2).gif>)

### 添加可视化

你可以通过进入编辑模式并点击相应的按钮，简单地将可视化内容添加到你的仪表盘中。要进入编辑模式，首先打开你自己的一个仪表盘，点击右上方的编辑按钮。

![](<../.gitbook/assets/2021-11-09 13-17-41.gif>)

### 添加文本框

要在仪表盘上添加文本框，你必须先进入编辑器模式，然后可以点击 "添加文本小部件"。这将打开一个简单的文本编辑器窗口。

![](<../.gitbook/assets/2021-11-09 13-20-39.gif>)

文本框支持markdown的一个子集，你可以编辑文本并嵌入图像和gif。

#### 文本编辑

这是一个关于markdown语法的简短清单。更高级的markdown指南可以在[这里](dashboards.md#dashboards-are-where-dunes-content-lives-and-gets-discovered.)找到。

| 元素                                                                         | Markdown语法                                                                                    |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| [标题](https://www.markdownguide.org/basic-syntax/#headings)                 | <p><code># H1</code><br><code>## H2</code><br><code>### H3</code></p>                              |
| [加粗](https://www.markdownguide.org/basic-syntax/#bold)                        | `**bold text**`                                                                                    |
| [斜体](https://www.markdownguide.org/basic-syntax/#italic)                    | `*italicized text`                                                                                 |
| [有序列表](https://www.markdownguide.org/basic-syntax/#ordered-lists)       | <p><code>1. First item</code><br><code>2. Second item</code><br><code>3. Third item</code><br></p> |
| [无序列表](https://www.markdownguide.org/basic-syntax/#unordered-lists)   | <p><code>- First item</code><br><code>- Second item</code><br><code>- Third item</code><br></p>    |
| [代码](https://www.markdownguide.org/basic-syntax/#code)                        | `` `code` ``                                                                                       |
| [水平规则](https://www.markdownguide.org/basic-syntax/#horizontal-rules) | `---`                                                                                              |
| [链接](https://www.markdownguide.org/basic-syntax/#links)                       | `[title](https://www.example.com)`                                                                 |

####

### 插入图片和GIFs

我们的文本框也可以用来嵌入图片或gif文件到你的仪表盘。

嵌入图片的语法是：

| [Image](https://www.markdownguide.org/basic-syntax/#images-1) | `![alt text](image url)` |
| ------------------------------------------------------------- | ------------------------ |

由于你不能在我们的服务器上本地存储图像，你需要在其他地方上传你的图像，或者在互联网的某个地方找到原始文件。

在实际中，这可能看起来像这样：

```
![text](https://pbs.twimg.com/media/FEWVLQwWUAQcqLY?format=jpg&name=medium)
--this is an image stored on twitters servers
```

你可以通过简单地调整它所包含的小组件的大小来调整图像的大小。

你可以在一个小组件中结合图像和文本：

![插入一个图像并改变大小](<../.gitbook/assets/2021-11-18 16-23-05.gif>)

### 仪表盘布局

你可以以任何你喜欢的方式安排仪表盘上的不同部件。

小部件总是试图向上移动，所以如果你想在你的仪表盘上创建一个视觉分隔部分，建议创建一个大文本框作为分隔。



![](<../.gitbook/assets/2021-11-19 01-22-49.gif>)
