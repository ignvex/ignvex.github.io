---
title: "yazi-优秀的支持多系统的文件管理器"
description: 基于 Rust 的终端文件管理器 Yazi：安装与依赖清单，覆盖 Windows（Scoop/WinGet）、Debian、Arch 等平台。
date: 2026-07-04T20:48:39+08:00
image: image.png
math: 
license: 
comments: true
draft: false
categories:
    - 优秀软件
tags:
    - linux
    - windows
    - file manager
build:
    list: always    # Change to "never" to hide the page from the list
---

yazi 是基于rust编写的文件管理器，响应速度非常优秀，使用体验非常好，功能强大,[项目地址](https://github.com/sxyazi/yazi),[项目文档](https://yazi-rs.github.io)

## 快速安装方法

### Windows
#### Scoop
```
scoop install yazi
scoop install ffmpeg 7zip jq poppler fd ripgrep fzf zoxide imagemagick
```
#### WinGet
```
winget install sxyazi.yazi
winget install Gyan.FFmpeg 7zip.7zip jqlang.jq sharkdp.fd BurntSushi.ripgrep.MSVC junegunn.fzf ajeetdsouza.zoxide ImageMagick.ImageMagick
```
### Debian & Ubuntu & Deepin ...
```
sudo apt install ffmpeg 7zip jq poppler-utils fd-find ripgrep fzf zoxide imagemagick
```
### Manjaro & arch ...
```
sudo pacman -S yazi ffmpeg p7zip jq poppler fd ripgrep fzf zoxide imagemagick
```
### 使用
输入yazi唤起