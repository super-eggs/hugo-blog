---
title: "使用 GitHub Actions 部署 Hugo 到云服务器"
date: 2022-10-15T17:41:43+08:00
lastmod: 2022-10-15T17:41:43+08:00
author: ["SuperEggs"]
tags:
 - Hugo
 - Github Action
---

![image-20221016001305540](https://tva1.sinaimg.cn/large/008vxvgGly1h76fede7cjj316g0h8mxp.jpg)

## 期望

我的博客站是使用 hugo 搭建的，每次发布都要手动通过 FTP 上传到服务器，很是繁琐，现在想通过 GitHub Actions 实现每次提交后，自动部署到服务器。（果然懒是第一生产力）



不熟悉 GitHub Actions 的同学建议先看一下下面的入门教程。

[GitHub Actions 入门教程 - 阮一峰的网络日志 (ruanyifeng.com)](http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html)

[About workflows - GitHub Docs](https://docs.github.com/cn/actions/using-workflows/about-workflows)

## 实现

### 前置操作

添加两个部署要用到的仓库秘钥

入口：Settings →  Secrets → Actions → New repository secret

- SERVER_IP：服务器地址
- SSH_PRIVATE_KEY：部署用到的服务器 ssh 私钥

![image-20221015180656439](https://tva1.sinaimg.cn/large/008vxvgGly1h764tevlrkj31f70m1mzo.jpg)

### 代码

```yaml
name: Auto Deploy Hugo
on:
  push:
    branches:
      - main
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.91.2'

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: wlixcc/SFTP-Deploy-Action@v1.0
        with:
          username: 'root'
          server: '${{ secrets.SERVER_IP }}'
          ssh_private_key: ${{ secrets.SSH_PRIVATE_KEY }}
          local_path: './public'
          remote_path: '/var/www/hugo-blog'
```

## 讲解

```yaml
name: Auto Deploy Hugo # 工作流标题
```

~~~yaml
# 检测到在 main 分支执行 push 动作之后触发
on:
  push:
    branches:
      - main  # 设置要部署的分支
~~~

~~~yaml
# 工作流程中的一个任务
jobs:
  deploy: # 任务名称
~~~

~~~yaml
# 执行当前任务需要的的环境
runs-on: ubuntu-latest

# 需要执行的步骤，多个
steps:
~~~

~~~yaml
 # 第一步，检出代码
 - uses: actions/checkout@v3
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod
~~~

~~~yaml
# 配置 hugo 环境
- name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.91.2' # hugo 版本，latest: 表示最新版
~~~

~~~yaml
 # 打包 hugo 
 - name: Build
        run: hugo --minify # 最小化打包
~~~

~~~yaml
# 部署
- name: Deploy
        uses: wlixcc/SFTP-Deploy-Action@v1.0
        with:
          username: 'root'   # ssh user name
          server: '${{ secrets.SERVER_IP }}' # 引用之前创建好的secret
          ssh_private_key: ${{ secrets.SSH_PRIVATE_KEY }} # 引用之前创建好的secret
          local_path: './public'  # 对应我们项目build的文件夹路径（config.yml）
          remote_path: '/var/www/hugo-blog' # 要部署到的位置
~~~



