---
title: RSP3:HA智能
date: 2019-02-19
tags:
    - RSP
    - homekit
categories:
    - Computer
---

# 前言

这里准备做一些智能的设置，因为主要是使用 HK 作为控制中心，使用 HA 作为智能中心，所以对于 HA 的各种有趣界面的设置我也就不高兴做了，有兴趣可以看看墨澜的东西，写得还是不错的而且是中文的。

# 设置

## 需要调用设备的一些功能

比如灯要设置场景，空气净化器要设置各种模式（这些都会在 HA 中以 service 的形式置入，如果没有的话就不能控制了，就是说 HA 中智能做到以一定条件一定顺序调用各种 service，如果没有这个 service 就只能自己写 platform（基于 python）独立接入这个设备或者等开发人员更新了...）

在官网中 conponents 下搜索 template 可以发现许多可以自定义的组件，按需取用即可

## 智能场景

可以定义各种 script 和 automation，script 和 automation 将会以开关的形式在 HK 中显示。而 automation 可以调用 script。

script 本质是按照一定顺序调用一些服务，当然加入 condition 也可以检测当时的状态决定调用哪一些服务。而 automation 主要由 trigger 触发，之后会调用一些服务。

## 分组

可以按照官网中的知道，编写 group 文件，将 HA 的设备按照分类整理好，这样可以方便地找到需要的设备，在 customize 里面也可以将一些自动加入的设备（比如 zigbee 的传感器）的名字改成易读的名字。

# PS

这样就可以实现米家的大部分功能，并且智能部分是比米家要功能更多的（但是有些功能，比如网关的广播是不行的，当然广播要联网，我的目的是米家设备不联网，就算了也罢）

若需要两地不同的 homekit 但是却有公用的设备，可以植被碗一张 sd 卡后另外制备一张 sd，设备 token 可以使用第一张 sd 卡相同的，这时候开启 homekit 服务将会是另一个 homekit，然后在 ios 中切换到另一个家添加 homekit。
