---
layout: post
title:  "csharp string类解读"
categories: csharp
tags: csharp
---

* content
{:toc}

## string类型

引用类型,class String继承自System.Object,但是String是一个特殊的引用
类型，特殊关键在两点:不可变性和常驻性。

## string对象不可变性

字符串对象一旦创建，在整个进程的生命周期中是不可改变的，无法对其进行
加长、缩短、改变等操作，既然它不可变，所以也就不存在线程同步的问题。
看看下面代码：

```
string str1 = "Hello";
string str2 = str1;            
Console.WriteLine(object.ReferenceEquals(str1, str2)); //True

str1 += " World";
Console.WriteLine(object.ReferenceEquals(str1, str2)); //False
```



第一次调用object.ReferenceEquals方法比较的str1和str2，它们指向的是同一个
字符串对象引用，所以结果为true，而str1 += " World";的过程是重新创建了一个
对象，且把新的对象引用赋给str1，此时str1与str2指向的不是同一个对象引用，
所以在第二次调用object.ReferenceEquals方法时返回的是false。无论是使用+=操
作符还是其他的对字符串修改的方法，都会引起重新创建字符串对象，并且复制旧的
字符串到新的内存区，而不是我们常说的“对XX字符串进行修改”，如果非要说“改变”，
那就是对对象引用的改变。

## string对象的驻留性

根据前面的描述，字符串是不可变的，而在编程中，我们会大量使用字符串，这
就会导致不停地创建字符串对象，不停地分配内存，并且很有可能不停地执行垃
圾回收，如此以来会大大损伤性能,所以CLR对字符串进行了特殊的优化机制，下
面我们来对这些机制及特性进行描述。

字符串驻留是CLR提供的一种提高性能的对待字符串的机制，它保证在一个进程内
的某个字符串在内存中只分配一次。看以下代码：

```
string str1 = "abc";
string str2 = "abc";
Console.WriteLine(object.ReferenceEquals(str1, str2)); //True
```

这里声明了两个对象str1和str2，调用object.ReferenceEquals方法返回的是True，
为什么它们指向的是同一个引用呢？这就说明了CLR的字符串驻留，相同的字符串在
托管内存中只分配一次，再次声明相同的字符串对象时，会将后来一次的声明指向第
一次声明所引用的对象。那么CLR如何保证做到的呢？原来，在CLR初始化时创建一个
内部的哈希表，我们知道哈希表在处理表内数据时是非常快的，这个表相当于一个字
典表（HashMap<TKey,TValue>），键就是字符串，而值是指向托管堆中该字符串对象
的引用，当在声明一个字符串时，会调用对象的Intern方法，该方法接收一个string
对象，它会先在哈希表中检查该字符串是否存在？如果存在，则返回这个字符串对应
的对象引用；否则，将创建该字符串的副本，并将副本添加到哈希表中，最后返回对
该副本对象的引用。String类还提供了一个IsInterned方法，该方法会根据字符串在
哈希表中检查是否已经存在相同的串，如果存在，则返回该字符串对象的引用，否则
，返回null，但它是它不会向哈希表中添加字符串。我们对上面的代码进行改造：

```
string str1 = "abc";
string str2 = "abc";
Console.WriteLine(object.ReferenceEquals(str1, str2)); //True
str1 += str2;
Console.WriteLine(str1);
```

字符串”abc”是有驻留的,+=操作是要重新创建对象的，但CLR对临时计算的新对象"abcabc"
没有进行驻留。这里可以看出，并不是所有的字符串都会进行驻留，我们将代码改成这样：

```
string str1 = "abc";
string str2 = "abc";
Console.WriteLine(object.ReferenceEquals(str1, str2)); //True
str1 += str2;
Console.WriteLine(str1);
string str3 = "abcabc";
Console.WriteLine(object.ReferenceEquals(str1, str3)); //False
str1 = string.Intern(str1);
Console.WriteLine(object.ReferenceEquals(str1, str3)); //True
```

在没有使用string.Intern方法是，字符串对象是没有驻留的，使用这个方法后，"abcabc"
是进行驻留，值得注意是：尽管String.Intern(string)方法的字符串参数（上面代码中的
str1）被垃圾回收器回收，但是CLR已将这个str1的副本添加到哈希表中，垃圾回收器是无
法对哈希表引用的字符串进行回收。

## 参考资料

[CSDN字符串只是梳理](http://www.cnblogs.com/solan/archive/2012/08/03/CSharp07.html?utm_source=tuicool&utm_medium=referral)

[MSDN字符串特效介绍](https://msdn.microsoft.com/en-us/library/ms228362.aspx)

[String字符串源码实现](https://referencesource.microsoft.com/#mscorlib/system/string.cs,8281103e6f23cb5c)

[Object对象源码实现](https://referencesource.microsoft.com/#mscorlib/system/object.cs,d9262ceecc1719ab)





