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
  - 顶点裁剪
  - 面剔除Cull
  - 图元装配
  - 光栅化操作
+ 片段着色器（自定义操作）
  - 文理赋值
  - 其他操作
+ 各种测试
  - 模板测试（Stencil Test）
  - 深度测试 (Depth Test)
  - 透明度测试 (Alpha Test)
  - 混合测试 (Blend Mode)
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

