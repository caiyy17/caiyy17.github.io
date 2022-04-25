---
title: VPS1:购买以及设置
date: 2018-9-30
tags:
    - VPS
    - safety
categories:
    - Computer
---

# 前言

对于一个准备完计算机的喜欢折腾的学生来说，拥有一台自己的服务器是十分有必要的，在那上面，可以干许许多多事情。

# Github 学生包

相信大家对 github 都不陌生，但 github 上除了代码管理和社区之外，github 提供的学生包也是个非常不错的东西。学生包获取方法就根据提示一步一步来就行，需要学校邮箱。

其中个人暂时用到的有：

-   DigitalOcean 上 50 刀的代金券
-   AWS 上 150 刀代金券
-   Namecheap 一年的免费.me 域名

这些基本都和 VPS 有关，本文主要介绍 DO 上的 VPS，这个也比较简单。

# DigitalOcean

注册 DO，需要用 paypal 或者信用卡向其中充值 5 美金，就可以注册完成了。完成后，在 DO 网站中输入学生包优惠码，会发现账户变成了 55 美金，这样就可以用基础套餐 11 个月了。

然后添加 Droplets，我使用的是 centos，最基础的套餐（5 刀一个月），用校园网的话记得勾选 ipv6，然后 **添加一个 sshkey** （这一步很重要，不然申请完成后需要再设置这个 VPS 很多步）

然后就完成了基本操作，可以使用电脑连接服务器了。

## 设置防火墙

DO 提供了十分方便的防火墙设置，可以控制进出开放的端口，然后你可以只开你需要的端口，可以免去在 VPS 里面设置防火墙，甚至可以把里面的防火墙关了。

这里一般会设置几套防火墙，每一套之间是叠加的，就是最终开放的端口是所有防火墙组的开发端口的并集。可以有前段防火墙开发基本端口，然后 ss 防火墙专门开发 socks 协议使用的端口......这个就看自己组合了。

我这边是开了 ssh 使用的 22 端口；网站的 443 和 1080 端口；以及 ss 或者 v2ray 使用的端口（自己设置）。如果不用网站，可以将 443 或者 1080 作为翻墙端口。

## 连接服务器

Windows 可以用 putty，使用 ssh 登陆，然后在 ssh 设置中添加自己的私钥，就可以连了。

Mac 的话直接用系统自带的终端

```
ssh -p [port] [user]@[ip]
```

# 用户设置

默认登陆的是 root 用户，但是这样是不安全的，我们可以建立管理员用户，然后只允许管理员用户切换到 root，再禁止 root 的 ssh 登陆，这样我们钥使用 root 就只能先 ssh 登陆管理员账户，然后再使用密码切换到 root。

1. 新建一个用户

    ```
    useradd [user]
    ```

2. 将用户添加至管理员组

    ```
    usermod –g wheel [user]
    ```

3. 修改/etc/pam.d/su 配置

    去掉这行的注释，就完成了禁止普通用户切换 root

    ```
    #auth required pam_wheel.so use_uid
    ```

4. 修改/etc/ssh/sshd_config

    将 PermitRootLogin 选项改为

    ```
    PermitRootLogin no
    ```

这样就禁止了 root 直接登陆。

# ssh 设置

这个在用户文件夹下的.ssh/authorized_keys 中可以添加 ssh 的公钥，然后就可以使用不同的私钥链接服务器了，然是保险起见得保证 **从修改文件到确认可以用修改后的钥匙登陆前** 最好都保持有一个终端窗口连在服务器上，这样玩意出问题还可以再次改那个文件。

# OK

然后就可以愉快地在服务器中安装各种软件了......附上一些现成代码

```
useradd -g wheel admin
passwd admin

echo "" >> /etc/pam.d/su
echo "auth required pam_wheel.so use_uid" >> /etc/pam.d/su

echo "" >> /etc/ssh/sshd_config
echo "RSAAuthentication yes" >> /etc/ssh/sshd_config
echo "PubkeyAuthentication yes" >> /etc/ssh/sshd_config
echo "PermitRootLogin no" >> /etc/ssh/sshd_config
echo "PasswordAuthentication no" >> /etc/ssh/sshd_config
service sshd restart

su admin
cd ~
mkdir .ssh
chmod 755 .ssh
cd .ssh
touch authorized_keys
echo [your_public_keys] > authorized_keys
chmod 644 authorized_keys
```
