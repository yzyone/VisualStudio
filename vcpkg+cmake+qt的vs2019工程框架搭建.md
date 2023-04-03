# 【vcpkg+cmake+qt的vs2019工程框架搭建】

vcpkg+cmake+qt的vs2019工程框架搭建

## 概要

cmake工程已经应用非常普及，vcpkg可以很方便的应用第三方库，省去了自己编译第三方库的时间，本文档说明基于cmake+qt+vcpkg的多项目结构搭建。

## 项目目录结构

|—games #游戏集根目录
cmakefilelist.txt #根cmake文件
|—build #cmake生成vs2019的工程目录
|—source #源代码根目录， 可以包含多个独立的游戏
cmakefilelist.txt #源代码cmake文件
|—coin #翻金币游戏
cmakefilelist.txt #翻金币cmake文件
|—snake # 贪吃蛇游戏

## 各个cmakefilelist文件说明

**根cmakefilelist.txt**

```
#project games
cmake_minimum_required(VERSION 3.17.0)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

#游戏根项目
project(games)

message(STATUS "Target processor: ${CMAKE_SYSTEM_PROCESSOR}")
message(STATUS "Host processor: ${CMAKE_HOST_SYSTEM_PROCESSOR}")

if (WIN32)
find_package(Qt5 REQUIRED Core Gui Network PrintSupport Widgets Xml)
find_package(Qt5WinExtras REQUIRED)

#find_package(Qt5LinguistTools)
endif()

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

#源码目录
add_subdirectory(source)
```

**source源码目录cmakefilelist.txt**

```
include(GNUInstallDirs)


# Qt configuration.
set(CMAKE_INCLUDE_CURRENT_DIR ON)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
list(APPEND CMAKE_AUTORCC_OPTIONS -compress 9 -threshold 5)


if (WIN32)
    # Target version.
    add_definitions(-DNTDDI_VERSION=0x06010000
                    -D_WIN32_WINNT=0x0601
                    -D_WIN32_WINDOWS=_WIN32_WINNT
                    -DWINVER=_WIN32_WINNT
                    -D_WIN32_IE=0x0800
                    -DPSAPI_VERSION=2)

    # Other definitions.
    add_definitions(-D_UNICODE
                    -DUNICODE
                    -DWIN32_LEAN_AND_MEAN
                    -DNOMINMAX)
endif()

# For Qt.
add_definitions(-DQT_NO_CAST_TO_ASCII
                -DQT_NO_CAST_FROM_BYTEARRAY
                -DQT_USE_QSTRINGBUILDER)

#QT基本库
set(QT_COMMON_LIBS
    Qt5::Core
    Qt5::CorePrivate
    Qt5::QGenericEnginePlugin
    Qt5::Gui
    Qt5::GuiPrivate
    Qt5::QICOPlugin
    Qt5::Network
    Qt5::NetworkPrivate
    Qt5::PrintSupport
    Qt5::PrintSupportPrivate
    Qt5::Widgets
    Qt5::WidgetsPrivate
    Qt5::Xml
	#Qt5::Sql
    Qt5::XmlPrivate)

#QT windows平台库
if (WIN32)
    set(QT_PLATFORM_LIBS
        Qt5::WinMain
        Qt5::WinExtras
        Qt5::WinExtrasPrivate
        Qt5::QWindowsIntegrationPlugin
        Qt5::QWindowsVistaStylePlugin
        Qt5::QWindowsPrinterSupportPlugin)
endif()

#source目录 include 路径
include_directories(${PROJECT_SOURCE_DIR}/source ${PROJECT_BINARY_DIR}/source)

# C++ 17标准 compliller flags.
set(CMAKE_CXX_STANDARD 17)

if (MSVC)
    # C++ compliller flags.
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /W3 /MP /arch:SSE2")
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MP /W3")

    # C compiller flags.
    set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} /W3 /MP /arch:SSE2")
    set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} /W3 /MP")
    
    # Linker flags.
    set(CMAKE_SHARED_LINKER_FLAGS_RELEASE "${CMAKE_SHARED_LINKER_FLAGS_RELEASE} /LTCG /INCREMENTAL:NO /OPT:REF /IGNORE:4099")
    set(CMAKE_STATIC_LINKER_FLAGS_RELEASE "${CMAKE_STATIC_LINKER_FLAGS_RELEASE} /LTCG")
    set(CMAKE_EXE_LINKER_FLAGS_RELEASE "${CMAKE_EXE_LINKER_FLAGS_RELEASE} /LTCG /INCREMENTAL:NO /OPT:REF /IGNORE:4099")
    
    # Static runtime library.
    set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
endif()

include(CheckIPOSupported)
check_ipo_supported(RESULT IPO_SUPPORTED OUTPUT error)
if (IPO_SUPPORTED)
    message(STATUS "IPO/LTO supported")

    # Enable link-time code generatation.
    set(CMAKE_INTERPROCEDURAL_OPTIMIZATION TRUE)
else()
    message(STATUS "IPO/LTO not supported")
endif()

# Compiller flags.
message(STATUS "CXX compiller version: ${CMAKE_CXX_COMPILER_VERSION}")
message(STATUS "CXX flags (release): ${CMAKE_CXX_FLAGS_RELEASE}")
message(STATUS "CXX flags (debug): ${CMAKE_CXX_FLAGS_DEBUG}")
message(STATUS "C compiller version: ${CMAKE_C_COMPILER_VERSION}")
message(STATUS "C flags (release): ${CMAKE_C_FLAGS_RELEASE}")
message(STATUS "C flags (debug): ${CMAKE_C_FLAGS_DEBUG}")

# Linker flags.
message(STATUS "Linker flags (shared release): ${CMAKE_SHARED_LINKER_FLAGS_RELEASE}")
message(STATUS "Linker flags (shared debug): ${CMAKE_SHARED_LINKER_FLAGS_DEBUG}")
message(STATUS "Linker flags (static release): ${CMAKE_STATIC_LINKER_FLAGS_RELEASE}")
message(STATUS "Linker flags (static debug): ${CMAKE_STATIC_LINKER_FLAGS_DEBUG}")
message(STATUS "Linker flags (exe release): ${CMAKE_EXE_LINKER_FLAGS_RELEASE}")
message(STATUS "Linker flags (exe debug): ${CMAKE_EXE_LINKER_FLAGS_DEBUG}")

#子项目, 可以多个项目
add_subdirectory(coin)
#add_subdirectory(snake)
```
**coin翻金币游戏 cmakefilelist.txt**

