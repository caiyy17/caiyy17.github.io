---
title: Mac新机设置
date: 2018-08-15
tags:
    - Computer
    - Apple
categories:
    - Computer
---

# 前言

新的 mbp2018 总算到了，这也是我自己的第一台苹果电脑，自然要熟悉好一会儿，在此记录下我的折腾过程。

# 基础：系统偏好设置

在设置中，我增加了这些东西

1. 调度中心：使窗口按应用程序成组
2. 调度中心：设置触发角（左下角桌面，右下角关闭屏幕）
3. 节能：屏幕不关闭
4. 安全与隐私：打开防火墙
5. 通知：设定哪些程序可以通知
6. 键盘：文本取消自动纠正与智能引号
7. 触控板：功能全勾上
8. iCloud：功能全勾上
9. 扩展："今天"中显示的内容
10. 命令行设置显示全路径

    ```shell
    defaults write com.apple.finder _FXShowPosixPathInTitle -bool TRUE; killall Finder
    ```

11. 更改截屏保存地址

    ```shell
    defaults write com.apple.screencapture location "路径"; killalll SyatemUIserve
    ```

12. Finder 偏好设置

    边栏->全勾上

    高级->勾选"显示所有文件扩展名"和"按名称排序时保持文件夹在顶部"

    顶部 显示->显示标签页栏，显示状态栏

    在任意文件夹空白处->右键查看显示选项->改设置并用作默
    可以作用于之后新建的文件夹

    目前设置：
    排列方式：无；排序方式：名称；显示项目简介；显示图标预览

13. 文本编辑偏好设置

    新建文稿->格式->纯文本

    新建文稿->文本取消自动纠正与智能引号

    打开和储存->不自动添加.txt 扩展名

# 基础：几个好用的快捷键

1. command+delete 删除；command+shift+delete 清空垃圾箱
2. command+q 退出程序；command+w 退出当前标签页
3. command+i 显示信息；command+o 打开文件
4. command+Up 上一级目录；command+Down 下一级目录
5. command+control+F 全屏
6. command+T 新建标签页；command+N 新建窗口；command+shift+N 新建文件夹
7. 其他捷径看键盘偏好设置
8. Finder 快捷键看工具栏中的标注
9. 触控板使用看触控板偏好设置
10. command+space：spotlight 搜索
11. 按住绿色全屏按钮可以实现分屏

# 基础：安装日常应用（见软件篇）

# 进阶：安装编程相关应用并配置

1. Xcode+Clion

    申请 Apple 开发者账号，从 AppStore 安装 Xcode，生成 macOS 选项下的 CommandLineTool 写 C++工程；Clion 直接下载安装就行，当然有学生免费。

2. VSCode+Git

    安装 VSCode，安装 Git，运行 VSCode 并安装插件

    配置 Git，在网页 Github 的 develop 设置里加一个个人密码用于 git 的操作（代替密码），并且设置本地记住密码

    ```shell
    git config --global credential.helper store
    ```

    然后本地新建仓库：（在需要建立的文件夹下）

    ```shell
    git init
    git add .
    git commit -m "First Commit"
    git remote add origin <仓库地址>
    git pull --rebase origin master
    git push -u origin master
    ```

    或从远程拉取仓库：

    ```shell
    git clone <仓库地址>
    ```

3. Latex

    官网下载 MacTex，安装后运行一遍，之后配置 VSCode 的配置文件

