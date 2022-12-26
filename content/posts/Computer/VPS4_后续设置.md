---
title: VPS3:githubio博客和VPN
date: 2022-08-26
tags:
    - VPS
    - Blog
categories:
    - Computer
---

# 前言

之前使用 VPS 搭建个人博客和代理，但是这样需要时刻维护服务器，其实还是占用了不少时间和精力，为了最方便地使用，目前使用 hugo 博客搭配 githubio 使用，而代理则使用成品代理，有专人替你维护。

# HUGO 安装

HUGO 是基于 go 的博客框架，编译起来比之前基于 node 的 js 框架要快非常多。

[Install Hugo](https://gohugo.io/getting-started/installing/)

```zsh
brew install hugo
```

然后选一个主题即可，我这边选择的是 PaperMod，使用 git submodule 安装：

[Install PaperMod](https://github.com/adityatelange/hugo-PaperMod/wiki/Installation)

# 部署到 Github

设置参照 theme 给的例子即可，需要注意的是部署到 github 需要将

```toml
publishDir = "docs"
```

加在配置中，然后在 githubio 设置中将发布路径改为 docs，这样不用设置另外分支，非常方便。

# 生成网页

博客直接在 posts 中用 md 编写即可，需要发布时先用 hugo 命令生成，然后同步 github 的 repo

```zsh
hugo -D
```
