---
layout: post
title:  "build cmake简单介绍"
categories: build
tags: build
---

* content
{:toc}

CMake是一个跨平台的安装(编译)工具,可以用简单的语句来描述所有平台的安装(编译过程)。他能够输
出各种各样的makefile或者project文件,能测试编译器所支持的C++特性,类似UNIX下的automake。CMake
的所有的语句都写在一个叫:CMakeLists.txt 的文件中。当CMakeLists.txt文件确定后,可以用ccmake命
令对相关 的变量值进行配置。这个命令必须指向CMakeLists.txt所在的目录。配置完成之后,应用cmake
命令生成相应的makefile（在Unix like系统下）或者 project文件（指定用window下的相应编程工具编
译时）。




## CMakeLists.txt剖析

### 注释

以#开头的行便是注释。

### cmake_minimum_required

cmake_minimum_required(VERSION 2.8) 指令标识CMake使用最低版本号。

### project

project(mame-xxx) 指令标识项目名称。

### 条件语句

在CMake中if语法比较简单，if后面括号中的参数随着CMake版本的推进，在
else和endif中也可以不用写了。

```
if(address)
    #TODO
else()
    #TODO
endif()
```
条件语句一般用在字符串的比较，常用两种情况，第一种：判断变量是否定义过，
有两种方式可以可以判断。

(1):
```
if(DEFINED address)
else()
endif()
```

(2):
```
if(address)
else()
endif()
```

两种都可以验证这个变量有没有定义过，注意：是定义过，比如你在CMake命令行中
随便写了-Daddress=xxx,就表示定义过了，对里面的值没有任何要求。

第二种：字符串的比较，也是比较常用的方式。

```
if(${address} STREQUAL "ON")
else()
endif()
```

这种就是比较字符串的值了，注意：在这个时候，你的脚本已经假设你对于address已
经有了 已被定义的默认要求 ，也就是说此时如果你没有定义过address变量，在正常
处理cmake ..的时候会报错，会提示无法识别的参数“STREQUAL”和"ON",这个需要特
别注意。

### option

指令标示为变量设置默认值，option(address "This is a option for address" ON)
此时表示，如果用户没有定义过address,那我address的默认值就是ON,如果用户在命令行
显示改变过address的值比如为OFF,那么在脚本中address的值就是OFF。有一个需要注意，
就是对于那些依赖于address有没有定义过的时候，如果你用了option你要特别注意，就
是在脚本处理过程中，如果你没有在命令行中显示定义过address，那么在option之前的
语句对于address都是没有定义过，直到到达了option,此时才真正定义了address.说起
来不好理解，给个例子：

```
if(address)
	message("defined address!!!!!!!!!!")
else()
	message("NOT defined address!!!!!!!!!")
endif()

option(address "hello world" ON)
message("option is ${address}")

if(address)
	message("defined address!!!!!!!!!!")
else()
	message("NOT defined address!!!!!!!!!")
endif()

结果：

NOT defined address!!!!!!!!
option is ON
defined address!!!!!!!!!!
```

### 变量

主要有隐式定义和显式定义两种,前面举了一个隐式定义的例子,就是 PROJECT 指令,
他会隐式的定义<projectname>_BINARY_DIR 和<projectname>_SOURCE_DIR 两个
变量。显式定义的例子我们前面也提到了,使用 SET 指令,就可以构建一个自定义变
量了SET(OBJ_NAME xxxx)，使用时${OBJ_NAME}。

### 案例

案例是xlua的编译文件，如果感兴趣可以直接去下载xlua的脚本编译进行测试。

