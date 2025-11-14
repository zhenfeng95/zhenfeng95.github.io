
---
title: 开发环境搭建
type: environment
order: 1
---

## 整体介绍

* Node.js环境


​		[参考文档](/backend/node/)

* git环境

​		[参考文档](/tool/git/)

* IDE


​		介绍最常见的Webstorm与vscode的使用方式

* 虚拟机


​		主要是windows与macOS平台下的虚拟化介绍，用于搭建docker环境，后续的开发环境全部建立在docker之上，安装在Linux环境（虚拟机）之内。

​		如果大家想使用云服务器，可以考虑购买各大云服务商的2C4G的配置，磁盘最少要40G（配置4G的swap），最好**按量付费**，用完即删。

* Linux入门


​		常见的Linux命令，可以上手即可，比如：vim编辑器、常见的文件操作命令、更新包安装包的命令等。

## homebrew(macOS包管理工具)

### 简介

`Homebrew`是一款包管理工具，目前支持`macOS`和`Linux`系统。主要有四个部分组成: `brew`、`homebrew-core` 、`homebrew-cask`、`homebrew-bottles`。

| 名称             | 说明                                  |
| ---------------- | ------------------------------------- |
| brew             | Homebrew 源代码仓库                   |
| homebrew-core    | Homebrew 核心源                       |
| homebrew-cask    | 提供 macOS 应用和大型二进制文件的安装 |
| homebrew-bottles | 预编译二进制软件包                    |

