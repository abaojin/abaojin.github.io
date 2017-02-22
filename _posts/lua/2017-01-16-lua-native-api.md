---
layout: post
title:  "lua 原生api解读"
categories: lua
tags: lua
---

* content
{:toc}

## 简介

研究lua也有一段时间了，对lua也算是小有了解，对于lua的api可能和其他脚本语言不是
很相同，使用lua来编写，lua原生api都采用C语言开发的，理解这些api含义对于理解lua
语言设计的本身又起着至关重要的作用，故此在这里记录下，lua-api具体含义。

## LuaAPI

#### 1.创建新表
```
void lua_createtable(lua_State *L, int narr, int nrec)

创建一个新表，并将它放在栈顶，narr和nrec分别制定该table的array和hash部分的预分配
元素的数量。
返回值：无
栈高度+1，栈顶元素是新的table。
 ```

#### 2.创建新表
 ```
 #define lua_newtable(L) lua_createtable(L, 0, 0)
 创建表的另一个api，常用这个api,上面的宏一定很清楚不多介绍。
 ```

 #### 3.取表中元素
 ```
void lua_getfield (lua_State *L, int index, const char *k)
操作:   arr = Stack[index]
        Stack.push( arr[k] )
取表中键为k的元素, 这里的表是由index指向的栈上的一个表
无返回值
栈高度+1, 栈顶元素是(Stack[index])[k]
注意, 该操作将触发 __index 元方法

 ```







## 参考资料

[lua源码](https://github.com/lua/lua)

[UniLua源码](https://github.com/xebecnan/UniLua)

[Csharp-lua源码](https://github.com/abaojin/csharp-lua)

[lua-api论坛](http://www.cnblogs.com/ringofthec/archive/2010/10/22/lua.html)



	






