---
layout: post
title:  "build make使用简单介绍"
categories: build
tags: build
---

* content
{:toc}

## 简介

GUN Make简单来说就是项目构建工具，用来快速的构建项目，注意和CMake区别。

## 文件

GUN Make默认寻找当前目录下面的，“GNUmakefile”、“makefile”和“Makefile”。其按顺序找这三个文件，一旦找到，就
开始读取这个文件并执行。
也可以指定文件，比如定义文件是android.mk，执行时我们就可以使用：make -f android.mk方式执行。



## 目标

GNU Make是由目标组成的，默认用第一个目标作为最终目标，以此来关联其他目标，这是其默认的执行方式。当然也可以指定目标
执行，比如Makefile里面定义多个目标all、clean，这时想先执行clean目标，只需要 make clean即可按照指定的目标执行。

## 特殊标识

GUN Make常见的赋值运算包括，= 、:=、?=、+=这些运算符代表了不同的含义。
= 是最基本的赋值
:= 是覆盖之前的值
?= 是如果没有被赋值过就赋予等号后面的值
+= 是添加等号后面的值

内置常用变量（Implicit Variables）：
$@ 当前目标名称
$^ 所有依赖文件
$< 第一个依赖文件
$(MAKE) 当前使用的Make工具
$(CC) 当前使用的编译器

## 执行顺序

make 回    车 ： 默认执行第一个目标
make 目标名  ：执行指定的目标

## 使用案例

```
PARAM= install comple run clean

all:none

install:comple
	gcc main.o math.o  -o main.exe

uninstall:clean
	rm *.exe

comple:
	gcc -c main.c math.c

run:dunmy
	main.exe

none:
	@echo "Please do 'make param' where param is one of these:"
	@echo "   $(PARAM)"

dunmy:

clean:dunmy
	rm *.o
```

## 参考资料

[make指令教程](http://www.ruanyifeng.com/blog/2015/02/make.html)

[make官网资料](http://www.gnu.org/software/make/)


