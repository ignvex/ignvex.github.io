---
title: "hugo实用技巧"
description: 没错，不是衣服的那个雨果
date: 2026-08-14T22:44:27+08:00
image: 
math: 
license: 
comments: true
draft: false
build:
    list: always
categories:
    - 实用技巧
tags:
    - hugo
---


## 引言
用Hugo这个框架已经有了两个月，积累了一些常用的技巧，对于刚开始用的新人来说算是比较实用的。我认为官方文档写的不错的地方我会照搬，尊重文档 :)


## 步入正题

### hugo的预览

```shell
hugo server # 本地预览博客
hugo server -D # 预览本地博客，draft=true的文章会有显示
```

## hugo创建文章

```shell
hugo new post/***.md #纯文本博客
hugo new post/***/index.md #可以附上写静态资源的博客
```
## hugo文章内使用相对链接
这个直接照搬文档[官方文档](https://gohugo.io/shortcodes/relref/)