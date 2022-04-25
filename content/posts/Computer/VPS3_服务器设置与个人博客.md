---
title: VPS3:nginx服务与Hexo博客
date: 2018/9/30
tags:
    - VPS
    - Blog
categories:
    - Computer
---

- [前言](#前言)
- [HEXO 安装](#hexo-安装)
- [nginx](#nginx)
- [与服务器关联](#与服务器关联)
- [修改 nginx 配置](#修改-nginx-配置)
- [本地操作](#本地操作)
- [总结](#总结)
- [PS](#ps)
- [githubio](#githubio)

# 前言

一直想做一个个人博客，但又不想在个大博客网站上写，这样很不自由，还不如微信朋友圈（其实主要是感觉 VPS 只用来挂 ss 太亏了，想玩玩...）但是自己做网站代价很大，而 hexo 便是现成的可以直接挂 blog 的软件，又有丰富的主题科插件。

# HEXO 安装

在 Mac 中需要安装 git 和 nodejs，这两个都在 brew 中安装就可以了，brew 安装相关具体可以看
[Mac 设置](/2018/08/15/Computer/Mac1_新机设置/)
这篇文章，安装好之后便可以安装 hexo（根据官网来就行）

```
npm install -g hexo-cli
```

然后在需要建站的文件夹内：（[folder]为需要建立的文件夹名字）

```
$ hexo init [folder]
$ cd [folder]
$ npm install
```

然后更据提示修改\_config.yml 配置文件就行。hexo 的官方文档还是相当详细的。

可以通过 git clone 在 themes 文件夹下安装各种主题，官网有提供一些主题，也可以网上搜，配置同样在各个主题文件夹的\_config.yml 中，当然你也可以改动它的 css 和 js。

这里提一下 layout 是用于生成 html 的，比如博客底部的 powered by 相关字段，如果想修改可以进去找 ejs 文件中相关的代码。在我设置过程中我的 theme 生成的 html 对于 about 文件的引用就是不对的（它引用了 about.html 但是 hexo 生成的 about 是在 about/index.html 里面，找到 theme 里 ejs 中提到这个地址的代码改一下就行了）。

反正，一切都可以改。

然后用

```
$ hexo server -p 5000
```

这样的命令就可以在 localhost 中看到自己的 blog 了。

注意一点就是配置文件":"后面要加空格，这个很敏感，不如回错误...blog 内容就这么多，之后都是设计的工作了，接下来接着讲环境配置。

# nginx

这里假定服务器已经配置好，登陆服务器 shell 开始安装 nginx 相关程序并进行简单配配置。

安装 LNMP：（感觉以后肯定会有用的，不如一起装了）

```
yum install epel-release
yum update
yum install nginx
systemctl start nginx //启动nginx
systemctl enable nginx //设置开机启动

yum install php
systemctl start php-fpm //开启php-fpm
systemctl enable php-fpm //开机自动启动

yum install mariadb //mysql
```

# 与服务器关联

本地配置好 hexo，在配置文件中写入

```
deploy:
- type: git
  repo: user@服务器IP:Hexoblog.git(git路径)
```

本地就准备好了。服务器端配置：

创建一个网站的根目录（用于存放网站的部署的静态文件，就是 nginx 配置中的 root）

```
cd /
mkdir /var/www/Hexoblog
```

在用户目录添加一个.git 用于与本地交流

```
cd ~
git init --bare Hexoblog.git
cd ~/Hexoblog.git/hooks //配置hook
vim post-receive
```

在 post-receive 中写入：

```
#!/bin/zsh //我用的是zsh
cd /var/www/Hexoblog //网站root
unset GIT_DIR //解放路径
git fetch --all && git reset --hard origin/master && git pull //强制同步
```

添加操作权限

```
chmod +x ~/Hexoblog.git/hooks/post-receive
```

# 修改 nginx 配置

配置在 config 里面

```
vim /etc/nginx/nginx.conf
```

```
server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  [域名];
        root         /var/www/Hexoblog;
        index index.html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
}
```

注意需要将网站放的位置路径上一系列文件有+x 权限，并且网站根目录设置为 777 权限，这样使用 git 的用户才能写入。

# 本地操作

接下来，本地编辑好文件后，使用

```
cd [blog本地路径]
hexo generate
hexo d
```

就可以与云端同步了，

# 总结

nginx 可以做的远远不止 blog，之后可以做更多有趣的网站方面的尝试，包括监控网站状态，更多的网站功能......但对于简单的 blog，就这些就够了。

# PS

增加 Katex 支持（公式编辑）

下载插件[hexo-renderer-markdown-it-plus](https://github.com/CHENXCHEN/hexo-renderer-markdown-it-plus)

根据提示安装即可

```
npm un hexo-renderer-marked --save
npm i hexo-renderer-markdown-it-plus --save
```

然后注意要添加 css，一般在对应的 theme 文件夹里的 css 文件夹，又一个 all.styl，文本编辑打开 export 目标的 css（注意有可能有兼容问题，把 katex 导入在第一个目前没有遇到什么问题）

# githubio

其实完全不用那么复杂，静态网站直接放在 github 上，直接在 github 上建立[username].github.io 的公开仓库，然后在设置里面写上：

```
deploy:
  type: git
  repo: git@github.com:[username]/[username].github.io.git
```
