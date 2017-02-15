---
layout: post
title:  "Unity 插件开发"
categories: Unity
tags: Unity
---

* content
{:toc}

## 介绍

Unity可以开发两种插件，Managed plugins和Native plugins。

Managed plugins 是托管式.NET插件，使用对应Unity的.NET版本进行开发。

Native plugins 平台相关的原生插件，可以访问操作系统的插件或者第三方的插件。

在Unity5之前，通过插件存放的目录来区分支持的目标平台；然而Unity5可以把插件放
在任何目录，通过Plugin Inspector设置相关属性就可以了，但为了兼容以前的版本，
Unity5也会支持插件存放的目录，进行默认的插件设置。

规则如下:

文件夹	                                                  默认插件设置
Assets/**/Editor	                                    只兼容Editor
Assets/**/Editor/(x86 or x86_64 or x64)	                只兼容Editor，如果子文件夹存在，用于匹配目标CPU
Assets/Plugins/x86_64(or x64)	                        x64兼容
Assets/Plugins/x86	                                    x86兼容
Assets/Plugins/Android/(x86 or armeabi or armeabi-v7a)	只跟Android兼容，如果子文件夹存在，用于匹配目标CPU
Assets/Plugins/iOS	                                    只跟iOS兼容

## 托管插件（Managed plugins）

通常情况下，我们直接使用脚本实现相关功能，然后由Unity编译成目标可执行文件。但有
时我们想在外部把脚本编译成DLL，然后放在Unity中使用。这个DLL就是这里所说的托管式
插件，兼容.NET二进制。

## 原生插件

原生插件一般采用C,C++,Objective-C等等编写，Unity允许游戏代码来访问这些原生插件中的函数，
允许Unity和一些中间件库或者已有的C/C++进行整合和。

### Linux平台

TODO

### Mac平台

TODO

### Window平台

TODO

### Android平台

TODO

### iOS平台

TODO

## 参考资料

[Unity plugins github demo](https://github.com/hellowod/unity-plugins-development)

[Unity Manual](https://docs.unity3d.com/Manual/UnityManual.html)

[Unity Plugins](https://docs.unity3d.com/Manual/Plugins.html)

[Building Plugins for iOS](https://docs.unity3d.com/Manual/PluginsForIOS.html)

[Building Plugins for Android](https://docs.unity3d.com/Documentation/Manual/PluginsForAndroid.html)

[Building Plugins for Desktop Platforms](https://docs.unity3d.com/Documentation/Manual/PluginsForDesktop.html)





	






