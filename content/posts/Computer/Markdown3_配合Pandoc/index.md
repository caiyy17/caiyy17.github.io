---
title: Markdown3:配合Pandoc
date: 2018-10-17
tags:
    - Markdown
categories:
    - Computer
---

# 前言

从 Md1 到 Md2，基本的 Markdown 写作已经没有问题了，然后就会想用 md 解决其他的事情了，比如写学术论文，Latex 写起来太费劲了，公式还是 word 比较方便。

Pandoc 是一个开源的万能文档转换软件，可以在 pdf，md，word，html 之间转换文档格式。而装了 vscode-pandoc 后可以直接在 vscode 中使用，但是每次只能用同样的命令行参数，否则只能在终端中使用。

# 安装

直接 brew 安装即可，还要安装 pandoc-citeproc，vscode 插件也是一键安装。

然后是配置，word 不需要配置，html 需要配置命令行参数，在设置的 string 里面填入就行。

## html 参数

```
--standalone --self-contained --css pandoc.css
```

以上转换命令中选项含义介绍如下。

1. “–standalone”：为目标格式文件增加合适的“header”和“footer”。如果目标格式是“HTML”，那么通过该选项可解决“由于未设置合适的字符编码，而导致的乱码问题”。
2. “–self-contained” ：如果目标格式是“HTML”，那么可得到如下效果：在转换得到目标 HTML 文件的过程中，会将依赖的“脚本，CSS，图片，视频”等文件的内容内嵌到目标 HTML 文件中。
3. “–css pandoc.css” ：使得目标格式文件最后使用“pandoc.css”文件作为 CSS 文件，否则，使用默认的 CSS 文件。

## PDF 参数

pdf 需要 latex 的支持，mac 可以装 Mactex，Win 有 texlive（不过要配置环境变量）

然而当你尝试的时候就会有各种 bug，网上也有解决方案，但都不是很漂亮，所以我就放弃了这个选项......

# 所以

最终我选择了把 md 转为 word 然后可以使用 word 的公式编辑器编写公式......

现在分析需求：

-   数学公式：word 自带或者在 md 里面写 latex
-   引用：使用 pandoc 参数
-   脚注：使用 pandoc 参数
-   参考文献：编辑.bib 文件加上 pandoc 参数
-   实时预览：markdown-preview-enhanced

综上，参数一共是：

```
--filter=pandoc-citeproc //脚注
--bibliography=ref.bib //参考文献
--csl=FILE //参考文献引用标准
```

# 番外：幻灯片

下载 reveal.js 至目标文件夹，然后就可以使用 pandoc 命令来生成幻灯片了

## 幻灯片结构

-   slide level: 默认情况下，slide level 由文章的组织结构中，以紧接着文章内容（而不是另一个标题）的最高的 header 等级决定。
-   一条水平线总会开启一页新幻灯片。header 等级等于 slide level 总会开启一页新的幻灯片。组织结构中在 slide level 以下的 headers 会在同一幻灯片页中创建 headers。所以，也可以用`##`空标题来分页，好处是可以接着用{}来控制格式，坏处是文本部工整。
-   组织结构中在 slide level 以上的 headers 会创建“title slides”，它只包含了该 section 的标题，从而将整个幻灯片的内容划分为不同的 sections。
-   插入停顿：你可以在幻灯片页中添加停顿，该功能通过在幻灯片页中插入包含 3 个点的段落实现，且该三个点间以空格隔开。

    ```
    content before the pause

    . . .

    content after the pause
    ```

-   定义幻灯片样式：-V theme=moon -V transition=convex

-   将 sections 包裹进标签(在 HTML5 中包裹进标签)，并且将 header 的 identifiers 附属到标签中。

    ```
    # header {#identifier .class .class key=value key=value}
    ```

    其中每个部分都可以省略

## 常用参数

-   -s, –standalone：转换输出文档时会自动加上合适的 header 和 footer(例如 standalone HTML, LaTeX, RTF)。该选择在转换输出 pdf，epub，epub3，fb2，docx，odt 格式文件时会被自动设置，因此如果转换输出上述格式文件，则不用显示指定该选项。如上所述，使用该选项后，能够将所有依赖的文件(linked scripts,stylesheets, images, and videos)都转换输出到同一个文件中。
-   –slide-level=NUMBER：指定能够创建新幻灯片页的 headers 等级(对 beamer, s5, slidy, slideous, dzslides 而言)。在设定的 level 以上的 header 将幻灯片划分为不同的 sections，在 level 以下的 headers 在同一页幻灯片中创建子标题.默认情况下根据文档内容自动确定，见幻灯片的结构化.
-   -i, –incremental：使幻灯片中的列表选项增量式显示(one by one)。默认情况下，列表内容会一次性显示出来。
-   -c URL, –css=URL：链接到 CSS 样式表。该选项能够使用多次来引入多个文件，所指定的文件能够以指定的顺序依次引入。
-   –data-dir=DIRECTORY：指定用户数据目录，设定之后会在该目录下搜索 pandoc 数据文件。如果没有指定该选项，则会使用默认的用户数据目录:$HOME/.pandoc，可通过 pandoc --version 命令查看。
-   –base-header-level=NUMBER：指定 headers 的基准 level，默认是 1。
-   Math rendering in HTML：可参考官网。

PS：更多可以看[官网](http://pandoc.org/index.html)

## 我的常用命令

```
pandoc -s --self-contained -i -t revealjs test.md -o test.html
```

# 参考

[pandoc 将 markdown 转换输出 HTML slide](http://liumh.com/2014/07/05/pandoc-produce-slide-shows/#styling-slides)
