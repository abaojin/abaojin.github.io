---
layout: post
title:  "unity 项目优化"
categories: unity
tags: unity
---

* content
{:toc}

# 工程结构

工程美术资源、前端框架、脚本资源、插件资源还乱，开发调试及查找文件复杂。

 * 内容
    + 重构项目框架结构，在原有基础上修改。
    + 重新整理美术资源结构，优化之前美术资源规范。
    + 前端脚本代码结构。

* 具体方案

    * 总体结构
        + Engine 客户端框架
            * Framework 客户端框架
            * Framework.Editor 客户端框架编辑器部分 
        + Project 客户端工程
            * ProjectName 程序客户端工程
            * ProjectNameArt 美术客户端工程
            * Shared 共享对象
                *  Build 构建版本和资源
                    * Client 客户端构建生成目录
                    * Publish 发布资源和版本脚本（Python 3.6.2）
                    * Server 服务器构建生成目录 
                *  Conf 项目配置资源
                    * message 协议文件
                    * config 配配置文件（表等）
                *  Tool 基本工程（生成协议和配表运行文件）
            * Tool 工具工程

    * 框架Framework结构
        + 3rd（框架依赖第三方插件）
        + Base（基础模块）
            * Attribute（属性定义）
            * Buffer（流操作）
            * Component（常用组件）
            * Const（常量定义）
            * Entry（框架入口）
            * EventPool（事件池）
            * Entension（类工程扩展模块）
            * Helper（常用辅助模块）
            * Object（常用对象定义）
            * Singleton（单利模块）
            * Protocol （消息协议）
            * TaskPool（任务池模块）
            * Variable（默认类型包裹）
            * Utils（辅助函数）
        + Config（配置模块）
        + Debugger（调试模块）
        + Download（资源下载模块）
        + Event（事件模块）
        + Http（Http模块）
        + Localization（国际化模块）
        + Log（日志输出模块）
        + Network（网络层TCP模块）
        + Pool（对象池模块）
        + Procedure（流程管理模块）
        + PureMVC（MVC模块）
        + Resource（资源加载模块）
        + Scene（场景模块）
        + Setting（客户端设置模块）
        + Sound（音频模块）
        + UI（界面模块）

    * 框架Framework.Editor结构
        + Base （编辑器基础代码）
            + Config （编辑器配置）
            + Coroutine （编辑器模式下的携程）
            + Helper （编辑器一些辅助函数，结构不太合理）
            + Utils （常见的编辑器辅助代码）
        + Base（基础模块）
            + Application （应用构建-正在优化）
            + AssetBundle （资源打包功能）
            + AssetBundleBrowser （资源预览工具）
            + Folder （目录辅助代码）
            + Inspector （脚本空间面显示）
            + LineArgs （启动Unity命令参数解析工具）
            + Scene （场景相关编辑器工具）
            + Standard （项目标准定义或检查工具）
            + UGUI （ugui扩展脚本）
            + Unity （引擎相关编辑器代码）

    * 具体项目结构
        + 3rd-第三方插件目录
            * Demigiant 缓动插件
            * MeshAnimator  Mesh动画插件
            * PostProcessing 后期处理插件
            * AssetBundle-Browser 打包资源预览插件

        + Arts-美术资源目录
            * Atlas 游戏图集目录，主要为UI图集
            * Audio 音频资源目录
            * Config 项目配置文件目录
                *  Editor 编辑器插件（位置应该动态指定）
                *  Load 预加载配置
                *  Localization 国际化配置
                *  Tab 配表Tab
                *  Unit 角色配置
            * Effect 项目特效资源目录
            * Font 字体资源目录
            * Level 关卡或场景资源目录
                * Battle_NameXXX 战斗场景
                * Login 登陆初始化场景
                * CreatePlayer 创建角色场景
                * Map 瓦片大世界初始化场景
            * Shader 渲染脚本资源
            * Unit 显示单元目录
                * Hero 英雄角色资源目录
                * Map 大地图相关资源
                * Soldier 士兵角色资源目录
            * Window 界面资源目录
        + Editor Default Resources -默认标记器资源目录

        + Plugins-插件资源目录
            * Framework 项目框架插件
            * Manager 托管插件目录
            * Native 系统原生插件目录
                * iOS 苹果手机系统原生插件
                * Android 安卓系统原生插件
                * OSX Mac系统原生插件
        + Launcher 游戏初始化场景，启动游戏

        + Scripts
            * Auto 自动生成代码目录（目前包括消息和表格）
            * Base 项目基础代码目录（辅助代码，辅助模块）
            * Editor 项目编辑器代码目录
            * Logic 项目逻辑代码目录（请勿将基础代码放在该目录）
                * Map 大地图核模块
                * CreatePlayer 创建角色模块
                * Building 建筑模块

        + Standard Assets-标准资源目录

        + StreamingAssets-流资源包输出目录
 

# 功能优化

## 逻辑优化

* 网络模块

    网络层消息队列分级处理。

* 界面模块

    梳理界面界面开发流程，界面开发不高效，且缺少通用型组件。

* 资源管理

    资源加载优化，支持bundle包的动态加载和卸载。

* 配置数据模块

    统一整理游戏配置，修改数据初始化时机，部分数据结构调整。

## 工具优化

* 日志系统
* 资源打包
* 资源规格检查工具
* 整理目前游戏存在工具（常见网格编辑，动画编辑，界面导出，配置生成等）。

# 规范优化

* 程序编写上规范（编码，注释，结构，注意事项等）。
* 美术资源制作上规范 （制作方式，目录结构，命名规范等）
* 