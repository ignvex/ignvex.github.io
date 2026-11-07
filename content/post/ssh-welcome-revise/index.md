---
title: "ssh欢迎信息修改"
slug: ssh-motd
aliases:
    - "/p/ssh欢迎信息修改/"
description: 两种自定义 SSH 登录欢迎信息的方法：直接修改 /etc/motd 静态文件，或用 Ubuntu/Debian 的 update-motd 动态脚本。
date: 2026-07-04T20:47:43+08:00
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
    - ssh
---

众所周知，每次通过SSH连接到服务器都会看到一段欢迎信息，这个欢迎信息是可以完全自定义的。常见的方式有两种：直接修改静态的 `/etc/motd` 文件，或者利用 Ubuntu/Debian 的 `update-motd` 动态脚本。

## 1. 修改静态欢迎信息 /etc/motd

```shell
sudo nano /etc/motd
```

写入你想显示的内容，比如：

```
Welcome to my server!
```

保存后重新登录，就能看到新内容了。

## 2. Ubuntu/Debian：动态MOTD

如果你用的是 Ubuntu/Debian，登录时会看到一堆系统提示（版本信息、可更新包数量、livepatch 等），这些不是来自 `/etc/motd`，而是来自 `/etc/update-motd.d/` 目录下的脚本，由 PAM 在登录时动态执行生成。

查看目录：

```shell
ls /etc/update-motd.d/
```

常见的脚本：

- `00-header`：显示系统版本信息
- `10-help-text`：显示帮助提示
- `90-updates-available`：显示可更新软件包数量

想让登录界面只剩你自己的内容，把脚本的可执行权限去掉即可：

```shell
sudo chmod -x /etc/update-motd.d/*
```

然后再按第1步修改 `/etc/motd`，重新登录就只会显示你写的内容了。

## 3. 用figlet生成ASCII艺术字

觉得纯文本太素？可以用 `figlet` 生成 ASCII 艺术字：

```shell
sudo apt install figlet
```

生成并写入 `/etc/motd`：

```shell
figlet -f slant "Welcome to my server" | sudo tee /etc/motd
```

`-f` 指定字体，可以换成 `big`、`standard` 等，重新登录看看效果。

## 4. 修改后没生效怎么办

如果改了 `/etc/motd` 登录却不显示，检查 sshd 配置：

```shell
sudo grep -i printmotd /etc/ssh/sshd_config
```

默认是 `PrintMotd yes`，如果是 `no`，改成 `yes` 后重启 sshd：

```shell
sudo systemctl restart sshd
```

## 5. 恢复默认

不想要自定义内容了，清空 `/etc/motd` 并恢复动态脚本权限即可：

```shell
sudo truncate -s 0 /etc/motd
sudo chmod +x /etc/update-motd.d/*
```
