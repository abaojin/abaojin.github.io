---
layout: post
title:  "ai tensorflow之Window源码编译"
categories: ai
tags: ai
---

* content
{:toc}

## 简介

TensorFlow是谷歌开源的深度学习框架，是目前社区非常活跃的深度学习框架
，框架内容比较复杂，绝不是一个初学者能够简单入门的，这里笔者将和大家
一起学习TensorFlow的框架，今天主要给大家介绍的是Window系统下面编译框
架，废话不说，直接进入正题。

## 编译环境

Window 7 x64 或 Window 8 x64 或 Window 10 x64系统。

## 安装软件

```
vs2015.ent_chs.iso
Anaconda3-4.2.0-Windows-x86_64.exe
swigwin-3.0.11.zip
Git-2.11.0-64-bit.exe
cmake-3.7.1-win32-x86.msi
```

备注：
* 安装需记清楚各个软件安装路径，后续的编译需要使用Cmake、vs2015
swigwin、python等工具。
* 笔者是要就Tensorflow源码，故没有设计到CUDA相关支持。



## 生成VS工程

下载tensorflow源代码，可以使用下面的方式

```
git clone –recursive https://github.com/tensorflow/tensorflow 
```

建议登录网站https://github.com/tensorflow/tensorflow 
直接下载主分支代码压缩包tensorflow-master.zip。 
解压tensorflow-master.zip，到你指定的文件夹,例如我直接放在”F:\”下。

然后执行如下的编译指令生成工程。

```
"D:/Program Files (x86)/Microsoft Visual Studio 14.0/VC/bin/amd64/vcvars64.bat" 
cd /d F:\tensorflow-master\tensorflow\contrib\cmake 
mkdir build 
cd build
cmake .. -A x64 -DCMAKE_BUILD_TYPE=Release -DSWIG_EXECUTABLE="C:/Program Files/swigwin-3.0.12/swig.exe" -DPYTHON_EXECUTABLE="C:/Program Files/Python35/python.exe" -DPYTHON_LIBRARIES="C:/Program Files/Python35/libs/python35.lib"
```

备注：
 
* 上面命令的路径，根据你对应安装程序的路径进行修改。比如cmake命令里的路径 

D:/swigwin-3.0.11/swig.exe 

D:/Program Files/Anaconda3/python.exe 

D:/Program Files/Anaconda3/libs/libs/python35.lib 

还有上述命令默认不使用GPU，如果已经安装了cudnn等支持包，可以在cmake命令里添加字段
 
-Dtensorflow_ENABLE_GPU=ON -DCUDNN_HOME=”D:…\cudnn”

## 编译

通过上述步骤那么在F:\tensorflow-master\tensorflow\contrib\cmake\build生成vs工程，直接打开TensorFlow.sln工程直接编译工程即可，由于有上百个工程，编译需要一定时间，耐心等待即可。


## 参考资料

[cmake编译过程](http://blog.csdn.net/challno/article/details/54632501)


