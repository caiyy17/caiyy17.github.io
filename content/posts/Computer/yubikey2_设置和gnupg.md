---
title: yubikey2:配置和gnupg
date: 2018-09-30
tags:
    - Computer
    - safety
categories:
    - Computer
---

- [设置 yubikey](#设置-yubikey)
  - [初始化](#初始化)
  - [U2F](#u2f)
  - [PIV](#piv)
- [GPG](#gpg)
  - [安装 gnupg](#安装-gnupg)
  - [生成要最终导入 yubikey 的密钥](#生成要最终导入-yubikey-的密钥)
  - [导出密钥](#导出密钥)
  - [设置 yubikey 的 pin](#设置-yubikey-的-pin)
  - [设置 yubikey 的 PGP](#设置-yubikey-的-pgp)
  - [测试](#测试)
  - [SSH](#ssh)
  - [注意](#注意)
- [后记](#后记)

# 设置 yubikey

讲完了安全模型，就可以开始设置 yubikey 了

需要设置的有这些：

-   次重要密码导入 slot1
-   Master 密码导入 slot2
-   yubikey 的 pin（Masterpin）
-   管理员 8 位 pin（特殊 pin）
-   GPGkey（因为需要备份，我们从外部导入）
-   PIV（Masterpin）
-   U2F

从简单的开始

## 初始化

下载 yubikey manager，然后打开所有功能。在 OTP 里设置 slot1 和 slot2

## U2F

在支持的网站里双重验证下添加 key，然后按一下 yubikey 就行了

## PIV

下载 yubikey PIV manager，按照提示设置就行了。

# GPG

这个比较复杂，单独出来说。

## 安装 gnupg

这是个专门用于 gpg 的软件，设置可以参考我 mac 设置那篇文章。

## 生成要最终导入 yubikey 的密钥

虽然有些 yubikey（比如说我的 4-NEO）不支持 4096 位密码，但主密钥还是可以生产 4096 的，最后只要把子密钥导入就行了。

```
gpg --full-generate-key
Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
Your selection? 1
```

```
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048) 4096
Requested keysize is 4096 bits
```

```
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y
```

```
GnuPG needs to construct a user ID to identify your key.

Real name: Name
Email address: name@example.com
Comment:
You selected this USER-ID:
    "Name <name@example.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
```

中途还会有设置密码，貌似不能设为空密码。接下来开始设置子密钥。

```
gpg --expert --edit-key Name
gpg> addkey
```

连续设置签名，加密和认证密钥，但只能 2048 位。

```
Please select what kind of key you want :
   (3) DSA (sign only)
   (4) RSA (sign8 only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
Your selection? 4
//E用6，A用8（然后关闭S和E，开启A）
```

```
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want?（密钥长度） 2048
Requested keysize is 2048 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) （有效期）0
```

最后 save 保存。

## 导出密钥

注意接下来要导入了，但导入后原来的私钥就废了，所以 **一定要先导出！**

```
gpg --export -a Name > public.key
gpg --export-secret-key -a Name > private.keyq
```

然后转移到离线设备上，好好保存。

## 设置 yubikey 的 pin

插入 yubikey 后，可以在 `gpg --help` 中看可以干什么，有一个 `gpg --edit-card` 的命令。

```
gpg --card-edit

gpg/card> admin

gpg/card> passwd
    1 - change PIN
    2 - unblock PIN
    3 - change Admin PIN
    4 - set the Reset Code
    Q - quit
Your selection? 3

//修改admin pin（8位），初始12345678

PIN changed.

1 - change PIN
2 - unblock PIN
3 - change Admin PIN
4 - set the Reset Code
Q - quit

Your selection? 1

//修改pin（6位），初始123456
```

然后可以设置各种属性数据（不过这个并不重要）

## 设置 yubikey 的 PGP

开始导入

```
gpg --edit-key Name

gpg> key 1
gpg> keytocard
Please select where to store the key:
   (1) Signature key
   (3) Authentication key
Your selection? 1
```

```
gpg> key 1
gpg> key 2
gpg> keytocard
Please select where to store the key:
   (2) Encryption key
Your selection? 2
```

```
gpg> key 2
gpg> key 3
gpg> keytocard
Please select where to store the key:
   (3) Authentication key
Your selection? 3
```

```
gpg> save
```

这样就结束了。

## 测试

删除本地 GPG 钥匙环里的私钥，用 Yubikey 测试一下。如果可以的话，以上的就没问题了。

```
gpg --delete-secret-keys Name
```

运行 `gpg --card-status`，系统会建立一个像是指针的东西，指向智能卡。

测试签署一下：

```
gpg -s testfile
```

没问题的话就可以了。

## SSH

接下来还想用 yubikey 连接服务器

安装好 gpg-agent（有可能 gpg 自带？没有的话 brew 安装一下）

1. 导出公钥成 ssh 可理解的形式

    ```
    gpg --export-ssh-key Name ~/ssh_public_key.pub
    ```

    然后这个.pub 就和一般 ssh 的 key 没区别了，上传服务器就是了。

2. 配置 gpg-agent

    在~/.gnupg/gpg-agent.conf 中写入

    ```
    pinentry-program /usr/local/bin/pinentry
    enable-ssh-support
    ```

    gpg-agent 需要经常重启...这里可以用脚本：

    ```
    #!/bin/zsh
    source ~/.zshrc
    killall gpg-agent
    eval $(gpg-agent --daemon --enable-ssh-support)
    echo "restart"
    ```

## 注意

理论上现在就完全可以了，但注意你的 yubikey 的公钥要在电脑里，没有的话导入一下，或者你公钥在公钥服务器上的话 fetch 一下。

# 后记

这样，yubikey 就差不多设置好了，愉快使用吧。
