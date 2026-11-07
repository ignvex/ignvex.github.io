---
title: "Windows 11优化技巧集"
slug: windows-11-tips
aliases:
    - "/p/windows-11优化技巧集/"
description: 新笔记本换到 Windows 11 后的优化记录：任务栏精简、动画调整，以及 PowerShell 7、PowerToys、7zip-zstd 等软件推荐。
date: 2026-09-30T20:09:02+08:00
image:
math:
license:
comments: true
draft: false
build:
  list: always # Change to "never" to hide the page from the list
categories:
    - 优化之路
tags:
    - windows
    - wsl
---

## 序言之前

正值国庆与中秋相逢，祝大家节日快乐。与国同庆，与家同圆。

## 序言

笔者在9月没有忍住的换了一台全新的笔记本。为了避免上次的amd cpu + amd gpu的灾难戒网瘾体验，自觉地避开了AMD。吃上了大小核心设计的Intel cpu。好处是会有较好的续航，坏处是真的和win10说再见了(win10钉子户落泪)。正因如此有了此篇文章。

## 优化方向

### 系统优化

#### 任务栏移除任务视图&搜索框

将任务视图和搜索框隐藏。需要任务视图直接按`win`+ `tab`，需要搜索直接`win`+ 你要搜的内容。不过搜索会被接下来的快速访问取代的。

#### 自动隐藏任务栏

为了oled屏幕的笔记本做出的妥协。oled显示器有保修但是笔记本没有3年保修怪一言难尽的。

#### wsl 篇

我是有一个wsl系列文章的。感兴趣的朋友可以去看看，这里就不再赘述。 [跳转链接]({{% relref "/post/workspace-env-build-basic" %}})

#### 动画时长调整

windows 11的动画给我一种很难受的感觉。400ms的时长让我难以忍受，我认为1/5的时长刚刚好，力推`80`ms的动画时长。
注册表路径`计算机\HKEY_CURRENT_USER\Control Panel\Desktop`中的`MenuShowDelay`

### 软件优化

#### powershell7

powershell对我来说是一个很纠结的一个点。算系统优化?还是算软件优化。但基于poewrshell5与7的不可替代性我将其归类为软件优化。

powershell7 采用了兼容性更好的 utf-8编码(\o/),虽然支持了mac 和 linux意义不明，但使用体验得到了极大的提高。

主要优点如下:

- 运算符增加
- 并行处理支持
- 报错更直观

#### powertoys

包含了很多使用且好用的功能。其实这个适合给一个3级标题详细说。不过很多我在笔记本用不上旧不多讲我没用过的功能了

##### 命令面板

快捷指令:
`win`+`alt`+`space`
可以搜并打开应用，做计算，运行命令，打开终端，搜东西(??之后输入要搜索的内容并在你的默认浏览器打开) 等等

#### Light Switch

为并不完善的windows 深色主题开关增加随日落日出开关的功能。非常实用但是windows的应用和网页对深色主题的支持并不好，用久了应该是会关掉的。

#### 快捷键指南

`win`+`shift`+`up` 查看所有的快捷键

#### powerdisplay

`win`+`ctrl`+`shitft`+ `P`

#### cfosspeed

以前还是怪便宜的。40多，现在的价格涨了特别多，不推荐了。

#### 7zip-zstd

zstd算法的支持提高了极大的压缩比和压缩速度，较为推荐。

#### 卡巴斯基

推荐订阅软件我也是纠结了一会，不过卡巴斯基优选版订阅的组件我很推荐。

- 多端(包括ios/android/win/mac不限制设备数量)的密码管理器同步
- 弹窗拦截
- 文件销毁(文件所在物理位置覆写)
- 网页去广告(特别好用)
