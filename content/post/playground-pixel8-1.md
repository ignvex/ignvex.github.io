---
title: "Pixel8玩机日志"
description: 
date: 2025-04-04T10:59:44+08:00
image: 
math: 
license: 
comments: true
draft: false
categoris:
    - 玩机日志
tags:
    - pixel 8
    - magisk
    - kernelsu
build:
    list: always    # Change to "never" to hide the page from the list
---

# Pixel 8 玩机日志

前情提要，玩机前需要备份重要数据，数据无价。

## 1.获得权限

打开设置 -> 连按5次系统版本号 -> 解锁开发者模式 -> 进入开发者选项 -> 打开adb 调试

## 2. 安装magisk & 修补`init_boot`

在`github`的release中下载最新的`app-relase.apk`，修补准备好的init_boot.img

注: init_boot.img 可从官方线刷脚本获得
## 2. 进入fastboot & 解oem锁(bl锁)
```shell 
adb reboot fastboot 
```
重启后进入fastboot后
```shell 
fastboot oem unlock
```
警告: 打开oem锁后会清除所有用户数据,解锁需要先退有关账号
注: 如需回锁 `fastboot oem lock`
## 3. 刷入修补镜像 
```shell 
fastboot flash init_boot ***.img
```
## 4. 配置软件

不在着重讲述，各取所需