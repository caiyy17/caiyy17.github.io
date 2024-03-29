---
title: Markdown1:简易的文本格式
date: 2018-06-13
tags:
    - Markdown
categories:
    - Computer
---

# 前言

Markdown 是一种轻量级的标记语言，它可以被转换为 HTML 格式的内容。

# Markdown 概述

Markdown 的宗旨便是**易写易读**，即便一篇 Markdown 文档直接以纯文本形式发布，其内容同样易于阅读。同时，Markdown 提供了便捷的排版功能，尤其在转换为 HTML 格式后，在网页上使用非常方便。

在这里吐槽一下：

微信公众号的各种排版工具和网站实在是过于复杂，虽然它们也许可以实现很多非常花哨的功能，但是一点点调整一点点排版十分的反人类，又不是要做一张海报，我觉得一个非常简洁的工具才是比较好的。

至于 LaTeX，我个人认为其编辑效率并不理想，在编辑公式方面，Word 自带的公式编辑器（并非 MathType）的速度和便捷性实际上相当出色。另外，LaTeX 对浮动体的处理并不是特别友好，相比之下，Word 的嵌入功能会好很多（虽然它有时候会更占地方，但对于强迫症来说，我总希望所有物体出现在它该出现的地方...）。只有在交叉引用方面，LaTeX 会比 Word 方便很多。

吐槽完毕...

# Markdown 语法

好了，我们正式开始。

## 换行

和 Latex 差不多 Markdown 使用一个空行来换行

例：

    这是一个段落

    这是一个段落

效果：

> 这是一个段落
>
> 这是一个段落

当然，Markdown 也可以使用在行末尾加上两个或以上空格加来换行。（其实是在末尾加了&lt;br/>）

    这是一个段落（末尾有两个空格）
    这是一个段落

效果：

> 这是一个段落  
> 这是一个段落

会发现这样的的强制换行间距与空行换行不一样

## 标题

Markdown 支持 6 级标题，分别用不一样数量的\#来标记注意一定要在\#后面**加空格**

例：

    # 这是H1
    ## 这是H2
    ###### 这是H6

效果：

> # 这是 H1
>
> ## 这是 H2
>
> ###### 这是 H6

## 引用

引用和 email 中的很像，在引用区块的每行前面加一个或多个\>（几个就是几级）

例：

    >一级
    >>二级
    >>>三级

效果：（这里没有像前面一样另加一个引用层级）

> 一级
>
> > 二级
> >
> > > 三级

## 列表

使用\*，\+，\-作无序列表（注意加空格）

例：

    * 第一条
    * 第二条
    * 第三条

效果：

> -   第一条
> -   第二条
> -   第三条

使用数字加\.可以产生有序列表

例：

    1. 第一条
    2. 第二条
    3. 第三条

效果：

> 1.  第一条
> 2.  第二条
> 3.  第三条

**多级列表**

每一级和上一级缩进四个空格，同时在下一级中所有的代码都相应缩进 4 个空格，这个时候代码块用\`\`\`比较好。

例：

    * 第一级
        * 第二级
        * 第二级
    * 第一级
    * 第一级

效果：

> -   第一级
>     -   第二级
>     -   第二级
> -   第一级
> -   第一级

## 代码块

代码只要四个空格就可以成代码块了，或用\`\`\`括起来，若是行内代码，用\`括起来。

例：

````
    这是一段代码

\```
这是一段代码
\```

这是一段`行内代码`
````

效果：(注意代码块加引用的时候要 5 个空格，\>之后要有一个空格和四个空格分开)

>     这是一段代码
>
> ```
> 这是一段代码
> ```
>
> 这是一段`行内代码`

## 分割线

使用三个以上\*或\-或\_独立成行

例：

    文字
    ******
    文字

效果：

> 文字
>
> ---
>
> 文字

## 表格

表格形式：

    |表头|表头|表头|
    |---|:--:|---:|
    |内容|内容|内容|
    |内容|内容|内容|

其中：

-   第二行分割表头和内容。
-   \- 大于等于一个就行
-   左边加":"表示文字居左（不加默认居左），两边加":"表示文字居中，右边加":"表示文字居右
-   最两边的"|"可以省略

效果：

| 表头 | 表头 | 表头 |
| ---- | :--: | ---: |
| 内容 | 内容 | 内容 |
| 内容 | 内容 | 内容 |

## 区段元素

### 链接

链接形式:(标题可选)

    [接受链接的文字](http://example.net/ "标题")

或（注意后面[id]一定要定义好，不然没用）

    [接受链接的文字][id]

    [id]: http://example.net/ "标题"

或

    [接受链接的文字][]

    [接受链接的文字]: http://example.net/ "标题"

或

    <http://example.net/>

效果：

> [接受链接的文字][]
>
> [接受链接的文字]: http://example.net/ "标题"

或(最后一种)

> <http://example.net/>

### 强调

例：

    *文字*

    **文字**

效果：

> _文字_
>
> **文字**

### 图片

图片形式基本同链接，就是有前置感叹号!（这里用参考形式示范）

    ![图片文字][imgid]

    [imgid]: http://example.net/picture "标题"

注：参考形式的\[id\]可以放在任何地方，可以向参考文献一样统一放在文档末尾。

### 转义字符

转义在会引起歧义的符号前加\即可

# 进阶语法

这一部分有可能需要 pandoc 等高级工具支持。

## 数学公式

直接用 latex 的就好

## 脚注

使用

    Content [^1]

    [^1]: This is a footnote

效果：

Content [^1]

[^1]: This is a footnote

## 交叉引用

交叉引用网上并没有找到十分方便的方法，这是 LaTeX 的语法

    \label{img}

    See \ref{img}

## 参考文献

首先，有一个 ref.bib 的文件

    @book{book1,
    author = {author1, author2},
    title = {title1},
    publisher = {publisher1},
    year = 2018
    }
    @inproceedings{article2,
    author = {author3, author4},
    title = {title2},
    booktitle = {booktitle2},
    address = {here},
    year = 2019
    }

然后在头部写入：

    ---
    csl: FILE
    title: "Sample Document"
    bibliography: ref.bib
    ---

就可以正常引用了，引用格式

    [@book1] //bib里每个字条的第一个字段

# 参考

[markdown](https://daringfireball.net/projects/markdown/)
