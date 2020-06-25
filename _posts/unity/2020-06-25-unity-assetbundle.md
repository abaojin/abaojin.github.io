---
layout: post
title:  "unity assetbundle"
categories: unity
tags: unity assetbundle
---

* content
{:toc}

## 介绍

AssebBundle是Unity引擎处理资源方式，一个AssetBundle可以包含一个或多个Unity识别的Asset，今天
就简单聊聊AssetBundle关键技术点。

## 打包

使用BundleName方式构建AssetBundle。

+ 每一个AssetBundle可以包含1个或多个Asset，具体可以是贴图，动画，材质甚至是场景。
+ 打包必须处理好AssetBundle与另一个AssetBundle之间的依赖， 否则容易造成资源冗余。

## 压缩

+ 不压缩
+ LZMA 压缩率比较高，但是压缩时间和解压时间都比较慢
+ LZ4 压缩比相对较低，但是压缩时间和解压时间都比较快（推荐方式）

## 加载

+ AssetBundle.LoadFromFile 同步
+ AssetBundle.LoadFromFileAsync 异步
  - 推荐加载方式，效率高，节省内存
  - 加载只会加载AssetBundle头信息，不会加载具体Asset资源
  - 只能加载使用LZ4方式压缩资源，不支持LZMA压缩的资源
+ AssetBundle.LoadFormMemory 同步
+ AssetBundle.LoadFormMemoryAsync 异步
+ AssetBundle.LoadFormStream 同步
+ AssetBundle.LoadFormStreamAsync 异步
  - 不推荐加载方式，效率一般，主要浪费内容，必须先把资源加载成字节流
  - 内容最少占用2份，如果资源采用加密方式，可以考虑这种方式

## 注意事项

加载AssetBundle.LoadFromFile(Async)加载资源只加载AssetBundle头信息，是在Unity5.4之后版本支持的。

## 参考资料

[Unity Compress](https://blog.csdn.net/llsansun/article/details/86215830)
[Unity AssetBundle](https://learn.unity.com/tutorial/assets-resources-and-assetbundles#Loading_Asset_Bundles)

