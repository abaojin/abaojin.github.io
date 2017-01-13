---
layout: post
title:  "jekyll 支持多媒体图片附件"
date:   2017-01-13 22:14:54
categories: jekyll
tags: jekyll
---

* content
{:toc}

## 图片

图片是博客系统中使用非常频繁的表达方式，如何在你博客加入图片呢，这里
简单介绍一下，在jekyll中加入图片标记，在博客系统根路径下面建一个_media
用于存放多媒体资源。

格式：@![图片名称](图片相对地址路径)，例如下面图片：

![test_001](/_media/image/test_001.jpg)

## 附件

附件的方式和图片基本一样，就是一个超链接，链接地址即为附件。

格式：@[附件名称](附件的链接地址)，例如下面声音附件：

[我的音频](/_media/audio/test_001.mp3)

## 说明

添加多媒体有很多种方式，这里只是介绍最简单一种，也可以按照上面格式输入
多媒体全路径。

## 参考资料

[博客资料](http://www.cnblogs.com/OtisBlog/p/4487660.html)

[jekyll官网](http://jekyll.com.cn/)