```
cmake_minimum_required(VERSION 2.8)

if ( WIN32 AND NOT CYGWIN )
	set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} /MT" CACHE STRING "")
	set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} /MTd" CACHE STRING "")
	set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /MT" CACHE STRING "")
	set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MTd" CACHE STRING "")
endif ()

project(XLua)

option ( UINT_ESPECIALLY "using custom ulong" OFF )
option ( USING_LUAJIT "using luajit" OFF )

find_path(XLUA_PROJECT_DIR NAMES SConstruct
    PATHS 
    ${CMAKE_SOURCE_DIR}
    NO_DEFAULT_PATH
    )

MARK_AS_ADVANCED(XLUA_PROJECT_DIR)

if (USING_LUAJIT)
	
    if (APPLE AND NOT IOS)
	    include_directories(
		  ${CMAKE_SOURCE_DIR}
		  lua-5.1.5/src
		  lua-5.1.5/etc
		  ${CMAKE_CURRENT_BINARY_DIR}
		  ${THIRDPART_INC}
	    )
	    set ( LUA_CORE lua-5.1.5/src/lapi.c lua-5.1.5/src/lcode.c lua-5.1.5/src/ldebug.c lua-5.1.5/src/ldo.c lua-5.1.5/src/ldump.c lua-5.1.5/src/lfunc.c 
          lua-5.1.5/src/lgc.c lua-5.1.5/src/llex.c lua-5.1.5/src/lmem.c lua-5.1.5/src/lobject.c lua-5.1.5/src/lopcodes.c lua-5.1.5/src/lparser.c lua-5.1.5/src/lstate.c 
          lua-5.1.5/src/lstring.c lua-5.1.5/src/ltable.c lua-5.1.5/src/ltm.c lua-5.1.5/src/lundump.c lua-5.1.5/src/lvm.c lua-5.1.5/src/lzio.c lua-5.1.5/src/lauxlib.c 
          lua-5.1.5/src/lbaselib.c lua-5.1.5/src/ldblib.c lua-5.1.5/src/liolib.c lua-5.1.5/src/lmathlib.c lua-5.1.5/src/loslib.c lua-5.1.5/src/ltablib.c 
          lua-5.1.5/src/lstrlib.c lua-5.1.5/src/linit.c lua-5.1.5/src/loadlib.c)
	else ()
	    include_directories(
		  ${CMAKE_SOURCE_DIR}
		  luajit-2.1.0b2/src
		  ${CMAKE_CURRENT_BINARY_DIR}
		  ${THIRDPART_INC}
	    )

	    set ( LUA_CORE )
    endif ()
	set ( LUA_LIB )
else ()
	set ( LUA_IDSIZE 120 CACHE NUMBER "gives the maximum size for the description of the source." )

	configure_file ( lua-5.3.3/src/luaconf.h.in ${CMAKE_CURRENT_BINARY_DIR}/luaconf.h )

	include_directories(
	    ${CMAKE_SOURCE_DIR}
		lua-5.3.3/src
		${CMAKE_CURRENT_BINARY_DIR}
		${THIRDPART_INC}
	)

	set ( LUA_CORE lua-5.3.3/src/lapi.c lua-5.3.3/src/lcode.c lua-5.3.3/src/lctype.c lua-5.3.3/src/ldebug.c lua-5.3.3/src/ldo.c lua-5.3.3/src/ldump.c
	  lua-5.3.3/src/lfunc.c lua-5.3.3/src/lgc.c lua-5.3.3/src/llex.c lua-5.3.3/src/lmem.c lua-5.3.3/src/lobject.c lua-5.3.3/src/lopcodes.c lua-5.3.3/src/lparser.c
	  lua-5.3.3/src/lstate.c lua-5.3.3/src/lstring.c lua-5.3.3/src/ltable.c lua-5.3.3/src/ltm.c lua-5.3.3/src/lundump.c lua-5.3.3/src/lvm.c lua-5.3.3/src/lzio.c )
	set ( LUA_LIB lua-5.3.3/src/lauxlib.c lua-5.3.3/src/lbaselib.c lua-5.3.3/src/lbitlib.c lua-5.3.3/src/lcorolib.c lua-5.3.3/src/ldblib.c
	  lua-5.3.3/src/liolib.c lua-5.3.3/src/lmathlib.c lua-5.3.3/src/loslib.c lua-5.3.3/src/lstrlib.c lua-5.3.3/src/ltablib.c lua-5.3.3/src/linit.c
	  lua-5.3.3/src/lutf8lib.c lua-5.3.3/src/loadlib.c )
endif ()

set ( LUA_SOCKET
    luasocket/auxiliar.c
    luasocket/buffer.c
    luasocket/except.c
    luasocket/inet.c
    luasocket/io.c
    luasocket/luasocket.c
    luasocket/mime.c
    luasocket/options.c
    luasocket/select.c
    luasocket/tcp.c
    luasocket/timeout.c
    luasocket/udp.c 
)

if ( WIN32 AND NOT CYGWIN )
    list (APPEND LUA_SOCKET luasocket/wsocket.c)
else ( )
    list (APPEND LUA_SOCKET luasocket/usocket.c)
endif ( )

set ( XLUA_CORE
    i64lib.c
    perflib.c
    xlua.c
)

if (APPLE)
    if (IOS)
        set(CMAKE_OSX_ARCHITECTURES "$(ARCHS_STANDARD)")
        add_library(xlua STATIC
           ${LUA_CORE}
           ${LUA_LIB}
           ${LUA_SOCKET}
           ${XLUA_CORE}
           ${THIRDPART_SRC}
        )
    else ()
        set(CMAKE_OSX_ARCHITECTURES "$(ARCHS_STANDARD_32_64_BIT)")
        add_library(xlua MODULE
            ${LUA_CORE}
            ${LUA_LIB}
            ${LUA_SOCKET}
            ${XLUA_CORE}
            ${THIRDPART_SRC}
        )
        set_target_properties ( xlua PROPERTIES BUNDLE TRUE )
        #set_target_properties ( xlua PROPERTIES FRAMEWORK TRUE )
        #set_target_properties ( xlua PROPERTIES MACOSX_RPATH TRUE )
    endif ()
else ( )
    add_library(xlua SHARED
        ${LUA_CORE}
        ${LUA_LIB}
        ${LUA_SOCKET}
        ${XLUA_CORE}
        ${THIRDPART_SRC}
    )
endif ( )

if ( WIN32 AND NOT CYGWIN )
    target_compile_definitions (xlua PRIVATE LUA_BUILD_AS_DLL)
endif ( )

set_property(
	SOURCE ${LUA_SOCKET}
	APPEND
	PROPERTY COMPILE_DEFINITIONS
	LUA_COMPAT_APIINTCASTS LUA_LIB
)

if(UINT_ESPECIALLY)
    ADD_DEFINITIONS(-DUINT_ESPECIALLY)
endif()
	
if ( WIN32 AND NOT CYGWIN )
    if (USING_LUAJIT)
        target_link_libraries(xlua
            ${CMAKE_SOURCE_DIR}/luajit-2.1.0b2/src/lua51.lib
            ws2_32
            ${THIRDPART_LIB} 
        )
    else ()
        target_link_libraries(xlua
            ws2_32
            ${THIRDPART_LIB} 
        )
    endif()
else ( )
    if (USING_LUAJIT AND NOT APPLE)
		target_link_libraries(xlua
			${CMAKE_SOURCE_DIR}/luajit-2.1.0b2/src/libluajit.a
			m
            ${THIRDPART_LIB} 
		)
    else ()
        target_link_libraries(xlua
            m
            ${THIRDPART_LIB} 
        )
    endif()
endif ( )
```

## 参考资料

[cmake github](https://github.com/Kitware/CMake)

[cmake官网](https://cmake.org/)



	






