---
title: "便捷安装系统证书的 Magisk 模块"
slug: magisk-move-certificate
aliases:
    - "/p/便捷安装证书的magisk模块/"
description: 用 Move Certificate 模块把抓包证书安装为系统证书，Magisk / KernelSU 通用，解决 reqable 等工具在 Android 上的证书信任问题。
date: 2026-07-04T22:28:43+08:00
image: 
math: 
license: 
comments: true
draft: false
build:
    list: always    # Change to "never" to hide the page from the list
categories:
    - 玩机日志
tags:
    - android
    - magisk
    - kernelsu
---

直到今日才把以前写的文章搬到博客上来确实有些晚了，不过magisk和kernelsu是通用的，换到ksu也能用，这篇文章还是值得发的


由于 reqable 客户端提供的安装证书模块未能使抓包正常工作，作者放弃使用 reqable 提供的模块并选择了另一优秀的 Magisk 模块[Move Certificates](https://github.com/ys1231/MoveCertificate)

# 使用方法

由于文档的介绍简单明了，这里只进行简要概括。

1. 下载模块
2. 刷入模块
3. 将证书放入 `/data/local/tmp/cert/`
4. 重启，使模块生效

注: 如果 reqable 用户选择保存证书，证书会自动保存到`/sdcard/download/reqable/`

参考资料:
https://github.com/ys1231/MoveCertificate
https://reqable.com/zh-CN/docs/getting-started/installation/
