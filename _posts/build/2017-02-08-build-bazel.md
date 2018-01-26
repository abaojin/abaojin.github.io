---
layout: post
title:  "build bazel简单介绍"
categories: build
tags: build
---

* content
{:toc}

## 简介

Bazel是Google的一个项目构建工具，目前还处于测试阶段，Bazel支持构建客户端和服务器软件，
包括客户端的应用iOS和Android，同时也提供了扩展框架去自定义自己的构建规则。

## 为什么使用Bazel?

项目构建的工具已经非常的多，比如java的ant maven，C语言make等等，那么为什么还有使用
Bazel呢？原因是Bazel足够的强大，主要表现这几个方便：




### 速度

Bazel拥有优化的依赖分析，高效缓存和并行执行构建的行为，你能获得更加快速构建和清理。

### 可伸缩性

Google数以千计的工程使用Bazel来构建大型项目，关键的基础设施服务和公共的web应用程序。
然而Bazel仅仅是个一个小项目。

### 灵活性

全面和灵活规则允许你构建各种不同平台不同语言的项目，Bazel是一个是一个可扩展规则框架
，允许你去开发和分享你的构建规则为其他编程语言和平台。

### 正确性

Bazel在依赖配置中检查检查源文件内容，而不仅仅依赖于时间戳去确定是否需要重新构建，因为
增量构建几乎总是正确的，没有必要去每次都清理项目。

### 可靠性

当你使用Bazel构建软件时，你运行的工具，在Google内部已经经过多年的提炼和测试，是足够
可靠和稳定的。

## Bazel处于测试阶段

Bazel目前还处于测试阶段，有很多功能还是存在缺陷的，但是相信Bazel一定能够成为一个足够
稳定产品为大家使用。

## Bazel使用

Bazel使用相对来说也是比较简单的，Bazel官网的资料也写的非常的详细，这里就不介绍具体使用
方法。

[构建java案例](https://bazel.build/versions/master/docs/tutorial/java.html)

[构建c++案例](https://bazel.build/versions/master/docs/tutorial/cpp.html)

[构建Mobile Application案例-iOS和Andoroid](https://bazel.build/versions/master/docs/tutorial/app.html)

## 参考资料

[Bazel源码](https://github.com/bazelbuild/bazel)

[Bazel官网](https://bazel.build/)



	






