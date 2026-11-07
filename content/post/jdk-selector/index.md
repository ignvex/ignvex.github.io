---
title: "SDKMAN!：Java 版本管理器"
slug: sdkman
aliases:
    - "/p/sdkmanjava-版本管理器/"
description: SDKMAN! 安装与使用：一条命令切换 JDK 多版本，覆盖 Windows（Scoop）、Debian、Arch 等平台。
date: 2026-07-05T23:44:18+08:00
image: background.svg
math: 
license: 
comments: true
draft: false
build:
    list: always    # Change to "never" to hide the page from the list
categories:
    - 开发工具
tags:
    - java
    - linux
    - windows
---


SDKMAN! 是一款轻量级的软件开发工具包管理器，专为 Java、Kotlin、Gradle、Maven 等 JVM 生态工具设计。基于 Shell 脚本实现，安装零依赖，响应飞快，不侵入系统环境。无论是频繁切换 JDK 版本，还是尝鲜最新构建工具，一行命令即可搞定。

[项目地址](https://sdkman.io/) | [使用文档](https://sdkman.io/usage)

## 快速安装

### Windows

Scoop
```shell
scoop install sdkman
source "$HOME/.sdkman/bin/sdkman-init.sh"
```

### Debian / Ubuntu / Deepin / macOS / Manjaro / Arch
```shell
sudo apt install curl -y
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
```

### Manjaro / Arch
```shell
yay -S sdkman
```

## 使用

打开终端，输入 `sdk` 唤起。以下是常用指令


```
查看可用的 SDK 版本
sdk list java          # 所有可安装的 Java 发行版及版本
sdk list gradle        # 查看 Gradle 可用版本

安装指定版本
sdk install java 17.0.10-tem    # 安装 Eclipse Temurin 的 JDK 17
sdk install maven 3.9.6         # 安装 Maven 3.9.6
sdk install kotlin              # 不指定版本则安装最新稳定版

版本切换
sdk use java 11.0.22-tem       # 当前终端临时切换为 JDK 11
sdk default java 21.0.2-tem    # 设置 JDK 21 为新终端默认版本

查看当前版本与已安装列表
sdk current java               # 当前使用的 Java 版本
sdk current                    # 查看所有被管理的 SDK 当前版本

卸载不需要的版本
sdk uninstall gradle 7.2
```

## 支持的部分 SDK 列表
Java (Adoptium, Corretto, GraalVM, Zulu…) · Kotlin · Groovy · Scala  
Gradle · Maven · sbt · Ant · Spring Boot CLI · Micronaut · Quarkus · VisualVM  
MongoDB Shell · Hadoop · Spark · 更多可通过 `sdk list` 查找