brew官网：[https://brew.sh/(opens new window)](https://brew.sh/)

## brew安装&卸载

推荐安装脚本

项目地址1：[https://gitee.com/cunkai/HomebrewCN(opens new window)](https://gitee.com/cunkai/HomebrewCN)

项目地址2：[https://brew.idayer.com/(opens new window)](https://brew.idayer.com/)

### 安装

用命令行安装工具前先检查一下子，用于在Mac上安装或更新的Xcode的命令行工具

```bash
xcode-select -p
```

没安装的话先进行安装

```bash
xcode-select --install
```

苹果电脑标准安装脚本：（推荐 优点全面 缺点慢一点）

```text
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

苹果电脑极速安装脚本：（优点安装速度快 缺点update功能需要命令修复 ）

```text
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)" speed
```

Linux 标准安装脚本：

```text
rm Homebrew.sh ; wget https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh ; bash Homebrew.sh
```

### 卸载

苹果电脑卸载脚本：

```text
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh)"
```

Linux卸载脚本：

```text
rm HomebrewUninstall.sh ; wget https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh ; bash HomebrewUninstall.sh
```

### brew使用，手动设置镜像

#### 中科大源

```shell
git -C "$(brew --repo)" remote set-url origin https://mirrors.ustc.edu.cn/brew.git

git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git

brew update
```

#### 清华大学源

```shell
git -C "$(brew --repo)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git

git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git

git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git

brew update
```

#### 设置 bottles 镜像

设置环境变量需要注意终端的类型，可以先通过以下方式获取：

执行命令`echo $SHELL`，根据结果判断：

* `/bin/zsh` => `zsh` => `.zprofile`
* `/bin/bash` => `bash` => `.bash_profile`

然后继续正式操作，以**中科大源**为例：

从`macOS Catalina`(10.15.x) 版开始，`Mac`使用`zsh`作为默认`Shell`，对应文件是`.zprofile`，所以命令为：

```shell
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles/bottles' >> ~/.zprofile
source ~/.zprofile
```

如果是`macOS Mojave` 及更低版本，并且没有自己配置过`zsh`，对应文件则是`.bash_profile`：

```shell
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles/bottles' >> ~/.bash_profile
source ~/.bash_profile
```

> 注意：上述区别仅仅是`.zprofile`和`.bash_profile`不同，上下文如有再次提及编辑`.zprofile`，均按此方法判断具体操作的文件。

至此，安装和设置操作都完成了。

#### 恢复默认源

```shell
git -C "$(brew --repo)" remote set-url origin https://github.com/Homebrew/brew.git

git -C "$(brew --repo homebrew/core)" remote set-url origin https://github.com/Homebrew/homebrew-core.git

git -C "$(brew --repo homebrew/cask)" remote set-url origin https://github.com/Homebrew/homebrew-cask.git

brew update
```

`homebrew-bottles`配置只能手动删除，将 `~/.zprofile` 文件中的 `HOMEBREW_BOTTLE_DOMAIN=https://mirrors.xxx.com`内容删除，并执行 `source ~/.zprofile`。

## 本地开发环境

### IDE

* WebStorm（收费）：[https://www.jetbrains.com/webstorm/](https://www.jetbrains.com/webstorm/)
* VSCode（推荐）：[https://code.visualstudio.com/](https://code.visualstudio.com/)
* Atom：[https://atom.io/](https://atom.io/)
* Hbuilderx：[https://www.dcloud.io/hbuilderx.html](https://www.dcloud.io/hbuilderx.html)
* 微信开发者工具：[https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)
* 谷歌浏览器：[下载地址](https://www.google.com/chrome/?brand=CHBD&ds_kid=20441638724&gclsrc=aw.ds&gad_source=1&gad_campaignid=22008060471&gbraid=0AAAAAoY3CA6rv_DT0ukd9Y-OqU2Vum9lJ&gclid=CjwKCAiAw9vIBhBBEiwAraSATupbbC0FQq3XxJscjPQjcSIxEpMu6uAWwxSEVmXasPJ4nz5CYiydhBoCt6wQAvD_BwE)

### Vue-CLI

Vue官方的开发[CLI工具 (opens new window)](https://cli.vuejs.org/zh/guide/)，安装Node之后，使用npm命令安装：

```text
npm i -g @vue/cli
```

CLI 服务是构建于 [webpack (opens new window)](http://webpack.js.org/)和 [webpack-dev-server (opens new window)](https://github.com/webpack/webpack-dev-server)之上的。它包含了：

* 加载其它 CLI 插件的核心服务；
* 一个针对绝大部分应用优化过的内部的 webpack 配置；
* 项目内部的 `vue-cli-service` 命令，提供 `serve`、`build` 和 `inspect` 命令。

如果你熟悉 [create-react-app (opens new window)](https://github.com/facebookincubator/create-react-app)的话，`@vue/cli-service` 实际上大致等价于 `react-scripts`，尽管功能集合不一样。

## 测试环境

### 虚拟机自建环境（不推荐，比较麻烦）

* Parallels -> 针对 macOS
* VMWare -> 全平台
* Hyperv -> Windows平台支持

### 云服务器（推荐）

阿里云：[https://cn.aliyun.com/(opens new window)](https://cn.aliyun.com/)

腾讯云：[https://cloud.tencent.com/(opens new window)](https://cloud.tencent.com/)

华为云：[https://www.huaweicloud.com/(opens new window)](https://www.huaweicloud.com/)

AWS：[https://aws.amazon.com/cn/(opens new window)](https://aws.amazon.com/cn/)

### Docker服务（推荐宝塔一键安装）

* macOS：[https://docs.docker.com/desktop/mac/install/ (opens new window)](https://docs.docker.com/desktop/mac/install/)里面有两个很大的按钮(按照自己的CPU版本来选择)：

  ![image-20220112002442828](Environment.assets/image-20220112002442828.cde166a6.png)

* Linux：[https://github.com/docker/docker-install(opens new window)](https://github.com/docker/docker-install)

  推荐使用get-docker的脚本：

  ```text
  curl -fsSL https://get.docker.com -o get-docker.sh
  sh get-docker.sh
  ```

* Windows：[https://hub.docker.com/editions/community/docker-ce-desktop-windows(opens new window)](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

安装docker-compose集成命令（只针对Linux）服务：[https://docs.docker.com/compose/install/(opens new window)](https://docs.docker.com/compose/install/)

```text
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

> Desktop下载后，自己会带docker-compose，不需要另外安装

#### Docker加速服务

* 阿里云加速器([点击管理控制台 (opens new window)](https://www.aliyun.com/product/acr?source=5176.11533457&userCode=8lx5zmtu)-> 登录账号(淘宝账号) -> 右侧镜像工具 -> 镜像加速器 -> 复制加速器地址)
* 网易云加速器 [`https://hub-mirror.c.163.com`(opens new window)](https://www.163yun.com/help/documents/56918246390157312)
* 百度云加速器 [`https://mirror.baidubce.com`(opens new window)](https://cloud.baidu.com/doc/CCE/s/Yjxppt74z#使用dockerhub加速器)
* 科大加速器 `https://docker.mirrors.ustc.edu.cn/`
* 七牛云加速器：`https://reg-mirror.qiniu.com`

Linux中设置`/etc/docker/daemon.json`：

```text
{
  "registry-mirrors": [
    "https://hub-mirror.c.163.com",
    "https://mirror.baidubce.com"
  ]
}
```

Mac/Windows上设置：

```text
{
  "registry-mirrors": [
    "https://hub-mirror.c.163.com",
    "https://mirror.baidubce.com"
  ]
}
```

使用`docker info`查看是否设置生效：

```text
Registry Mirrors:
 https://hub-mirror.c.163.com/
```

## 终端bash&zsh&fish对比

### bash

默认的shell工具，大多的Linux系统自带的。

bash2也有自动补全与语法高亮，工具[bel.sh(opens new window)](https://github.com/akinomyoga/ble.sh)

### zsh

zsh相对于bash 高可配置、高扩展。目前是mac上的默认的shell工具。推荐它的主题网址：[https://ohmyz.sh/(opens new window)](https://ohmyz.sh/)

通过扩展可以获得如下功能：

* 自动补全
* 语法高亮
* 插件系统（插件管理）
* 命令行提示（git仓库）
* 颜色主题

下面截图的就是一个示例，这些都需要手动设置。

![image-20220130112837562](Environment.assets/image-20220130112837562.3ba326aa.png)

> bash 与 zsh已经诞生大概有30年了历史了。

### fish

### 特点

官网：[https://fishshell.com/(opens new window)](https://fishshell.com/)

相比于zsh，fish会有很多自动化的配置，默认的安装即可。

特点：

* 命令历史
* 自动补全、自动搜索
* 语法高亮
* 运算+逻辑
* 运行行颜色设置

### 安装方法

安装方法：

macOS上：

```text
brew install fish
```

windows上可以通过

* [MSYS2 (opens new window)](https://msys2.github.io/)命令：`pacman -S fish`
* 安装配置[CygWin (opens new window)](https://cygwin.com/)，在安装的过程中可以选择fish作为默认的shell

### 美化&扩展

omf就是fish的一个插件管理工具，扩展fish主题配色，项目地址：[https://github.com/oh-my-fish/oh-my-fish (opens new window)](https://github.com/oh-my-fish/oh-my-fish)。

> 还有一个插件管理工具是fisher：[https://github.com/jorgebucaran/fisher(opens new window)](https://github.com/jorgebucaran/fisher)

强烈推荐`starship`来美化你的fish shell，地址：[https://starship.rs/ (opens new window)](https://starship.rs/)，安装方式：

* brew方案：

```sh
brew install starship
```

* Linux

```text
sh -c "$(curl -fsSL https://starship.rs/install.sh)"
```

添加配置：

```bash
# fish配置文件 ~/.config/fish/config.fish
starship init fish | source

# 选择一：~/.bashrc 针对于平时使用bash的小伙伴
eval "$(starship init bash)"


# 选择二：~/.zprofile 针对于平时使用zsh的小伙伴
eval "$(starship init zsh)"
```