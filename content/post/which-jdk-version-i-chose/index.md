---
title: "Java 发行版选择指南"
slug: java-distribution-guide
aliases:
    - "/p/java发行版选择指南/"
description: JVM / JRE / JDK 概念梳理与 Java 发行版选择指南：LTS 支持周期、Minecraft 各版本对 Java 的要求，帮你选对发行版。
date: 2026-07-05T23:02:10+08:00
image: background.png
math: 
license: 
comments: true
draft: false
build:
    list: always    # Change to "never" to hide the page from the list
categories:
    - 实用技巧
    - 游戏
tags:
    - java
    - minecraft
---

java是非常优秀的编程语言，是21世纪极为优秀的语言。但java在2006年将本体与hotspot开源并形成openjdk后，java发行版的选择也不只官方独有，可选择的内容多了，但如何选择适合自己需求的发行版，便成为了一个重要的议题，即本文主题。

## java的概念明晰
jvm, jre, jdk, java fx 之间是包含关系。jvm 是执行程序的虚拟机，是核心；jre 是基于 jvm 的标准库，相当于运行环境；jdk 在 jre 基础上提供面向开发者的工具，算是开发工具。但 jvm 并不是最小的分发单位，jre 才是。

java fx 的地位比较特殊，它是一套独立的图形界面方案，定位跟前面三个完全不同。简单说，它就是专门用来画桌面窗口、按钮、动画这些东西的库，靠显卡来做渲染，界面效果比传统 swing 强一截。很多 java 桌面的项目依赖它，像是 hmcl 这类启动器。问题是它从来不属于标准库的一部分，现在常见的 jdk 发行里也不会预装。开源社区维护的版本叫 openjfx，关系和 openjdk 差不多。如果用到的程序需要它，要么自己下载 openjfx 配上去，要么换一个已经把 java fx 打包好的 jdk 发行版。

## java版本选择

在使用场景中,java版本求稳不求新。原理很好解释,举个例子即可，java8(lts)官方扩展支持将持续到2030年12月，但同为lts的java17仅支持到2029年9月，从安全性等角度出发，这会是最有说服力的示例。

以minecraft举例
| minecraft版本 | java要求|
|--- |---|
| <= 1.12.2 | 仅java 8 |
| >= 1.16 | java 17+ |
| >= 1.21 | java 21+ |


## java 版本号选择

同版本下字数越大越好，越新越好。

java更新的主要内容
- 关键安全更新（CPU） - 经过测试的安全性更新
- 补丁集更新（PSU） - 功能性更新/调整
- 紧急更新 - 通常为修补前两类更新缺陷的紧急更新
## java主要发行版

| 发行版名称 | 发行商 | 描述 |
|------------|--------|------|
| Azul Zing (Azul Platform Prime) | Azul Systems | 优势: 自研 Falcon JIT 编译器, 无停顿 C4 垃圾收集器, 基于 JIT 缓存与 profile 引导的 ReadyNow 冷启动加速, 综合性能和停顿表现优异, 适合大型整合包服务器; 劣势: 仅支持 Linux; 商业产品, 仅 "评估和开发用途" 免费, 商业化服务器使用有法律风险; 需额外 JVM 参数才能发挥全部功能. |
| Azul Zulu (Azul Platform Core) | Azul Systems | 优势: 免费生产就绪级 OpenJDK, 更新稳定, 提供 JavaFX 选项和可视化 .msi 安装包, 安装/更新体验友好, 是官方 Java 的极佳替代; 劣势: 源代码不直接公开, 本质仍是标准 OpenJDK 构建, 性能与非定制版差异不大. |
| Oracle GraalVM (旧有 Java 发行版) | Oracle | 优势: 曾支持 AOT 原生编译, 多语言运行时, 冷启动快, 内存占用低; 劣势: Oracle 宣布停止发布新的 GraalVM for JDK 版本, 已 "坠机", 核心 Java 优化已或即将回归上游 OpenJDK, 不再适合作为主流 JDK 发行版持续选用. |
| Adoptium Temurin | Eclipse Adoptium (原 AdoptOpenJDK) | 优势: Eclipse 顶级项目, 最标准的 OpenJDK 构建, 社区驱动的生产就绪级, 多项认证, 曾是首选, 跨平台且完全开源免费; 劣势: 与上游几乎一致, 所谓性能提升通常不显著, 无特殊定制优化. |
| Amazon Corretto | Amazon | 优势: 声称综合性能与安全性提升, 使用自研密码学库强化加密, 免费, 长期更新; 劣势: 除密码学相关外, 性能与上游 OpenJDK 无显著差异, 亮点不多. |
| Alibaba Dragonwell | 阿里巴巴 | 优势: 基于 AJDK 的优化移植, 开源, 对大规模 Java 应用可能有一定增益; 劣势: 社区国际化程度有限, 更新节奏依赖阿里内部, 非定制场景下提升不明显. |
| IBM Semeru Runtime | IBM | 优势: 即 Eclipse OpenJ9, 内存占用相比 HotSpot 更优, 适合内存受限场景; 劣势: 性能往往不如 HotSpot, 且属于高度自定义的 JDK, 一般游戏服务器可能收益不大. |
| Microsoft Build of OpenJDK | Microsoft | 优势: 我的世界启动器默认下载的高版本 Java, 猜测为避免版权纠纷; 劣势: 无突出亮点, 本质上为标准 OpenJDK 构建, 只是提供方不同. |


## 言归正传

推荐 Azul Zing的发行版。
## 参考资料
- [geeksforgeeks](https://www.geeksforgeeks.org/java/differences-jdk-jre-jvm/)
