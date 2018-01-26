---
layout: post
title:  "clang 函数变长参数解析"
date:   2017-01-13 23:14:54
categories: clang
tags: clang
---

* content
{:toc}

## 简介

C语言中我们经常会遇到一些函数参数可变的情况，例如print函数，如何来声明
、定义、使用这样的函数，这里简单记录一下，大神请直接跳过。



## 声明

可变参数函数定义，可变参数必须放在所有参数的最后，可变参数使用 ... 三个
点表示。例如：

```
// 声明可变函数（func_param）
void func_param(int p, ...);
```

## 定义

可变函数定义关键是如何将可变的参数取出来，这是关键，这里必须提到三个API
这三个api包含在stdarg.h包中，stdarg.h 头文件定义了一个变量类型 va_list 
和三个宏，这三个宏可用于在参数个数未知（即参数个数可变）时获取函数中的
参数。va_list这是一个适用于 va_start()、va_arg() 和 va_end() 这三个宏
存储信息的类型。三个宏的声明如下：

```
void va_start(va_list ap, last_arg)
type va_arg(va_list ap, type)
void va_end(va_list ap)
```

案例：

```
void func_param(int p, ...) {
	va_list argp;
	int arg_v = p;
	int arg_c = 0;
	va_start(argp, p);
	do {
		++arg_c;
		printf("the %d th argv: %d\n", arg_c, arg_v);
		arg_v = va_arg(argp, int);
	} while (arg_v != NULL);
	va_end(argp);
}
```

## 调用

可变参数调用和普通函数调用没有什么区别，只是可以指定边长的参数。
案例：

```
int main(int argc, char* argv[]) {
	func_param(1);
	printf("-----------------------\n");
	func_param(1, 2, 3);
	return 0;
}

输出：
the 1 th argv: 1
-----------------
the 1 th argv: 1
the 2 th argv: 2
the 3 th argv: 3
```

## 完整代码

```
#include <stdio.h>
#include <stdarg.h>

void func_param(int p, ...);

int main(int argc, char* argv[]) {
	func_param(10);
	printf("-----------------------\n");
	func_param(1, 2, 3);
	return 0;
}

void func_param(int p, ...) {
	va_list argp;
	int arg_v = p;
	int arg_c = 0;
	va_start(argp, p);
	do {
		++arg_c;
		printf("the %d th argv: %d\n", arg_c, arg_v);
		arg_v = va_arg(argp, int);
	} while (arg_v != NULL);
	va_end(argp);
}
```

## 参考资料

[c reference api](http://www.cplusplus.com/reference/cstdarg/)



