4. **_Iterm2 及相关_**

    **_Iterm2，oh-my-zsh，homebrew，gnupg_**

    - 安装 Iterm2
    - 官方安装 brew
    - 安装 oh-my-zsh

        ```shell
        sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
        ```

    - 安装主题

        安装的主题都在~/.oh-my-zsh/themes 里面，若没有想要的主题可以在网上安装（git clone 然后把.zsh-theme 拷贝到 themes 文件夹里即可）

        然后在~/.zshrc 修改主题名字，再

        ```shell
        source ~/.zshrc
        ```

    - 安装插件

        ```shell
        cd ~/.oh-my-zsh/custom/plugins
        git clone <插件地址>
        ```

        然后在~/.zshrc 中的 Plugins 选项里加入下载的插件，再

        ```shell
        source ~/.zshrc
        ```

        一些推荐：

        - z（可以以简写跳转历史记录中的目录）
        - colored-man-pages
        - zsh-autosuggestions
        - zsh-syntax-highlighting

    - 修改配色和背景

        这个自己改就行了，也可以网上下现成的然后导入进去

    - 安装 gnupg

        使用 brew 安装 gnupg，然后写配置文件~/.gnupg/gpg-agent.conf

        ```shell
        pinentry-program /usr/local/bin/pinentry
        enable-ssh-support
        ```

        若需要图形界面，可以 brew 安装 pinentry-mac，然后

        ```shell
        pinentry-program /usr/local/bin/pinentry-mac
        ```

    P.S. 更换 brew 更新源

    ```shell
    # 替换brew.git:
    $ cd "$(brew --repo)"
    # 中国科大:
    $ git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
    # 清华大学:
    $ git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git

    # 替换homebrew-core.git:
    $ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
    # 中国科大:
    $ git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
    # 清华大学:
    $ git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git

    # 替换homebrew-bottles:
    # 中国科大:
    $ echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
    $ source ~/.bash_profile
    # 清华大学:
    $ echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.bash_profile
    $ source ~/.bash_profile

    # 应用生效:
    $ brew update
    ```

    （之后还可以安装 brew cask，以及 cake 和 launchpad 两个可视化软件）

# 进阶：其他设置

1. 创建一个 TempUser 账户供其他人用，设置家长控制，限制其权限
2. 设置时间机器
3. 分区

    分一个存放大的下载文件的盘并排除在 time machine 之外。然后在其中建立一个 temp 文件夹用于存放临时文件，将其快捷方式放在桌面上。

# 进阶：Windows 相关

安装 WinToGo

1. Win 下载 WinToGo 制作软件和官方 iso

    用它制作安装盘，选择 uefi 启动方式（这会彻底格式化整个硬盘），vhd 设置大小为 C 盘大小

2. Mac 下载 BootCamp 的驱动程序（在菜单中找）放入 U 盘
3. Mac 下 command+R 启动，使用工具中关闭加密启动，允许外部安装
4. Mac 下 Option 启动安装，，操作中需要一个外置鼠标，最后安装驱动

# 服务器相关

1. 连接服务器

    使用 brew 安装 gnupg，将公钥放到服务器

    P.S. 如果 gpg 崩了就（若使用 sh 执行，执行时用"source 文件名"，也即". 文件名"）

    ```shell
    killall gpg-agent
    eval $(gpg-agent --daemon --enable-ssh-support)
    ```

    然后将私钥放入`~/.ssh`并更改权限为 700，公钥放服务器的`~/.ssh`，接下来

    ```shell
    ssh-add -K 私钥文件
    #这样就吧key加好了

    ssh -p [port] 用户名@服务器地址
    ```

2. 使用 yubikey 登陆

    安装 PIV Manager 和 Yubikey Manager，然后再

    ```shell
    brew install yubikey-personalization
    ```

    根据 PIV 的提示设置 PIN，然后就设置完成了，接着连着 Yubikey 就可以使用 PIN 登陆 Mac，而 mac 本身可以设置一个复杂的密码，放在 Yubikey 的 Slot1 或 2（我的设置可以见 yubikey 设置的文章）

3. 使用 yubikey 登陆服务器

    详情可以见 yubikey 设置的文章。

    在~/.gnupg/gpg-agent.conf 中写入

    ```
    pinentry-program /usr/local/bin/pinentry
    enable-ssh-support
    ```

    gpg-agent 需要经常重启...这里可以用脚本：

    ```shell
    #!/bin/zsh
    source ~/.zshrc
    killall gpg-agent
    eval $(gpg-agent --daemon --enable-ssh-support)
    echo "restart"
    ```

    之后"ssh-add -l"卡中的钥匙应该会自动出现，（有可能需要导入公钥信息）。

    其他私钥.ssh 中的私钥"ssh-add -K 文件"加一下就行了。
