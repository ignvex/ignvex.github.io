---
title: "自用docker镜像源搭建"
description: 没错，是自建
date: 2026-08-14T23:09:30+08:00
image: 
math: 
license: 
comments: true
draft: false
categories:
    - 实用技巧
tags:
    - caddy
    - docker
    - linux
build:
    list: always
---

公共 docker 加速器关了一大批，自己用一台海外服务器反代镜像仓库，一劳永逸。

## 快速搭建

### 安装caddy

二进制放 `/usr/bin/caddy`，服务文件 `/etc/systemd/system/caddy.service`，配置 `/etc/caddy/Caddyfile`
```shell
sudo mv caddy /usr/bin/caddy
sudo chmod 755 /usr/bin/caddy
sudo setcap cap_net_bind_service=+ep /usr/bin/caddy

sudo groupadd --system caddy
sudo useradd --system \
    --gid caddy \
    --create-home \
    --home-dir /var/lib/caddy \
    --shell /usr/sbin/nologin \
    --comment "Caddy web server" \
    caddy
```

### 配置caddy守护进程

`/etc/systemd/system/caddy.service`：

```ini
[Unit]
Description=Caddy
Documentation=https://caddyserver.com/docs/
After=network.target network-online.target
Requires=network-online.target

[Service]
Type=notify
User=caddy
Group=caddy
ExecStart=/usr/bin/caddy run --environ --config /etc/caddy/Caddyfile
ExecReload=/usr/bin/caddy reload --config /etc/caddy/Caddyfile --force
TimeoutStopSec=5s
LimitNOFILE=1048576
LimitNPROC=512
PrivateTmp=true
ProtectSystem=full
AmbientCapabilities=cap_net_bind_service

[Install]
WantedBy=multi-user.target
```

```shell
sudo systemctl daemon-reload
sudo systemctl enable --now caddy
```

### 配置caddyfile

```shell
# 换成你自己的域名
DOMAIN="example.com"

cat << EOF > /etc/caddy/Caddyfile
{
    email you@example.com
}

hub.docker.\${DOMAIN} {
    reverse_proxy https://registry-1.docker.io {
        header_up Host registry-1.docker.io
        header_down Www-Authenticate "https://auth.docker.io" "https://auth.docker.\${DOMAIN}"
        header_down Location "https://production.cloudflare.docker.com" "https://production.cloudflare.docker.\${DOMAIN}"
    }
}

auth.docker.\${DOMAIN} {
    reverse_proxy https://auth.docker.io {
        header_up Host auth.docker.io
    }
}

production.cloudflare.docker.\${DOMAIN} {
    reverse_proxy https://production.cloudflare.docker.com {
        header_up Host production.cloudflare.docker.com
    }
}

hub.quay.\${DOMAIN} {
    reverse_proxy https://quay.io {
        header_up Host quay.io
        header_down Www-Authenticate "https://quay.io/v2/auth" "https://hub.quay.\${DOMAIN}/v2/auth"
    }
}

hub.gcr.\${DOMAIN} {
    reverse_proxy https://gcr.io {
        header_up Host gcr.io
        header_down Www-Authenticate "https://gcr.io/v2/token" "https://hub.gcr.\${DOMAIN}/v2/token"
    }
}

hub.k8s.\${DOMAIN} {
    reverse_proxy https://registry.k8s.io {
        header_up Host registry.k8s.io
        header_down Www-Authenticate "https://registry.k8s.io/v2/token" "https://hub.k8s.\${DOMAIN}/v2/token"
        header_down Location "https://storage.googleapis.com" "https://storage.googleapis.\${DOMAIN}"
    }
}

storage.googleapis.\${DOMAIN} {
    reverse_proxy https://storage.googleapis.com {
        header_up Host storage.googleapis.com
    }
}
EOF
```

注意事项：

- `email` 换成自己的邮箱
- 证书不用管，Caddy 自动申请续期，配置里不用写任何 `tls`
- `k8s.gcr.io` 已退役，k8s 站点反代的是新的 `registry.k8s.io`
- `header_down` 两个参数是删除、三个参数才是替换，且查找串要写上游实际域名（`https://auth.docker.io` 这种），写成"期望结果"不会生效

### 配置dns

7 条 A 记录指向服务器公网 IP：

```
hub.docker.example.com
auth.docker.example.com
production.cloudflare.docker.example.com
hub.quay.example.com
hub.gcr.example.com
hub.k8s.example.com
storage.googleapis.example.com
```

嫌麻烦就加一条泛解析 `*.docker.example.com`。配置里出现的域名必须都能解析，否则 Caddy 会一直重试签证书。

### 验证部署

```shell
sudo caddy validate --config /etc/caddy/Caddyfile
sudo systemctl reload caddy
curl -sI https://hub.docker.example.com/v2/
```

返回 `401 Unauthorized` 是正常的，同时响应头里 `Www-Authenticate` 的 realm 应该是 `https://auth.docker.example.com/token`，说明重写生效。最终测试：

```shell
docker pull hub.docker.example.com/library/hello-world:latest
```

## 快速使用

### unraid

Settings → Docker → Advanced View → Docker Hub URL 改成 `https://hub.docker.example.com`，或者拉镜像时直接写完整地址 `hub.docker.example.com/linuxserver/plex`。

### 其他主流linux

`/etc/docker/daemon.json`：

```json
{
  "registry-mirrors": ["https://hub.docker.example.com"]
}
```

```shell
sudo systemctl restart docker
```

之后 `docker pull nginx` 自动走代理。registry-mirrors 只接管 Docker Hub，gcr/quay 镜像换前缀拉：

```shell
docker pull hub.quay.example.com/prometheus/prometheus:latest
docker pull hub.gcr.example.com/istio/proxyv2:1.20.0
```

可以加公共源兜底：

```json
{
  "registry-mirrors": [
    "https://hub.docker.example.com",
    "https://mirrors.tuna.tsinghua.edu.cn"
  ]
}
```
