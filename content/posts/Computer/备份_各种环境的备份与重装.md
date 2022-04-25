---
title: 备份:各种环境的备份与重装
date: 2020-08-15
tags:
    - Backup
categories:
    - Computer
---

# 前言

每一次大更新设备时，总想要有一个新的干净的环境，这就需要重装，二重装总是越快越好，所以有一些配置数据最好可以备份出来。

# 备份

先讲一下一些基本设备的数据备份......

## Apple

Mac 使用 timeMachine 备份即可，详见 Mac 设置那几篇。移动设备 iCloud 备份

## Windows

我的 Windows 只是辅助以及游戏影音功能，不设备份

# 配置导出

然后时重装时的配置导出......

## 移动设备

依赖于 iCloud 的设备同步，若是想要新环境，只能自己调整配置，下载软件。

## Mac

安装 brew 之后，安装 mas，然后

```
brew bundle dump
```

可以生成 brewfile，包括 brew 和 mas 中的软件，安装时 brew bundle 即可。其他软件依然是记录 md 文件

## 其他

zsh 和 bash 的配置在.zshrc 和.bashrc 中，备份即可，而 VScode 如下备份（要早 VSCode 中安装 code 命令行命令）

```
code --list-extensions > ext-list.txt
```

然后正则替换`(.*)\r?\n`变成`--install-extension $1`，最后加上 code 变成`code --install-extension [扩展1] --install-extension [扩展2] ... `就可以安装了

## 重装指南

在网盘中存一份重装指南，分门别类存好信息即可
