---
title: "Scoop：Windows 上的开源包管理器"
slug: scoop
aliases:
    - "/p/scoopwindows-上的开源包管理器/"
description: 无需管理员权限即可安装、更新、卸载软件的 Windows 包管理器，附镜像源与 bucket 扩展配置。
date: 2026-08-14T21:25:42+08:00
image: 
math: 
license: 
comments: true
draft: false
build:
    list: always   
categories:
    - 优秀软件
tags:
    - windows
    - scoop
    - open-source
---

`scoop` 基于Windows系统，提供的无需提权(不需要管理员权限)软件管理方案。具备极高的灵活性和定制性。有着在linux系统下使用包管理器的体验，一定程度上比linux的包管理器好用，这个是真的不用提权。

## 安装

值得注意的是,scoop默认情况下是禁止powershell以管理员权限安装的，需要在普通用户模式下运行

```shell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

### 配置镜像源

基本操作，提高scoop的工作效率

```shell
scoop config SCOOP_REPO "https://gitee.com/scoop-installer/scoop" 
scoop update
```
### 额外操作

scoop有一种叫做bucket的概念，你可以通过增加bucket来扩展你可以在scoop下载、安装、管理的软件。也就是说他们需要配置额外的镜像源。

慢慢安装，其实bucket有不少需要装的。主要有main、extras、dorado、echo、scoopcn、siku、versions好多
```shell
scoop bucket add main
```

## 使用
如下是scoop的主要使用方法
```shell
scoop bucket add extras           # 添加扩展仓库
scoop search <name>               # 搜索软件
scoop install <name>              # 安装
scoop install <bucket>/<name>@版本号 # 带版本号安装
scoop reset <name>@版本号         # 回退到 已安装 指定版本 
scoop list                        # 列出已安装的软件
scoop list <name>                 # 列出已安装某软件 已装版本
scoop update <name>               # 更新某软件
scoop update                   # 只更新 bucket，不升级软件
scoop update *                 # 升级所有已安装软件
scoop uninstall <name>            # 卸载
scoop which <exe>                 # 查看某可执行文件实际路径
scoop prefix <name>				  # 快速查询 通过 Scoop 安装 软件所在根目录路径
```
## 参考文章
我的文章内容主要摘抄自他们，感兴趣的朋友们可以看一下。

[Cnblogs的上善若水 | Win环境下的包管理工具,(包含winget,scoop)](https://www.cnblogs.com/jingzh/p/19122446#133-%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)

[少数派的FavourHong | windows上缺失的包管理器,包含winget,chocolatey,scoop](https://sspai.com/post/65933)

[知乎上的scoop,winget,chocolatey三者比较](https://zhuanlan.zhihu.com/p/694665774)

[知乎上的scoop设置国内镜像加速](https://zhuanlan.zhihu.com/p/27467105881)

[官方地址](https://scoop.sh)