---
title: "Git 使用规范：分支、提交信息与版本管理"
slug: git-conventions
aliases:
    - "/p/git-使用规范分支提交信息与版本管理/"
description: 从分支命名、Conventional Commits 提交信息到语义化版本与协作流程，整理一套可落地的 Git 使用规范，让提交历史清晰可追溯。
date: 2026-08-26T16:09:10+08:00
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
    - git
---

## 引言
git是非常舒适的版本管理器。广受使用且评价极高。规范的使用方法也尤为重要，提高协作效率，这就是写本篇文章的目的，整理git的常见规范。


## Git规范的核心部分

- 分支命名规范
- Commit提交信息规范
- 分支合并规范

### 分支命名规范
- master/main 主分支,生产环境代码
- develop 开发分支,最新的代码
- feature/* 新功能分支
- bugfix/* 或 fix/* 修补bug分支
- release/* 预发布分支
- hotfix/* 热修复分支

### 提交信息规范
这里推荐conventionalcommits规范
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```
#### type
- build: 用于修改项目构建系统，例如修改依赖库、外部接口或者升级 Node 版本等；
- chore: 用于对非业务性代码进行修改，例如修改构建流程或者工具配置等；
- ci: 用于修改持续集成流程，例如修改 Travis、Jenkins 等工作流配置；
- docs: 用于修改文档，例如修改 README 文件、API 文档等；
- style: 用于修改代码的样式，例如调整缩进、空格、空行等；
- refactor: 用于重构代码，例如修改代码结构、变量名、函数名等但不修改功能逻辑；
- perf: 用于优化性能，例如提升代码的性能、减少内存占用等；
- test: 用于修改测试用例，例如添加、删除、修改代码的测试用例等。
#### scope
表示修改的范围 e.g.feat(auth):
#### description
总结代码变动了什么，并且使用祈使句
#### body
记录具体的变动，修改原因，对比
#### footer
关联关联的需求或声明破坏性变更
破坏性变更主要是以`!`和`BREAKING CHANGE`
示例请去参考资料中的原文中查看

### 语义化版本
X.Y.Z
- X为主版本号,当你做了不兼容的 API 修改，
- Y为次版本号,当你做了向下兼容的功能性新增，
- Z为修订号,当你做了向下兼容的问题修正。

### 流程规范

#### 提交操作规范
- 原子化提交,一个commit只做一件事
- 避免合并污染,使用rebase而不是merge
- 合理Squash

#### 代码评估规范

- 背景/原因，为什么做这个改动
- 改动点，具体动了哪些核心模块。
- 复现，附上单元测试结果或前端截图/录屏。

#### 权限设计规范

- 保护核心分支,锁定main/master/develop分支
- 除去敏感信息与密钥
#### 仓库存储规范

- 10M+静态文件不得推送到git仓库,需将其托管到仓库外的静态资源服务器
- 分支需要在合并后及时清理

## 参考资料
[conventionalcommits规范原文](https://www.conventionalcommits.org/zh-hans/v1.0.0/)
[语义化版本原文](https://semver.org/lang/zh-CN/#%E6%91%98%E8%A6%81)