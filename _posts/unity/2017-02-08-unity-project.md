---
layout: post
title:  "unity 资源结构设计"
categories: unity
tags: unity
---

* content
{:toc}

## 工程目录

一般一个纯粹的Unity工程的目录结构由三个目录组成。

Assets: 工程中的所有脚本和资源存放位置，此目录需要通过svn（或git等）进行管理

ProjectSettings: 工程设置目录，例如哪些场景需要编译等，需要通过svn进行管理

Library：工程编译的中间文件，包括 Atlas, Shader, metadata等，Unity运行需要，不存在会自动生成，不需要通过svn进行管理





## Asset目录内容

Asset中的内容分为两部分：

脚本：脚本如果使用C#进行代码编写一般会生成四个工程

资源：资源可以分为三类，Resoueces目录的资源，StreamingAssets目录的资源和其他资源

## 脚本工程

Assembly-CSharp-first-pass.csproj：包括了 Assets 目录下 Standard Assets, Standard Assets Pro, Plugins 三个目录里面的脚本

Assembly-CSharp-Editor first-pass.csproj：包括了上述三个目录下 Editor 子目录里面的脚本

Assembly-CSharp.csproj：包括了除了上述三个目录和所有 Editor目录下的其他脚本

Assembly-CSharp-Editor.csproj：包括了其他 Editor 目录下的所有脚本

editor 和 runtime 分开，工具和游戏分开，减少编辑的时候的编译时间，一般只会对一个工程进行修改，
此时主要编译其中一个工程即可。上述四个工程的依赖关系是，后编译的资源可以依赖于前面的工程，而不
能相反，editor 在 runtime 之后，保证 editor 工具可以访问 runtime 脚本。

## 编译方式

Android 平台运行时使用 mono 虚拟机，运行C#程序集，使用 JIT 编译方式

iOS 平台使用 AOT 编译方式，最后编译成的是native 可运行代码

## 资源档案meta文件

Assets 目录中的资源都会生成有一个meta文件“伴读”，该文件的作用有二，首先：Import 资源保存
ImportSetting，其次：保存每个资源的 guid，后文介绍。注意该文件需要纳入SVN进行管理，否则
会导致资源引用关系在不用项目成员之间发生混乱。

## 资源”身份证”—GUID

Assets每个资源都有一个meta文件，文件中记录了一个 guid，这个guid用于记录资源之间的引用关系，引用关系在介绍到后文的 fileID 后一并介绍。
这里介绍 guid 的生成规则：工程新import或者创建一个资源，生成 meta 文件和 guid；删除meta文件后，重新导入资源会生成新的 meta 文件和 guid；
在 非Unity中移动资源位置，会重新生成新的 meta 文件和 guid。另外guid的生成规则不是单纯按照目录结构或者资源内容来的，多次生成的guid是不同的，
这也就是meta文件需要提交的原因，防止同一个资源在不同人的工程中生成不同的guid。

## fileID和资源引用关系

首先介绍 fileID，fileID用于标识资源内部的资源。一般fileID也是直接记录在 meta 文件里面的。

资源间的依赖关系使用<guid, fileID>来确定

资源内部的依赖关系使用 fileID

## 参考案例

案例主要是给出一个Test2.prefab的Text内容，预设依赖三个对象：两个脚本，一个材质球

```
%YAML 1.1
%TAG !u! tag:unity3d.com,2011:
--- !u!1 &165074
GameObject:
  m_ObjectHideFlags: 0
  m_PrefabParentObject: {fileID: 0}
  m_PrefabInternal: {fileID: 100100000}
  serializedVersion: 4
  m_Component:
  - 4: {fileID: 486426}
  - 114: {fileID: 11406988}
  - 23: {fileID: 2355686}
  m_Layer: 0
  m_Name: Test2
  m_TagString: Untagged
  m_Icon: {fileID: 0}
  m_NavMeshLayer: 0
  m_StaticEditorFlags: 0
  m_IsActive: 1
--- !u!4 &486426
Transform:
  m_ObjectHideFlags: 1
  m_PrefabParentObject: {fileID: 0}
  m_PrefabInternal: {fileID: 100100000}
  m_GameObject: {fileID: 165074}
  m_LocalRotation: {x: 0, y: 0, z: 0, w: 1}
  m_LocalPosition: {x: 0, y: 0, z: 0}
  m_LocalScale: {x: 1, y: 1, z: 1}
  m_LocalEulerAnglesHint: {x: 0, y: 0, z: 0}
  m_Children: []
  m_Father: {fileID: 0}
  m_RootOrder: 0
--- !u!23 &2355686
MeshRenderer:
  m_ObjectHideFlags: 1
  m_PrefabParentObject: {fileID: 0}
  m_PrefabInternal: {fileID: 100100000}
  m_GameObject: {fileID: 165074}
  m_Enabled: 1
  m_CastShadows: 1
  m_ReceiveShadows: 1
  m_Materials:
  - {fileID: 2100000, guid: 8fa36358c42035448bbbcf0ca0150e30, type: 2}
  m_SubsetIndices: 
  m_StaticBatchRoot: {fileID: 0}
  m_UseLightProbes: 1
  m_ReflectionProbeUsage: 1
  m_ProbeAnchor: {fileID: 0}
  m_ScaleInLightmap: 1
  m_PreserveUVs: 0
  m_IgnoreNormalsForChartDetection: 0
  m_ImportantGI: 0
  m_MinimumChartSize: 4
  m_AutoUVMaxDistance: 0.5
  m_AutoUVMaxAngle: 89
  m_LightmapParameters: {fileID: 0}
  m_SortingLayerID: 0
  m_SortingOrder: 0
--- !u!114 &11406988
MonoBehaviour:
  m_ObjectHideFlags: 1
  m_PrefabParentObject: {fileID: 0}
  m_PrefabInternal: {fileID: 100100000}
  m_GameObject: {fileID: 165074}
  m_Enabled: 1
  m_EditorHideFlags: 0
  m_Script: {fileID: 1682275622, guid: 563eda70411fb5f49834600794290898, type: 3}
  m_Name: 
  m_EditorClassIdentifier: 
--- !u!1001 &100100000
Prefab:
  m_ObjectHideFlags: 1
  serializedVersion: 2
  m_Modification:
    m_TransformParent: {fileID: 0}
    m_Modifications: []
    m_RemovedComponents: []
  m_ParentPrefab: {fileID: 0}
  m_RootGameObject: {fileID: 165074}
  m_IsPrefabParent: 1
```



	






