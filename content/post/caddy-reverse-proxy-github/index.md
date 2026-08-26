---
title: "Caddy反代github"
description: github 直连不稳定，用一台海外服务器 + Caddy 反向代理自建加速站，网页、clone、raw、release 一把梭，附完整 Caddyfile 与 git 配置。
date: 2026-08-15T13:07:30+08:00
image: 
math: 
license: 
comments: true
draft: false
build:
    list: always
categories:
    - 实用技巧
tags:
    - caddy
    - git
    - linux
---

github 直连不稳定，clone 大仓库动不动断流，release 下载经常超时。用一台海外服务器反代 github，网页、clone、raw、release 一把梭，一劳永逸。

caddy 的安装与守护进程写法在上一篇已经详细介绍过，这里不再赘述，[跳转链接]({{% relref "/post/caddy-reverse-proxy-docker" %}})。

## 快速搭建

### 配置caddyfile

```shell
# 换成你自己的域名
DOMAIN="example.com"

cat << EOF > /etc/caddy/Caddyfile
{
    email you@example.com
}

gh.\${DOMAIN} {
    reverse_proxy https://github.com {
        header_up Host github.com
        header_up origin https://github.com
        header_down Location "https://github.com" "https://gh.\${DOMAIN}"
    }
}

raw.gh.\${DOMAIN} {
    reverse_proxy https://raw.githubusercontent.com {
        header_up Host raw.githubusercontent.com
    }
}

objects.gh.\${DOMAIN} {
    reverse_proxy https://objects.githubusercontent.com {
        header_up Host objects.githubusercontent.com
        header_down Location "https://objects.githubusercontent.com" "https://objects.gh.\${DOMAIN}"
    }
}

codeload.gh.\${DOMAIN} {
    reverse_proxy https://codeload.github.com {
        header_up Host codeload.github.com
    }
}

api.gh.\${DOMAIN} {
    reverse_proxy https://api.github.com {
        header_up Host api.github.com
    }
}

gist.gh.\${DOMAIN} {
    reverse_proxy https://gist.github.com {
        header_up Host gist.github.com
    }
}

avatars.gh.\${DOMAIN} {
    reverse_proxy https://avatars.githubusercontent.com {
        header_up Host avatars.githubusercontent.com
    }
}
EOF
```

注意事项：

- `email` 换成自己的邮箱
- 证书不用管，Caddy 自动申请续期，配置里不用写任何 `tls`
- `header_down` 两个参数是删除、三个参数才是替换，且查找串要写上游实际域名（`https://github.com` 这种），写成"期望结果"不会生效
- release 下载会 302 跳到 `objects.githubusercontent.com`，所以必须反代它并重写 `Location`，否则下载还是走原站
- 页面里的 css/js 在 `github.githubassets.com`、头像在 `avatars.githubusercontent.com`，这些静态资源仍然直连；如果它们也慢，把对应域名也加进反代
- 登录受 cookie 域名限制，反代域名登录不上就直连原站登录，浏览不受影响
- 配置里出现的域名必须都能解析，否则 Caddy 会一直重试签证书

### 配置dns

7 条 A 记录指向服务器公网 IP：

```
gh.example.com
raw.gh.example.com
objects.gh.example.com
codeload.gh.example.com
api.gh.example.com
gist.gh.example.com
avatars.gh.example.com
```

嫌麻烦就加一条泛解析 `*.gh.example.com`。

### 验证部署

```shell
sudo caddy validate --config /etc/caddy/Caddyfile
sudo systemctl reload caddy
curl -sI https://gh.example.com/
```

返回 `200 OK` 说明反代正常。再试一次 clone：

```shell
git clone https://gh.example.com/octocat/Hello-World.git
```

release 下载验证（换成真实的 release 链接）：

```shell
curl -sIL https://gh.example.com/user/repo/releases/download/v1.0/file.zip
```

响应里的 `Location` 应该是 `https://objects.gh.example.com/...`，说明重写生效。

## 快速使用

### git

```shell
git config --global url."https://gh.example.com/".insteadOf "https://github.com/"
```

一劳永逸，记得换掉 example.com。之后 clone、pull 自动走反代，remote 地址不用改：

```shell
git clone https://github.com/user/repo.git   # 实际请求自动改写为 https://gh.example.com/user/repo.git
```

查看改写规则是否生效：

```shell
git config --get-regexp '^url\.'
```

### 网页

直接访问 `https://gh.example.com/`，浏览仓库、代码、issue 都没问题。需要登录的操作（发 issue、提 PR）建议直连原站。

### raw与release

```shell
curl -sSL https://raw.gh.example.com/user/repo/main/install.sh
wget https://gh.example.com/user/repo/releases/download/v1.0/app.tar.gz
```

脚本安装场景很实用，把 `curl ... | bash` 里的地址换成反代域名即可。


### 稍作补充

用户目录下的 `.gitconfig` 通过 `insteadOf` 将 `git@github.com:` 开头的地址换为反代地址（见下方示例），防止 VSCode 将你识别为企业版 github 用户。

示例:
```shell
[url "https://example.com/"]
	insteadOf = git@github.com:
```
