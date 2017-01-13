---
layout: post
title:  "jekyll 添加多媒体图片和附件"
date:   2017-01-13 22:14:54
categories: jekyll
tags: jekyll
---

* content
{:toc}


## 图片

图片是博客系统中使用非常频繁的表达方式，如何在你博客加入图片呢，这里
简单介绍一下，在jekyll中加入图片，在博客系统根路径下面建一个media文件夹
用于存放多媒体资源，使用图片相对路径，在超链接markdown标签前加叹号即可，
下面是一张图片显示案例。

![test_001](/media/image/test_001.jpg)



## 附件

附件的方式和图片基本一样，就是一个超链接，链接地址即为附件。附件的显示
方式和超链接是一样的，只是这里需要写入附件的相对路径。

[我的音频](/media/audio/test_001.mp3)

## 说明

添加多媒体有很多种方式，这里只是介绍最简单一种，也可以按照上面格式输入
多媒体全路径。如果不懂这种方式，可以直接fork项目看编码实现：[abaojin]
(https://github.com/abaojin/abaojin.github.io)。

## 参考资料

[博客资料](http://www.cnblogs.com/OtisBlog/p/4487660.html)

[jekyll官网](http://jekyll.com.cn/)



