---
title: 软件2:VSC
date: 2018-09-30
tags:
    - Software
categories:
    - Computer
---

# 前言

前面讲的所有软件中，我想重点讲一下 VSCode 这个软件，简直神器，这是微软发布的一款轻量级的编辑器，但拥有非常多扩展功能，而且原生自带 git 支持，非常适合平时写写文章，脚本和轻量级的程序。特别是平时写文章，非常地好用，三大主流系统通用啊！

具体编程累软件安装参见 Mac 篇

# 安装

按照官网下载就行了，然后可以下载各种插件，都在插件商店里面有，下载完重新加载一下就行，而卸载插件也是一键式的。插件也会分文件夹装在 VSC 生成的一个隐藏文件夹中，不会乱跑，Windows 和 Mac 都在用户文件夹下的.vscode\extensions 里面，当然也可以改目录。

# 设置

VSC 的设置也是越来越方便了，当然本质还是一个.json 文件，应该一看就可以看懂，设置分三种：用户设置，工作区设置和文件夹设置。

其中用户设置保存在程序中（setting.json 可以通过打开设置，然后查看文件路径得知，这样就可以备份出来了）；工作区设置保存在.code-workspace 文件中，可以自己指定存的地方；文件夹设置在本文件夹下的.vscode\setting.json 里面。

# git

打开文件夹后，在 VSC 的 git 那一栏下初始化 git 仓库。然后在集成的终端内输入：

```
git remote add origin https://github.com/XXX.git
git pull origin master
```

结下来发布时有可能要输入密码，照着做就行。

不想输入密码就：

```
git config --global credential.helper store
```

# 插件推荐

1. 主题
    - Bracket Pair
    - One Dark Pro
    - Code runner
2. Markdown
    - Markdown all in one
    - Markdown-preview-enhanced
    - vscode-pandoc
3. 语言（按需求装）
    - Latex Workshop
4. 其他
    - Quick task

Markdown 部分参见我关于 Markdown 的三篇博文。

基本上都是马上可以用的，windows 下 latex 要写一些配置（主要是路径）才能用，但也不麻烦。如果想编译 c++，则要一番配置，不如用现成的 ide，Clion 就很不错。

# 后记

这样就可以愉快地使用 VSC 了，非常简洁和轻量。
