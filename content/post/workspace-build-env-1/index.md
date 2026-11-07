---
title: "工作环境搭建（一）：WSL 基础配置"
slug: workspace-env-build-1
description: 工作环境搭建系列第一篇：配置 .wslconfig 与 wsl.conf、APT 换源、按需开关的代理，以及 Node、Python、Go 工具链的安装。
date: 2026-11-07T20:00:00+08:00
image:
math:
license:
comments: true
draft: true
categories:
    - 优化之路
tags:
    - windows
    - wsl
    - linux
    - proxy
---

## 前言

[序篇]({{% relref "/post/workspace-env-build-basic" %}})只做了最基本的事：把 WSL2 和 Ubuntu 装起来，再顺手配了代理和换源。用了一个多月，踩到的坑基本都和"默认配置"有关：内存被吃满、DNS 时灵时不灵、代理一开就关不掉。这一篇把这些补上，再把日常要用的语言工具链装好。

本文环境：Windows 11，WSL 2.7，Ubuntu 26.04 LTS。

## Windows 侧：.wslconfig

`.wslconfig` 控制的是 WSL2 虚拟机本身，对所有发行版生效。文件放在 `%USERPROFILE%\.wslconfig`，不存在就新建一个。

```ini
[wsl2]
memory=12GB
swap=8GB
dnsTunneling=true
autoProxy=false

[experimental]
autoMemoryReclaim=gradual
sparseVhd=true
```

逐项说明：

- `memory`：WSL2 默认可以用掉主机一半的内存，跑几个容器加编译时 Windows 那边会明显变卡。按自己的内存大小给一个上限，我的机器给 12GB。
- `swap`：交换空间大小，内存上限压低之后适当加一点，避免编译时直接 OOM。
- `dnsTunneling`：让 WSL 的 DNS 请求经 Windows 解析。开了代理软件或连公司 VPN 时，DNS 解析失败的问题基本靠它解决。
- `autoProxy`：是否自动继承 Windows 的系统代理。我选择关掉，改为在 shell 里手动控制，原因见下文代理一节。
- `autoMemoryReclaim=gradual`：WSL 空闲时把缓存占用的内存慢慢还给 Windows。默认不回收，任务管理器里 `VmmemWSL` 常年占着好几个 G。
- `sparseVhd=true`：新建的虚拟磁盘使用稀疏文件，Linux 里删掉的文件空间可以还给 Windows，不会只涨不缩。

改完后在 PowerShell 里执行 `wsl --shutdown`，再重新进入 WSL 才会生效。

## Linux 侧：/etc/wsl.conf

`/etc/wsl.conf` 只对当前发行版生效，用来控制发行版内部的行为。

```ini
[boot]
systemd=true

[user]
default=ignvex
```

- `systemd=true`：启用 systemd。Docker、各种 `systemctl` 管理的服务都依赖它。新版 Ubuntu 默认已经开启，检查一下即可。
- `default`：进入 WSL 时默认登录的用户，填你自己的用户名。

同样需要 `wsl --shutdown` 后重启生效，可以用 `systemctl is-system-running` 确认 systemd 已经接管。

## APT 换源

序篇里一句"tuna + 系统名"带过了，这里补完整。Ubuntu 从 24.04 开始改用 deb822 格式，源配置在 `/etc/apt/sources.list.d/ubuntu.sources`，旧的 `/etc/apt/sources.list` 已经是空的。

先备份，再整个替换（26.04 的代号是 `resolute`，其他版本自行替换）：

```shell
sudo cp /etc/apt/sources.list.d/ubuntu.sources /etc/apt/sources.list.d/ubuntu.sources.bak
sudo tee /etc/apt/sources.list.d/ubuntu.sources > /dev/null <<'EOF'
Types: deb
URIs: https://mirrors.tuna.tsinghua.edu.cn/ubuntu/
Suites: resolute resolute-updates resolute-backports
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

Types: deb
URIs: https://mirrors.tuna.tsinghua.edu.cn/ubuntu/
Suites: resolute-security
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
EOF
sudo apt update && sudo apt upgrade -y
```

注意是**替换**而不是在文件末尾追加。追加的话官方源还在，`apt update` 会两边都拉一遍，速度没有任何提升。`proposed` 仓库是预发布更新，日常使用不要加。

