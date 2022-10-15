---
title: "[Tools] Brew 强大的软件包管理器👍"
date: 2022-03-22T10:43:03+08:00
lastmod: 2022-03-22T10:43:03+08:00
author: ["SuperEggs"]
tags:

- Tools
- Mac

---

## 介绍

brew是MAC与LINUX上的软件包管理器，类似于Linux中的yum与apt软件管理器 。

官网：[https://brew.sh/index\_zh-cn.html](https://brew.sh/index_zh-cn.html "https://brew.sh/index_zh-cn.html")

![](https://i.loli.net/2021/04/26/fhEiZosHCSIaxGy.png)

## 手动安装

### MAC

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### LINUX

和MAC系统下的HOMEBREW一样，[LinuxBrew (opens new window)](https://docs.brew.sh/Homebrew-on-Linux "LinuxBrew (opens new window)")
是简单方便的LINUX包管理软件，可访问[应用商店 (opens new window)](https://formulae.brew.sh/formula-linux/ "应用商店 (opens new window)")
查看应用软件细节。

* 软件相比YUM安装的要新些

* 安装在用户指定的目录，所以不需要`sudo`访问

* Brew 命令在MAC下支持服务开启和关闭，在LINUX暂不支持

**安装配置**

如果安装了zsh，需要将`source ~/.profile` 中有关brew配置添加到 `~/.zshrc` 文件头部

1. 输入下面指令执行安装，网络可能不稳定需要多试几次，祝你好运

   ```bash
   sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)"
   ```

2. 依次执行下面命令完成配置（新增用户不需要执行上面命令，执行下面就可使用brew）

   ```bash
   test -d ~/.linuxbrew && eval $(~/.linuxbrew/bin/brew shellenv)

   test -d /home/linuxbrew/.linuxbrew && eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)

   test -r ~/.bash_profile && echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.bash_profile

   echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.profile
   ```

3. 如果安装了zsh需要执行命令修改`.zshrc`配置文件

   ```bash
   echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.zshrc
   ```

4. 注销并重新登录

   ```bash
   exit
   ```

5. 下面来安装`nginx`来测试 neovim

   ```bash
   brew install neovim
   ```

### 国内镜像

使用国内镜像可以加快软件安装速度，依次执行以下命令即可设置为国内镜像

```bash
cd "$(brew --repo)"

git remote set-url origin [https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git](https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git)

cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"

git remote set-url origin [https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git](https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git)

brew update
```

执行以下命令可还原镜像

```bash
cd "$(brew --repo)"

git remote set-url origin [https://github.com/Homebrew/brew.git](https://github.com/Homebrew/brew.git)

cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"

git remote set-url origin [https://github.com/Homebrew/homebrew-core](https://github.com/Homebrew/homebrew-core)

brew update
```

## 一键安装 👍

<https://zhuanlan.zhihu.com/p/111014448>

### MAC

**常规安装脚本（推荐 完全体 几分钟安装完成）：**

```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

**苹果电脑 极速安装脚本（精简版 几秒钟安装完成）：**

```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)" speed
```

**苹果电脑 卸载脚本：**

```bash
/bin/zsh -c "$(curl -fsSL [https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh](https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh))"
```

### **Linux**

安装脚本：

```bash
rm [Homebrew.sh](http://Homebrew.sh) ; wget [https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh](https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh) ; bash [Homebrew.sh](http://Homebrew.sh)
```

卸载脚本：

```bash
rm [HomebrewUninstall.sh](http://HomebrewUninstall.sh) ; wget [https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh](https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh) ; bash [HomebrewUninstall.sh](http://HomebrewUninstall.sh)
```

### 安装**错误**

去下方[地址](https://link.zhihu.com/?target=https://gitee.com/cunkai/HomebrewCN/blob/master/error.md "地址")查看

[https://gitee.com/cunkai/HomebrewCN/blob/master/error.md](https://gitee.com/cunkai/HomebrewCN/blob/master/error.md "https://gitee.com/cunkai/HomebrewCN/blob/master/error.md")

## 开始使用

下面介绍使用brew管理软件包的操作。

### 搜索软件

```bash
brew search xxx
```

### 查看版本信息

```bash
brew info xxx
```

### 安装软件

如果用过Linux 中的 apt 或 yum ，brew使用方式与它们差不多，下面演示安装软件的方式。

安装 wget

```bash
brew install wget
```

安装 curl

```bash
brew install curl
```

安装composer

```bash
brew install composer
```

### 更新

更新&#x20;

```bash
brew update xxx
```

全部更新

```bash
brew upgrade
```

### 卸载软件

```bash
brew uninstall xxx
```

### 更多命令

<https://docs.brew.sh/Manpage>

## Brew-cask

### 两者区别:

* brew是下载源码解压然后./configure && make install, 并且会自动配置好环境变量。

* brew cask主要用于有GUI的软件，下载已经编译好的应用包(.dmg/.pkg)。

### 安装

```bash
brew install caskroom/cask/brew-cask
```

## brew 问题整理：

### 执行 brew 后报错，(无权限)Permission denied&#x20;

使用homebrew安装软件，例如 brew install python3时，会提示 “Permission denied @ dir\_s\_mkdir -
/usr/local/Frameworks”，因为权限问题而失败。
如果使用sudo brew install python3，则会提示“Running Homebrew as root is extremely dangerous and no longer supported.”
很困扰。这个时候可以通过给当前用户赋予权限的方法解决这个问题。

```bash
sudo chown -R $(whoami) /usr/local
```

1. brew安装的软件路径一般都是local下面，所以直接给与local的读写权限，可以避免以后再次遇到类似问题
2. whoami指代的是当前用户名称，这里直接使用就好。

