---
title: "Linux 基础工具代理配置（wget / curl / git）"
slug: linux-tools-proxy-config
aliases:
    - "/p/linux-基础工具代理配置wget-curl-git/"
description: 在 Linux 下为 wget、curl、git 配置代理，解决国内网络环境下命令行工具下载慢、连接失败的问题。
date: 2025-07-04T20:43:30+08:00
image: 
math: 
license: 
comments: true
draft: false
build:
    list: always    # Change to "never" to hide the page from the list
categories:
    - 实用技巧
tags:
    - linux
    - git
    - proxy
---


## Wget

目标路径 `/etc/wgetrc`

```
https_proxy = http://127.0.0.1:7890/
http_proxy = http://127.0.0.1:7890/
ftp_proxy = http://127.0.0.1:7890/
use_proxy = on

```

## Git

命令行执行 
```
git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy http://127.0.0.1:1080

```

取消代理
```
git config --global --unset http.proxy
git config --global --unset https.proxy

```

## Curl

目标路径 `~/.curlrc`
```
proxy="http://127.0.0.1:7890"
```