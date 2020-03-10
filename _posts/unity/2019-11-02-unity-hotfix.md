---
layout: post
title:  "unity 热更新版本控制"
categories: unity
tags: unity
---

* content
{:toc}

## 介绍

热更新或热修复是现在手游必不可少的东西，这里不介绍怎么进行资源，采用什么脚本进行
游戏的开发，我们假定我们现在需要更新游戏的资源、脚本、配置表等数据资源，该如何来
控制资源的版本呢。

## 版本

游戏版本一般推荐使用3位表示

+ 版本号组成： 大版本号.次版本号.资源版本号
+ 应用版本号： 大版本号+次版本号+资源版本号

## 结构

+ cdn
  + ios
    + ios_app_1.0.ipa
    + 1.0 (大版本号)
      + 1 (资源版本号可以使用SVN号)
        + version.txt
        + depends
        + bundles
        + scripts
        + configs
      + 2 (资源版本号可以使用SVN号)
        + version.txt
        + depends
        + bundles
        + scripts
        + configs
  + android
    + android_app_1.0.apk
    + 1.0 (大版本号)
      + 1 (资源版本号可以使用SVN号)
        + version.txt
        + depends
        + bundles
        + scripts
        + configs
      + 2 (资源版本号可以使用SVN号)
        + version.txt
        + depends
        + bundles
        + scripts
        + configs
  + osx
    + osx_app_1.0.dpg
    + 1.0 (大版本号)
      + 1 (资源版本号可以使用SVN号)
        + version.txt
        + depends
        + bundles
        + scripts
        + configs
      + 2 (资源版本号可以使用SVN号)
        + version.txt
        + depends
        + bundles
        + scripts
        + configs

## 流程

+ 获得服务器版本号，与客户端当前版本号作比较，如果是大版本更新，去商店下载，如果是小版本更新则去资源服务器下载。

+ 拼接资源服务器地址，获得versionFile下载地址，根据大版本号，手机平台和资源版本号得到下载地址为：baseurl/ios/1.0/170513.1/VersionFile.txt。

+ 下载VersionFile，并与上一次手机缓存的VersionFile作对比，生成从上一次更新到这次更新需要下载的文件列表，然后逐一去对应的地址下载下载，资源地址需要根据VersionFile里面的版本号去生成。例如：baseurl/ios/1.0/170512.1/test.txt,baseurl/ios/1.0/170403.1/test.txt。每一个资源去对应的版本文件夹下去下载。

+ 下载完所有文件后，保存本次下载的VersionFile，等待下一次更新时与新的versionFile再做比较。

## 注意事项

 当有大版本更新时，更新结束后，应该删除之前缓存的所有更新，因为新包都是默认带着所有资源。这样既减少包的大小，也避免了游戏运行时加载错资源。因为VersionFile存放了我们所有的更新文件，所以游戏加载资源会根据VersionFile是否存在该资源来决定去加载下载目录里的资源还是去加载包里面自带的资源。因此游戏包更新后为了避免加载到上一个版本的资源，应该删除掉之前下载目录里的文件。