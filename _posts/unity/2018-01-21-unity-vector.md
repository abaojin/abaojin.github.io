---
layout: post
title:  "unity Vector基本操作"
categories: unity
tags: unity
---

* content
{:toc}

## 介绍

无论是在3D数学基础和现实生活中，Vector的操作都非常重，今天就向量定义和基本操作简单记录一下。

## 定义

向量：也称矢量，指即有大小也有方向的量。

标量：也称数量，指只有大小的量。

## 模计算

模即向量长度。

V1=(x1, y1, z1) <br>
V2=(x2, y2, z2)

|V1| = 根号x1*x1 + y1*y1 + z1*z1 <br>
|V2| = 根号x2*x2 + y2*y2 + z2*z2




## 点乘 dot

向量点乘得到是一个标量，几何意义是两个向量之间的夹角或一个向量在另一个向量上映射。

V1=(x1, y1, z1) <br>
V2=(x2, y2, z2)

V1*V2 = |V1||V2|cos<V1,V2> <br>
V1*V2 = x1*x2 + y1*y2 + z1*z2

## 叉乘 cross

向量叉乘得到是一个向量，几何意义向量叉乘得到是一个垂直于两个向量的向量。

V1=(x1, y1, z1) <br>
V2=(x2, y2, z2)

V1xV2 = |V1||V2|sin<V1, V2> <br>
V1xV2 = (y1*z2 - z1*y2, z1*x2 -x1*z2, x1*y2 - y1*x2)


## 相加 add

向量相加得到还是一个向量，几何意义向量加法满足平行四边形法则。

V1=(x1, y1, z1) <br>
V2=(x2, y2, z2)

V1+V2 = (x1+x2, y1+y2, z1+z2)

## 减法 sub

向量减法得到还是一个向量，几何意义向量减法满足三角形法则。

V1=(x1, y1, z1) <br>
V2=(x2, y2, z2)

V1-V2 = (x1-x2, y1-y2, z1-z2)

## 参考资料

[vector - 向量类基本定义](http://blog.csdn.net/qq_33951440/article/details/70832840)





	