## 代理：按需开关

序篇的写法是在 `~/.bashrc` 里直接 `export`，问题是代理软件一关，WSL 里所有联网命令都会报错，只能手动 `unset`。改成两个函数，需要时再开：

```shell
# ~/.bashrc
proxy_on() {
    local host_ip
    host_ip=$(ip route | awk '/default/ {print $3}')
    local proxy="http://${host_ip}:${1:-7890}"
    export http_proxy="$proxy" https_proxy="$proxy" all_proxy="$proxy"
    export HTTP_PROXY="$proxy" HTTPS_PROXY="$proxy" ALL_PROXY="$proxy"
    export no_proxy="localhost,127.0.0.1,::1" NO_PROXY="localhost,127.0.0.1,::1"
    echo "proxy on: $proxy"
}

proxy_off() {
    unset http_proxy https_proxy all_proxy HTTP_PROXY HTTPS_PROXY ALL_PROXY no_proxy NO_PROXY
    echo "proxy off"
}
```

用法：`proxy_on` 使用默认端口 7890，`proxy_on 7897` 指定端口，`proxy_off` 关闭。想默认开启的话，在 `.bashrc` 末尾加一行 `proxy_on > /dev/null`。

几个要点：

- WSL2 默认是 NAT 网络，Windows 主机在 WSL 里的地址就是默认网关，所以用 `ip route` 取。这个地址每次重启都可能变，不要写死。
- 代理软件要打开"允许局域网连接"（Clash 系里叫 Allow LAN），否则 WSL 连不上 Windows 的代理端口。
- 一定要设置 `no_proxy`。不设的话，本地起的开发服务器（比如 `hugo server`）用 curl 访问也会被转发到代理，返回莫名其妙的 502。

## 开发工具链

### Node.js：nvm

Ubuntu 仓库里的 Node 版本偏旧，用 nvm 管理多版本：

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.8/install.sh | bash
source ~/.bashrc
nvm install --lts
```

npm 换成国内镜像：

```shell
npm config set registry https://registry.npmmirror.com/
```

pnpm 用官方脚本安装，脚本会在 `.bashrc` 里写入 `PNPM_HOME`：

```shell
curl -fsSL https://get.pnpm.io/install.sh | sh -
```

### Python：uv

uv 同时管理 Python 版本、虚拟环境和依赖，速度比 pip 快很多，装上之后基本不需要再碰系统自带的 Python。

```shell
curl -LsSf https://astral.sh/uv/install.sh | sh
uv python install
```

安装脚本会生成 `~/.local/bin/env` 并在 `.bashrc` 里 source 它，把 `~/.local/bin` 加进 `PATH`。

### Go：官方二进制

到 [go.dev/dl](https://go.dev/dl/) 下载最新的 linux-amd64 压缩包，解压到 `/usr/local`：

```shell
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go*.linux-amd64.tar.gz
```

然后在 `~/.bashrc` 里加入 `PATH`：

```shell
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
```

新版 Go 不需要手动设置 `GOROOT`，`GOPATH` 默认就是 `~/go`，这两个变量都可以不写。不开代理的话，可以把模块代理换成国内的 `go env -w GOPROXY=https://goproxy.cn,direct`。

## 检查

全部配置完后，`wsl --shutdown` 重启一次，进来逐项确认：

```shell
systemctl is-system-running   # running 或 degraded 都说明 systemd 已启用
free -h                       # 总内存应等于 .wslconfig 里的 memory
apt policy | grep -c archive.ubuntu  # 应为 0，只剩镜像源
proxy_on && curl -sI https://www.google.com | head -1
node -v && pnpm -v && uv --version && go version
```

## 下一篇

基础环境到这里就搭好了。下一篇打算写 Docker：在 WSL 里直接装 Docker Engine，不用 Docker Desktop，以及镜像加速怎么配。

## 参考文章

- [WSL 中的高级设置配置（Microsoft Learn）](https://learn.microsoft.com/zh-cn/windows/wsl/wsl-config)
- [Ubuntu 软件仓库镜像使用帮助（清华大学开源软件镜像站）](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)
- [(Hello CTF) WSL2 Environment](https://hello-ctf.com/hc-pwn/wsl2_environment/)
