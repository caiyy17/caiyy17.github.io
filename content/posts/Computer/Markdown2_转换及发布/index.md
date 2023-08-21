---
title: Markdown2:转换及发布
date: 2018-07-12
tags:
    - Markdown
categories:
    - Computer
---

# 前言

为了可以用自己喜欢的排版风格排版 Markdown，需要将其转换成其他有丰富排版功能的格式，最重要的无非是 html 和 tex，本文主要讲 html 的转换。

转至 html 后便可以配合已经编写好的\.css 文件，统一排版成自己想要的格式。便可以发布在自己的网站上了。然而自己便携插件还是有些困难，好在有 hexo 或者 markdownhere 等软件和插件的帮助。前者可以帮助我们制作博客，后者可以直接在 chrome 中将 markdown 原文在公众号网站中转换为带格式的文本。

之后就不需要麻烦的公众号编辑器，只需要每次写 Markdown 就行了。

# Markdown 对 html 的支持

这里说一下 Markdown 原生对 html 的支持。Markdown 的语法默认有对应的 html 语法，对应如下：(html 闭合部分省略，详细结构可以查 html 相应格式)

|      Markdown      |      html       |
| :----------------: | :-------------: |
| 段落（两个换行符） |     &lt;p>      |
|      x 级标题      |     &lt;nx>     |
|      无序列表      |     &lt;ul>     |
|      有序列表      |     &lt;ol>     |
|       代码块       |    &lt;code>    |
|        引用        | &lt;blockquote> |
|       分割线       |    &lt;hr/>     |
|        表格        |   &lt;table>    |
|        链接        |     &lt;a>      |
|        图片        |    &lt;img>     |
|       \*强调       |     &lt;em>     |
|      \*\*强调      |   &lt;strong>   |

而在 Markdown 中也可以自由插入 html 的代码，转换器会在转换的时候保留 html 的代码保持不动（有可能需要设置一下）。例如&lt;br/>

# Markdown 转 html

在 VSCode 里有"Copy Markdown as HTML"插件，可以将 Markdown 转成 html 到剪贴板，但这是人工方式。

而若使用类似 Hexo 的博客软件，可以直接使用 markdown 写文章，之后转换 html 和配合 css 都是自动完成的。

另一方案也可以使用 python 制作脚本，但既然是做公众号，我们可以使用 chrome 插件 markdown here，可以直接在网页输入中将 markdown 替换为带格式的文本。

## Markdown here

markdownhere 中可以设置替换是用的 css 文件，在网上可以找到一些现成的 css，当然也可以自己编一些花哨的 css 格式，这是另一回事了。另外如果自己定义了一些 html 标签，也可以一起写进 css，这些标签也会在替换时一起被渲染。

ps：markdownhere 的作用不止于微信公众号的文本替换，编辑邮件时也可以使用它。

# After all

总之，我个人认为在日常写作中，markdown 是非常有用的生产力工具，只要使用一些极简的文本编辑器甚至是记事本，就可以随时随地地写一些东西，并且在发表时也可以方便地为其渲染格式。
