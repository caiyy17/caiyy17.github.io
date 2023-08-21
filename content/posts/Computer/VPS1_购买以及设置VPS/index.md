---
title: VPS1:购买以及设置
date: 2018-09-30
tags:
    - VPS
categories:
    - Computer
---

# 前言

拥有一台自己的服务器是十分有必要的。

## 连接服务器

Windows 可以用 putty，使用 ssh 登陆，然后在 ssh 设置中添加自己的私钥，就可以连了。

Mac 的话直接用系统自带的终端

```
ssh -p [port] [user]@[ip]
```

# 用户设置

默认登陆的是 root 用户，但是这样是不安全的，我们可以建立管理员用户，然后只允许管理员用户切换到 root，再禁止 root 的 ssh 登陆，这样我们要使用 root 就只能先 ssh 登陆管理员账户，然后再使用密码切换到 root。

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

这个在用户文件夹下的.ssh/authorized_keys 中可以添加 ssh 的公钥，然后就可以使用不同的私钥链接服务器了，保险起见得保证 **从修改文件到确认可以用修改后的钥匙登陆前** 最好都保持有一个终端窗口连在服务器上，这样玩意出问题还可以再次改那个文件。

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
