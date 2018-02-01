---
layout: post
title:  "csharp box unbox装箱和拆箱"
categories: csharp
tags: csharp
---

* content
{:toc}

## 介绍

拆箱和装箱是C#很基础问题，这里简单说明一些装箱
和拆箱基本概念和应用，装箱和拆箱是值类型和引用
类型之间相关转换要执行的操作。

## 概念

1. 装箱在值类型向引用类型转换时发生。
2. 拆箱在引用类型向值类型转换时发生。

## 装箱

code
``` java
using System;

namespace ConsoleApplication1
{
    struct Vector : IDisposable
    {
        public void Dispose()
        { }
    }

    class Matrix : IDisposable
    {
        public void Dispose()
        { }
    }

    class Program
    {
        static void Main(string[] args)
        {
            object obj_v = new Vector();// 发生装箱
            object obj_i = 10;// 发生装箱
            object obj_m = new Matrix();// 未发生装箱
            int val_i = 10;// 未发生装箱
            Vector val_v = new Vector();// 未发生装箱
        }
    }
}
```
 code to il
``` java
.method private hidebysig static void  Main(string[] args) cil managed
{
  .entrypoint
  // Code size       43 (0x2b)
  .maxstack  1
  .locals init ([0] object obj_v,
           [1] object obj_i,
           [2] object obj_m,
           [3] int32 val_i,
           [4] valuetype ConsoleApplication1.Vector val_v,
           [5] valuetype ConsoleApplication1.Vector V_5)
  IL_0000:  nop
  IL_0001:  ldloca.s   V_5
  IL_0003:  initobj    ConsoleApplication1.Vector
  IL_0009:  ldloc.s    V_5
  IL_000b:  box        ConsoleApplication1.Vector//执行ILbox指令，在内存堆中申请Vector类型需要的堆空间
  IL_0010:  stloc.0
  IL_0011:  ldc.i4.s   10
  IL_0013:  box        [mscorlib]System.Int32//执行ILbox指令，在内存堆中申请System.Int32类型需要的堆空间
  IL_0018:  stloc.1
  IL_0019:  newobj     instance void ConsoleApplication1.Matrix::.ctor()
  IL_001e:  stloc.2
  IL_001f:  ldc.i4.s   10
  IL_0021:  stloc.3
  IL_0022:  ldloca.s   val_v
  IL_0024:  initobj    ConsoleApplication1.Vector
  IL_002a:  ret
} // end of method Program::Main
```

说明

* obj_v 是引用类型，new Vector()是值类型struts，将
值类型转为引用类型是发生装箱操作，变量obj_i也发生了装
箱操作。

* obj_m 是引用类型，new Matrix()是引用类型class，将
引用类型转为引用类型，并不会发生装箱操作。val_i和val_v
都是值类型赋给值类型，也不会发生装箱操作。

总结

以上就是装箱所要执行的操作了，执行装箱操作时不可避免的要
在堆上申请内存空间，并将堆栈上的值类型数据复制到申请的堆
内存空间上，这肯定是要消耗内存和cpu资源的。




## 拆箱

code
``` java
namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            object objValue = 4;// 发生装箱
            int value = (int)objValue;//发生拆箱
        }
    }
}
```

code to il
``` java
.method private hidebysig static void  Main(string[] args) cil managed
{
  .entrypoint
  // Code size       16 (0x10)
  .maxstack  1
  .locals init ([0] object objValue,
           [1] int32 'value')
  //上面IL声明两个局部变量object类型的objValue和int32类型的value变量
  IL_0000:  nop
  IL_0001:  ldc.i4.4//将整型数字4压入栈
  IL_0002:  box        [mscorlib]System.Int32//执行ILbox指令，在内存堆中申请System.Int32类型需要的堆空间
  IL_0007:  stloc.0//弹出堆栈上的变量，将它存储到索引为0的局部变量中
  IL_0008:  ldloc.0//将索引为0的局部变量（即objValue变量）压入栈
  IL_0009:  unbox.any  [mscorlib]System.Int32//执行IL拆箱指令unbox.any 将引用类型object转换成System.Int32类型
  IL_000e:  stloc.1//将栈上的数据存储到索引为1的局部变量即value
  IL_000f:  ret
} // end of method Program::Main
```

说明

上面的两行代码会执行一次装箱操作将整形数字常量4装箱成引用类型object变量objValue；然后又执行一次拆箱操作，将存储到堆上的引用变量objValue存储到
局部整形值类型变量value中。

总结

拆箱操作的执行过程和装箱操作过程正好相反，是将存储在堆上的引用类型值转换
为值类型并给值类型变量。装箱操作和拆箱操作是要额外耗费cpu和内存资源的，
所以在c# 2.0之后引入了泛型来减少装箱操作和拆箱操作消耗。


## 参考资料







