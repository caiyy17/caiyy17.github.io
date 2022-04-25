---
title: VPS2:socks协议以及ss设置
date: 2018-9-30
tags:
    - VPS
    - safety
categories:
    - Computer
---

# Socks 协议

Socket Secure(SOCKS)是一个网络协议，可以通过代理服务器来路由客户端和服务器之间的数据包。一个 SOCKS 服务器可以代理 TCP 连接到任何一个 IP 地址，同时为 UDP 的数据包提供一种转发方式。
SOCKS 完成的是 OSI 模型中第五层的工作（会话层，位于表示层和传输层之间）

## 历史

这个协议最初由 David Koblas 创造，David Koblas 是 MIPS 计算系统的管理员，在 1992 年 MIPS 被 Silicon Graphics 接管之后，Koblas 在那年的 Usenix 安全研讨会上发表了关于 SOCKS 的一篇文章，让 SOCKS 为公众所知。

SOCKS5 协议最初是一个安全协议被用来更容易的管控防火墙和其他安全产品，在 1996 年被 IETF 所批准，并与 Aventail 公司合作开发。

## 用途

SOCKS 是 Circuit-level gateway（注：Circuit-level gateway 是一种防火墙）事实上的标准

另外一种 SOCKS 的使用方式是作为规避工具，允许流量绕过网络过滤来获得内容，否则就被政府，学校，一些具体国家的 web 服务堵塞。（就是翻墙...）

而 shadowsocks 就是这样的一款软件。

# ss 设置

说这么多，就让我们在服务器上装 ss 吧

## 配置环境

安装 pip，接着安装 ss。

```shell
yum install python-setuptools && easy_install pip
pip install shadowsocks
```

## 修改配置

可以添加多用户。

```
vim /etc/shadowsocks.json
```

内容如下：

```json
{
    "server":"::",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "port_password":
    {
        "[port]":"[密码]",
        //继续写其他端口
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": ture
}
```

然后启动就行

```
ssserver -c /etc/shadowsocks.json -d start
```

防火墙开着的话，记得改一下防火墙。

# 客户端

找个地方下载客户端就行，配置一目了然。

# v2ray

现在 ss 已经会被封了，可以切换 vmess 协议

```
bash <(curl -L -s https://install.direct/go.sh)
sudo service v2ray start
```

v2ray 会生成一个 uuid 和一个端口，这可以在配置中改，同样可以使用多用户配置。推荐一个网站[V2Ray 配置指南](https://toutyrater.github.io)
