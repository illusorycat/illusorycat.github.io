---
title: Windows 下编译 FFMPEG
date: 2024-05-20 09:19:06
tags: 源码
---
# 目录
[前言](#1)
[Windows 下安装编译 FFMPEG](#2)

<h1 id="1">前言</h1>

本教程旨在记录自己源码编译安装 **FFMPEG** 的过程。<br>
如果没有修改源码需求，笔者还是更建议直接从[官网推荐途径](https://ffmpeg.org/download.html)或者其他可信途径下载二进制文件后使用。<br>

<h1 id="2">Windows 下安装编译 FFMPEG</h1>

首先我们要准备好编译环境，笔者这儿准备的是一台装有 Windows10 系统电脑，同时安装了 Visual Studio 2019。<br>
在 Windows 下编译 FFMPEG 我们还需要借助 MSYS2 ，可以[在此下载](https://www.msys2.org/)此软件。我们会在 MSYS2 环境中编译 FFMPEG ，编译时使用的编译器( cl.exe ) 和链接器( link.exe ) 则是由 VS2019 提供的。也就是说，我们使用的是 MSYS2 + MSVC 的方案。<br>
### 下载源码
编译环境准备好之后，我们下载一份 FFMPEG 源码，你可以是直接下载[最新版本](https://git.ffmpeg.org/ffmpeg.git)，也可以从官网下载[指定版本](https://ffmpeg.org/download.html)。<br>
源码下载后需要修改源码，将其中包含 CC_IDENT 宏的代码注释掉。否则会在编译时遇到如下错误。<br>

    error C2296: “%”: 非法，左操作数包含“char [138]”类型

源码中最好不要包含任何中文字符，否则中编译时会出现很多如下警告：<br>

     warning C4828: 文件包含在偏移 0x227 处开始的字符，该字符在当前源字符集中无效(代码页 65001)。

这同样意味着，你要删除 CC_IDENT 这个宏定义，因为其包含中文字符。<br>
### 编译
在打开 MSYS2 前，我们先进入它的安装目录，修改 **msys2_shell.cmd** 文件，将文件中第**17**行代码打开，即去掉 **rem** 关键字，如下所示：<br>

    @echo off
    setlocal EnableDelayedExpansion

    set "WD=%__CD__%"
    if NOT EXIST "%WD%msys-2.0.dll" set "WD=%~dp0usr\bin\"
    set "LOGINSHELL=bash"
    set /a msys2_shiftCounter=0

    rem To activate windows native symlinks uncomment next line
    rem set MSYS=winsymlinks:nativestrict

    rem Set debugging program for errors
    rem set MSYS=error_start:%WD%../../mingw64/bin/qtcreator.exe^|-debug^|^<process-id^>

    rem To export full current PATH from environment into MSYS2 use '-use-full-path' parameter
    rem or uncomment next line
    set MSYS2_PATH_TYPE=inherit
    ...

打开后， MSYS2 可以继承 Windows 控制台的环境变量。
之后，打开 `x64 Native Tools Command Prompt for VS 2019` 命令窗口，可以通过如下方式找到：<br>

    Windows 开始菜单 -> Visual Studio 2019 -> x64 Native Tools Command Prompt for VS 2019

然后在该命令窗口中输入下面的命令启动 MSYS2 软件：<br>

    #切换盘符，如果需要的话
    #d:

    #进入 MSYS2 的安装目录
    cd D:\Application\msys64

    #启动 MSYS2
    msys2_shell.cmd

此时会弹出 MSYS2 的命令窗口，在窗口中输入下面命令，安装必要的编译工具：<br>

    pacman -S diffutils make pkg-config yasm

当编译工具安装好后，在 MSYS2 命令窗口进入 FFMPEG 源码目录：<br>

    cd /d/WorkSpace/FFMPEG/ffmpeg-4.3.6

我的 FFMPEG 源码路径是 D:/WorkSpace/FFMPEG/ffmpeg-4.3.6。<br>
紧接着，运行 FFMPEG 源码目录中的 `configure` 脚本生成 **Makefile** 文件，如下：<br>

    ./configure --prefix=/usr/local/ffmpeg --arch=x86_64 --disable-amd3dnow --disable-amd3dnowext --enable-gpl --enable-nonfree --enable-shared --disable-doc --disable-postproc --disable-avdevice --toolchain=msvc

上述命令的含义是使用 **msvc** 作为 FFMPEG 的编译工具链；编译安装的 FFMPEG 库被放到 `/usr/local/ffmpeg` 目录下，该目录是 MSYS2 环境下的目录；编译的是动态库。<br>
至于其他的构建选项是基于笔者的需求，可以运行 `./configure --help` 查看支持哪些构建选项。<br>
上述脚本执行完成后，中 FFMPEG 源码目录下会多一个 Makefile 文件，然后我们就可以编译安装 FFMPEG 了，执行如下命令：<br>

    make -j 8 && make install

因为我们没有安装其他第三方库，因此会有一些内容是没有编译的，比如 ffplay.exe ，因为没有 SDL 库。有需要的话可以自行安装。<br>
同时我们可以在执行 ffmpeg.exe 程序时看到我们的构建脚本，也就是说我们也可以运行其他途径提供的二进制程序来查看它们的构建脚本。<br>