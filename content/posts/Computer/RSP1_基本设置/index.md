---
title: RSP1:基本设置
date: 2018-12-20
tags:
    - RSP
categories:
    - Computer
---

# 前言

又折腾买了个树莓派，正好右 32g 不用的 tf 卡，就准备来玩玩看可以做些什么。

# 安装系统

网上教程乱七八糟，最后还是官方文档比较清楚，下载系统解压后使用 etcher 烧录至 tf 卡，然后插入树莓派就可以启动了。

# 连接树莓派

先在根目录下加一个空白的名为 ssh 的文件让树莓派支持 ssh 连接，然后将电脑和树莓派通过网线相连。获取树莓派 ip 地址：

```
ping raspberrypi.local
```

然后就可以 ssh 连接这个树莓派，用户名"pi"，密码"raspberry"。接下来使用

```
sudo raspi-config
```

进行基础设置，更改密码，地区设置（加入 zh-CN UTF8 支持，但个人依然只有 en-GB UTF8 作为默认语言），允许 ssh，扩展储存（高级设置里扩展储存选项）

然后安装 vim，zsh 等常用的软件。

# 更换清华源

```
sudo nano /etc/apt/sources.list

//注释掉原来的，加入
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi

//保存退出，然后

sudo nano /etc/apt/sources.list.d/raspi.list

//注释掉原来的，加入
deb http://mirror.tuna.tsinghua.edu.cn/raspberrypi/ stretch main ui
deb-src http://mirror.tuna.tsinghua.edu.cn/raspberrypi/ stretch main ui
```

就可以更新了

```
sudo raspi-config
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
sudo rpi-update
sudo reboot //重启
```
