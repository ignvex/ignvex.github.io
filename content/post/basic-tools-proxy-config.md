---
title: "基础工具的代理配置"
description: 基础却重要
date: 2025-07-04T20:43:30+08:00
image: 
math: 
license: 
comments: true
draft: false
categories:
    - 实用技巧
tags: 
    - linux
    - wget
    - curl 
    - git
build:
    list: always    # Change to "never" to hide the page from the list
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
git config --global https.proxy http://127.0.0.1:1080
git config --global https.proxy https://127.0.0.1:1080

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