```
#coin.exe

#翻金币主程序
list(APPEND SOURCE_COIN
    coin.rc					#程序版本, main icon等
    main.cc					#主入口
	main_window.h			#主窗口
	main_window.cc
	main_window.ui			#主窗口UI
	)
	
#QT的资源文件, png图片, 音频等资源
list(APPEND SOURCE_COIN_RESOURCES
	res.qrc
	)
	
#文件分目录
source_group("" FILES ${SOURCE_COIN})					#根目录
source_group(moc FILES ${SOURCE_COIN_MOC})
source_group(resources FILES ${SOURCE_COIN_RESOURCES})

#生成执行文件
add_executable(coin MACOSX_BUNDLE
    ${COIN_ICON}					#ICON资源
	${SOURCE_COIN_RESOURCES}		#png图片资源
    ${SOURCE_COIN}					#源代码
	)
	
if (WIN32)
    set_target_properties(coin PROPERTIES WIN32_EXECUTABLE TRUE)
    set_target_properties(coin PROPERTIES LINK_FLAGS "/MANIFEST:NO")
endif()

#链接QT库
target_link_libraries(coin    
    ${QT_COMMON_LIBS}
    ${QT_PLATFORM_LIBS}
	)	

#需要设置 AUTOMOC, 否则导致编译成功，链接失败
set_property(TARGET coin PROPERTY AUTOMOC ON)
set_property(TARGET coin PROPERTY AUTOUIC ON)
set_property(TARGET coin PROPERTY AUTORCC ON)
```
**安装第三方库管理工具vcpkg**

略，自行baidu

**vcpkg安装qt相关包**

vcpkg install qt5-base:x64-windows-static
vcpkg install qt5-translations:x64-windows-static
vcpkg install qt5-winextras:x64-windows-static

**在build目录中生成vs2019工程**

cmake … -DCMAKE_TOOLCHAIN_FILE=C:\vcpkg\scripts\buildsystems\vcpkg.cmake -DVCPKG_TARGET_TRIPLET=x64-windows-static

**vs2019打开工程文件编译 —done**

————————————————

版权声明：本文为CSDN博主「qq_10654947」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/qq_30812427/article/details/125185692