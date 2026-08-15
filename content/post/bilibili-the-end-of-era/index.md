---
title: "大人，时代变了（Bilibili 版）"
slug: bilibili-the-end-of-era
description: 2026 年，Bilibili 逆向接口仓库与 BiliRoaming 等第三方项目接连收到律师函，回顾这场三方生态的终结与背后的商业化逻辑。
date: 2026-07-06T18:20:22+08:00
image: 
math: 
license: 
comments: true
draft: false
categories:
    - 杂谈
tags:
    - bilibili
build:
    list: always    # Change to "never" to hide the page from the list
---

## 引言

看着`bilibili`的三方生态在2026年这一年里一步步走进历史，总觉得应该记点什么。以杂谈的形式，将事情的起因、经过和我的想法稍作整理，写了这样一篇尽量真实、客观的文章

## 起因

github上逆向`bilibili`平台的仓库，以及存在多年的`BiliRoaming`（解决区域限制、海外用户看番的刚需），都遭到了律师函。在此之前，与bilibili合作过的律所已经起诉过多个规模较小、但不是源头、被人跳脸到官方的第三方`bilibili`app仓库。而频率最高的就是2026年——这一年，律师函落到了许多仓库头上。

## 结果

结果并没有值得说的。每一个开源项目的管理者与律师函的斡旋，不过都是先稍作配合，然后在律师函一步又一步的要求下，彻底移除侵权内容。毕竟律师函的下一步就是起诉，理不在开源项目这边，所以只能配合。

## 略作反思

事情到此告一段落，剩下的是几个值得想想的问题。

早年的bilibili对于api被逆向的问题是睁一只眼闭一只眼，只会律师函一些被端到官方群/评论区下的仓库。而`api-collect`、`BiliRoaming`这种大仓库，只会派人向仓库管理者问一下和黑产有没有关联（如果是黑产的话还会开源？）。

不难看出，b站的商业化发展到今天，对影音作品版权的处理上充满弹性，又急需广告业务（近几年广告内容多了不少，投入广告的入口也大幅增加）。这些绕过广告的服务，只会让b站对三方生态的容忍度每况愈下。

大人，时代变了。

## 涉事仓库

- [Bilibili-api-collect](https://github.com/socialsisteryi/bilibili-api-collect) —— [已归档]
- [Bili漫游](https://github.com/yujincheng08/BiliRoaming) —— [已归档]

## 写在最后

这些项目里的代码不会消失——fork还在，镜像还在。但「在github上光明正大地维护一个bilibili逆向项目」这件事，大概真的要成为历史了。

以后的人可能很难理解，曾经有一个时期，看视频是可以不看广告、不锁区的。