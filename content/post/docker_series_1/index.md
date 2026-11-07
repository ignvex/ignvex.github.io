---
title: "Docker 使用指南：镜像、容器与 Dockerfile"
slug: docker-guide
aliases:
    - "/p/docker-使用指南镜像容器与-dockerfile/"
description: Docker 基础入门：镜像管理、容器操作与 Dockerfile 写法，覆盖常用命令与使用注意事项，帮你快速上手容器化。
date: 2026-08-14T21:06:32+08:00
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
    - docker
    - linux
---


## 前言
docker是一个轻量化且开源的容器引擎。借由Go驱动有着极低性能开销。不过仅支持linux发行版的系统

## 安装
[金枪鱼镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/)

DOCKER的安装教程非常的多，再加上国外主机的访问速度奇差，这里建议直接用知名金枪鱼镜像站的文档去安装,节约更多的时间在搜索教程和镜像站上。

## 使用方法

### 镜像管理
```shell
docker image ls  # 列出本地所有镜像
docker image rm {IMAGE_NAME} # 删除本地的某个镜像 
docker build # 编译docker镜像
```
#### docker build
- -t为编译的镜像
- -f为指定dockerfile路径
- -no-cache 为不使用缓存构建镜像
- `.`当前目录及其子目录被当作上下文
#### 进阶操作
使用`.dockerignore`排除不必要的文件，减少镜像体积
### 容器操作
```shell
docker run # 创建并启动某个容器 
docker start {IMAGE_NAME} # 启动
docker stop {IMAGE_NAME} # 停止
docker restart {IMAGE_NAME} #重启
docker attach {IMAGE_NAME} # 在终端中直接进入容器内 (不推荐)
docker exec # 在终端中直接进入容器内 (推荐)
docker ps #查看所有容器
```
#### 容器操作补充
- docker restart 可以在docker容器没有运行的情况下直接启动，如果在运行就是重启
- attach 会随着你输入`ctrl c`或`exit`的同时关闭容器
- exec是你要访问的同一容器的新进程，需要查看你跑服务的进程请使用docker logs
#### docker run
- -d 在后台运行
- --name指定容器名称
- -p 容器内端口映射到容器外
- -v 主机路径映射到容器内
- -it 打开交互式终端
#### docker exec
- -i 允许向容器发送内容(交互)
- -t 分配一个伪终端
- -it `-i`和`it`的组合(常用)
- -d 在后台运行
- -u 指定用户
- -e 设置环境变量
- -w 指定容器内工作目录
- --privileged 最高权限模式。等同于直接操作物理机(用之前先确定自己为什么要这么做)
#### docker ps
- ps -a 查看所有容器
- ps -q 只查看容器id
- ps -s 显示容器大小
- ps -n 显示最后创建的
- ps -f 根据条件过滤容器(ps -f status=exited)
## Dockerfile 的写法
### 基础内容
```dockerfile
FROM {IMAGE_NAME}
RUN {CUSTOM}
```

## 参考资料
[菜鸟教程](https://www.runoob.com/docker/docker-container-usage.html)
[廖雪峰的教程](https://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)