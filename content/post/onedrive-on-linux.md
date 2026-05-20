---
title: "在linux上使用onedrive"
description: 
date: 2026-07-04T20:39:34+08:00
image: 
math: 
license: 
comments: true
draft: false
categories:
    - 实用技巧
tags:
    - linux
    - debian 
    - onedrive
build:
    list: always    # Change to "never" to hide the page from the list
---

ondrive官方并没有提供linux平台的支持。但在github上有一个可以使用onedrive的[仓库](https://github.com/abraunegg/onedrive)。配置起来相对简单，还能让我的microsotf365发挥他的价值。

## 1. 安装
```shell
apt install onedrive
```
## 2. 登录
```shell
onedrive
```
根据提示进行操作
## 3. 配置onedrive
根据[文档](https://github.com/abraunegg/onedrive/blob/master/docs/usage.md)进一步的配置

文档中的部分内容
获取配置文件
```shell
wget https://raw.githubusercontent.com/abraunegg/onedrive/master/config -O ~/.config/onedrive/config

```
非文档内容
systemd
```
[Unit]
Description=OneDrive Client for Linux
After=network.target NetworkManager.service systemd-networkd.service iwd.service

[Service]
Type=simple
LimitNPROC=500
LimitNOFILE=1000000
Restart=always
ExecStartPre=/usr/bin/sleep 1s

ExecStart=/usr/local/bin/onedrive --sync

[Install]
WantedBy=multi-user.target
```
需要代理可以增加以下内容
```
Environment="HTTP_PROXY=http://your.proxy.server:port"
Environment="HTTPS_PROXY=http://your.proxy.server:port"
```
