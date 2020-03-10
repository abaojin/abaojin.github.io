---
layout: post
title:  "lua 性能分析"
categories: lua
tags: lua
---

* content
{:toc}

## 简介

lua作为一种脚本语言，主要的用途有两个方面，一方面是作为项目或应用的配置脚本使用，另一方面
作为应用的逻辑编写脚本使用，作为逻辑编写脚本主要提交游戏方便，今天主要讨论lua编写时的一些
影响性能的注意事项，希望对于热爱lua这门脚本语言的同学以启发。

## 测试函数

为了进行性能，这里给出一个简单性能测试通用函数，下面的测试都将使用这个函数进行测试，废话不
多说，直接贴上代码。

```lua
function profiler(func, count)
    if type(func) ~= "function" then
        return
    end
    
    if count == nil then
        count = 100
    end

    local total_time = 0
    for i = 1, count do
        local startTime = os.clock()
        func()
        local endTime = os.clock()
        total_time = total_time + (endTime - startTime)
    end

    local average_time = total_time / count

    print("function run times=" .. count .. ",average cost time=".. average_time)
end
```

## lua表性能

table是Lua 的一种数据结构用来帮助我们创建不同的数据类型，如：数组、字典等。
下面给出两种方式看看性能数据

* 第一种方式

```lua
function func_table_1(c)
    for i = 1, c do
        local t = {}
        t[1] = 1
        t[2] = 2
        t[3] = 3
    end
end

profiler(function()
    func_table_1(100000)
end)

测试结果： function run times=100,average cost time=0.0631
```

* 第二种方式

```lua
function func_table_2(c)
    for i = 1, c do
        local t = {0, 0, 0}
        t[1] = 1
        t[2] = 2
        t[3] = 3
    end
end

profiler(function()
    func_table_2(100000)
end)

测试结果： function run times=100,average cost time=0.0279
```

上面两份测试结果可以看出第二种方式速度差不多是第一种的3倍，主要原因是：
默认创建出来的的表，都是空的，在插入元素的过程，逐渐翻倍扩大，从0到1， 1到2，2到4，...都会触发realloc，同时把旧元素拷贝到新申请的空间中，对于最终有成千上万个元素的table，扩张的开销可以接受，但是对于大量生成小的table的场景，会明显拖慢性能，可以通过lua的构造函数，让Lua的编译器预分配空间，Hash的扩展也是同样的原理。

## 全局和局部变量性能

全局变量和局部变量是编写代码时时刻需要涉及的，但是什么时候使用全局，什么时候使用局部，这些都是非常影响代码运行效率，先分析两种方式，看看性能结果。

* 第一种方式

```lua
function func_var_1(c)
    r = 0
    for i = 1, c do
        r = r + i
        math.type(i)
    end
end

profiler(function()
    func_var_1(1000000)
end)

测试结果：function run times=100,average cost 0.06832
```

* 第二种方式

```lua
function func_var_2(c)
    local r = 0
    local t = math.type
    for i = 1, c do
        r = r + i
        t(i)
    end
end

profiler(function()
    func_var_2(1000000)
end)

测试结果：function run times=100,average cost time=0.03914
```

上面测试结果可以看出第二种方式也是第一种方式的2倍效率，其中原因主要是因为
全局变量涉及的到表的查询和修改，所以性能要显著差于local变量，对于函数也是
相同的道理。

## 垃圾回收性能

lua5.0之后采用是3色标记的GC回收算法，相比较5.0的双色标记算法的有点是算法
可以分布进行计算，避免阻塞影响逻辑脚本性能，具体实现和影响性能测试改天单
独写一篇文章讲解，今天简单说如何避免GC的开销。

* 减少创建对象的次数，如果有需要可以循环利用
* 根据实际业务的需要，控制垃圾回收的启动和关闭

## 参考资料

[lua源码](https://github.com/lua/lua)
