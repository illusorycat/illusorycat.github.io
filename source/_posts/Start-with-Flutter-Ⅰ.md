---
title: Flutter 入门指引 Ⅰ
date: 2024-05-20 16:55:02
tags: 代码人生
---
# 目录
[前言](#1)
[开发环境搭建](#2)
[后续章节概述](#3)

<h1 id="1">前言</h1>

本文基于掘金小册[《Flutter 入门教程》](https://juejin.cn/book/7212822723330834487)，是笔者学习了该小册后的记录。<br>
该记录仅作为知识记录，用于帮助笔者在日后快速回忆 Flutter 的使用。<br>
由于该小册内容较多，笔者在此将其输出为多篇文章。本文是第一篇，主要讲述 Flutter 开发环境的搭建，以及后续文章内容的概览。<br>

<h1 id="2">开发环境搭建</h1>

笔者的环境为 macmini 主机，系统为 macOS 13.6.4 。开发环境搭建主要遵从了 Flutter 的[指南](https://docs.flutter.dev/get-started/install)。目前笔者使用的开发环境是 MacOS 下的 iOS 开发环境和 Android 环境，Flutter 版本为 stable 渠道的 3.19.6 。
本文默认使用的 sheel为 zsh 。<br>

## iOS 环境
#### 开发工具
下载并安装 Xcode 15 和 CocoaPods 1.13 。在安装 Xcode 时一般会同步安装 Git 2.27 或更高版本，如果没有请手动安装。<br>
安装 Xcode 后最好打开并运行一个简单的程序，以使你安装相关的需要，如果真的有的话。<br>
安装 CocoaPods 很简单，运行下述语句即可:<br>
```shell
sudo gem install cocoapods
```
安装完成之后最好设置一下环境变量：<br>

- 打开 zsh 环境变量文件：
    ```shell
    vi ~/.zshenv
    ```
- 在末尾添加下述语句：
    ```shell
    export PATH=$HOME/.gem/bin:$PATH
    ```

#### 文本编辑器
笔者选用的是 VS Code 。<br>
在 VS Code 中增加扩展 **Flutter** 。当你安装该扩展时，会默认帮你一起安装 **Dart** 扩展，如果没有请手动安装。<br>
#### Flutter SDK

1. 在 VS Code 中打开命令面板（Command + shift + P）
2. 输入 flutter ，选择 Flutter: New Project

这时 VS Code 会提醒在本地找到 Flutter SDK ，你可以选择 **Download SDK** ，下载 SDK 到你想要保存到目录下。<br>
下载完成时会提示你是否添加 Flutter SDK 到环境变量中，请最好选择是。<br>
重启 VS Code 你就能看到 Flutter SDK 已经准备就绪。<br>
##### 配置 Xcode 
签署 Xcode 许可协议：<br>
```shell
sudo xcodebuild -license
```
现在，你可以在 iOS 模拟器或真机上运行 Flutter 项目了。<br>
## 安卓环境
安卓环境是在上述 iOS 环境搭建的基础上继续添加。<br>
#### 开发工具
下载并安装 Android Studio。启动 Android Studio 并安装提示操作。<br>
确保安装以下组件，如果在初始指引中没有安装的，可以后续在 Android Studio 中继续安装：<br>

- Android SDK Platform, API 34.0.0
- Android SDK Command-line Tools
- Android SDK Build-Tools
- Android SDK Platform-Tools
- Android Emulator

#### 配置 Android
运行以下指令启用签名许可证：<br>
```shell
flutter doctor --android-licenses
```
## 故障排查
在终端运行如下指令：<br>
```shell
flutter doctor
```
指令结果应该如下：<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405201921882.png)
#### 替换 maven 访问源
在国内访问 maven 总是很困难，所以最好是将 maven 的访问源换成国内镜像。<br>

1. 打开 Flutter 的安装目录/packages/flutter_tools/lib/src/ 下的 http_host_validator.dart 文件。<br>
    ![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405201925547.png)
2. 将其中 maven 的访问源改为你喜欢的国内镜像源，笔者使用的是国内阿里云的源：<br>
    ![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405201927774.png)
3. 删除 flutter 安装目录/bin/cache 文件夹
4. 再次执行 flutter doctor

事实上，除了 maven ，gradle 的访问也比较麻烦，但好在它只需要挂一下香港的网络节点一般都可以访问。<br>

<h1 id="3">后续章节概述</h1>

对于开发环境的搭建就讲到这了，后面会直接开始 Flutter 项目实践，需要读者至少有 Dart 语言的基础，可以去[此处](https://illusorycat.github.io/2024/05/20/Dart-brief/)了解。<br>
接下来的文章会分别讲述三个小项目，猜数字、电子木鱼以及白板绘制。在完成这三个项目后，我们还会将这三个项目结合起来，简单讲述一些进阶内容，比如数据持久化、网络交互等。<br>