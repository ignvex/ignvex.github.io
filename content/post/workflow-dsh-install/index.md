---
title: "Deepseek Harness取巧-Windows专属"
description: 在懒和维护的便携性上寻找的一个平衡点
date: 2026-08-14T21:18:04+08:00
image: 
math: 
license: 
comments: true
draft: false
build:
    list: always    # Change to "never" to hide the page from the list
---

## 前言(吐槽)
`Deepseek Harness`不负众望的从内测被推进到的公测。采用与koishi的同源驱动cordis(是的没错，写q群bot可以进深度求索 (x )。npx的安装方式对于我这种不经常使用前端来说颇具挑战。`scoop install dsh`绝对是一个绝佳的选择，但很可惜，deeepseek harness并没有支持。那么就有了本文的慵懒操作。

## 准备清单
其实为了便于讲解我去写了一篇scoop的安装和便捷配置的文章。好了写完了。[scoop的安装和实用技巧]({{% relref "/post/recommand-opensource-software-scoop" %}})

### 阶段一,安装前置软件

```shell
scoop bucket add main
scoop install nodejs-lts
```
注: 我觉得你可能装过但是版本不是很新并不被支持，这里你可以直接使用`scoop update nodejs-lts`进行更新
### 阶段1.5,给npx换源
淘宝源其实不错，还有腾讯云、华为云镜像源可选。换掉地址即可。
```shell
npm config set registry https://registry.npmmirror.com 
```

### 阶段二,安装deepseek harenss
```shell
npx @deepseek-ai/dsh web
```
之后你就获得了开箱即用的deepseek harness了。

## 杂谈

自从openclaw的热门后腾讯对qq机器人就像是开智了一样，对的官方机器人放宽了，野生机器人的路封死了(规范运行上必不可少的一环)，推动了qq机器人的发展，早期的qq群机器人凭借正则表达式替换的服务也是能在群里安稳的用上了，是继数年前用野生框架时候那样的便捷。

deepseek harness与koishi同cordis驱动，其实为deepseek harness(cordis)做上协议适配器就可以当qq机器人跑。期待自建qq机器人在规范制度指引下在未来几年繁荣昌盛。

## 参考链接
[deepseek harness的github仓库](https://github.com/deepseek-ai/deepseek-harness)
[cordis的github仓库](https://github.com/cordiverse/cordis)