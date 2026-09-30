---
title: "工作环境搭建 序"
description: 下一代工作环境规划与部署
date: 2026-09-26T20:34:01+08:00
image:
math:
license:
categories:
  - 优化之路
tags:
  - Linux
  - Windows
comments: true
draft: false
build:
  list: always # Change to "never" to hide the page from the list
---

## 序言

笔者在最近没有经受住游戏的诱惑购买了一台新的笔记本。正好放下从高中时期在系统环境下处处埋雷的旧笔记本。到手的不到一个月里也走了不少的弯路，正值中秋，有着大块的时间对其稍作调整。

## 构思

`WSL2 + Ubuntu` 作为基础设施，wsl2提供虚拟化平台,ubuntu提供稳定可靠的系统支持。ubuntu是有对wsl做出定制优化的，出于对intel带有大小核设计的cpu兼容性选择对wsl合作更密切的ubuntu。

`WSL+`的环境搭建思路。由于我面向的最终生产环境是linux为主的系统，变相的减少了很多可能的兼容性问题。

## 安装

### 前置操作

启用WSL并将WSL2设为默认值

```shell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
wsl --set-default-version 2
```

### 查找&安装系统

`wsl -l -o`返回所有的可靠发行版(不包括社区维护版)
`wsl install`+发行版名称 进行安装

### 进入wsl

在powershell中输入wsl即可

## 优化设计

### wsl代理

```shell
host_ip=$(ip route | awk '/default/ {print $3}')
proxy_port=7890
export http_proxy="http://$host_ip:$proxy_port"
export https_proxy="http://$host_ip:$proxy_port"
export HTTP_PROXY="$http_proxy"
export HTTPS_PROXY="$https_proxy"
export ALL_PROXY="$http_proxy"
export all_proxy="$http_proxy"
```

### 换源

祖传秘技. tuna + 系统名 去找镜像源

## 参考文章

[(Hello CTF) WSL2 Environment](https://hello-ctf.com/hc-pwn/wsl2_environment/)