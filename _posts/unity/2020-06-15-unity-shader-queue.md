---
layout: post
title:  "unity shader 渲染管线和顺序"
categories: unity
tags: unity shader
---

* content
{:toc}

## 介绍

熟悉3D渲染顺序，对于游戏开发来说非常重要，不仅仅是基础知识，更是一项非常重要的工作技能，相信很多
同学对于Unity引擎的渲染顺序还是没有没有搞清楚，如果你已经非常清楚了，完全跳过阅读，避免浪费你宝
贵的时间。

## 渲染管线

+ 模型数据
+ 顶点着色器（自定义操作）
  - 顶点变换
  - 顶点裁剪
  - 面剔除Cull
  - 图元装配
  - 光栅化操作
  - 深度测试（Early-Z）
+ 片段着色器（自定义操作）
  - 文理赋值
  - 光照处理
+ 各种测试
  - 透明度测试 （Alpha Test Early-Z操作会失效）
  - 模板测试（Stencil Test）
  - 深度测试 (Depth Test)
  - 混合测试 (Blend Mode)
    + Blend Off 关闭混合
    + Blend SrcFactor DstFactor 基本配置，并启动混合操作，产生颜色乘以SrcFactor加上已有颜色乘以DstFactor
    + Blend SrcFactor DstFactor SrcFactorA DstFactorA 同上，只是Alpha单独处理
    + BlendOp Add|Min|Max|Sub|RevSub 混合的方式
      + Add 默认操作，将源像素和目标像素相加
      + Sub 将源像素减去目标像素
      + Min 取目标像素和源像素较小者
      + Max 取目标像素和源像素较大者
      + RevSub 将目标像素减去源像素
    + Blend Factor 混合因子
      + One 值为1，使用该因子，用来使帧缓冲区颜色和目标颜色完全通过
      + Zero 值为0，使用该因子，用来删除帧缓冲区颜色和目标颜色
      + SrcColor——使用此因子为将当前值乘以帧缓冲区源颜色的值
      + SrcAlpha——使用此因子为将当前值乘以帧缓冲区源颜色的Alpha的值
      + DstColor——使用此因子为将当前值乘以帧缓冲区目标颜色的值。
      + DstAlpha——使用此因子为将当前值乘以帧缓冲区目标颜色Alpha分量的值
      + OneMinusSrcColor——使用此因子为将当前值乘以(1-帧缓冲区源颜色值)
      + OneMinusSrcAlpha——使用此因子为将当前值乘以(1-帧缓冲区源颜色Alpha分量的值)
      + OneMinusDstColor——使用此因子为将当前值乘以(1-目标颜色值)
      + OneMinusDstAlpha——使用此因子为将当前值乘以(1-目标颜色Alpha分量的值) 
+ 更新帧缓冲中颜色值

备注： 顺序可能存在问题，随时更正。

## 渲染顺序

+ Camera Depth 越小越优先
+ RendererQueue 2500以下的（含2500）
    + Sorting Layer/Order in Layer 按照Sorting Layer/Order in Layer 设置的值，越小越优先
    + RenderQueue， 越小越优先
    + RenderQueue 相等，由近到远排序优先
+ RenderQueue 2500已上
    + Sorting Layer/Order in Layer 按照Sorting Layer/Order in Layer 设置的值，越小越优先
    + RenderQueue， 越小越优先
    + RenderQueue 相等，由远到近排序优先

## 注意事项

Sprite组件和Canvas组件 默认使用的Shader未写入z缓冲，但是进行z缓冲测试 默认 RenderQueue 均为 Transfront=3000

## 参考资料

[Unity Shader Reference](https://docs.unity3d.com/Manual/SL-Reference.html)
[Unity AlphaTest AlphaBlending](https://blog.csdn.net/candycat1992/article/details/41599167)

