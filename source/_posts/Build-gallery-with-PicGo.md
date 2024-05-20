---
title: 使用 PicGo 搭建图床
date: 2024-05-20 09:10:55
tags: 工具
---
# 目录
[前言](#1)
[图床](#2)
[搭建图床](#3)

<h1 id="1">前言</h1>

本教程用于记录笔者使用 **PicGo** 工具创建图床的过程。<br>

<h1 id="2">图床</h1>

图床其实就是在互联网上存储图片的一块空间，你可以把它理解为一个在线数据库，其中的数据是图片。我们可以通过一个 URL 访问图床中的图片。<br>
### 图床工具
图床工具主要是方便人们创建搭建图床，比如笔者使用的图床工具是 [PicGo](https://github.com/Molunerfinn/PicGo) 。<br>
PicGo 支持多个图床服务，比如七牛图床、腾讯云、Github 等。笔者选用的是 Github 。<br>

<h1 id="3">搭建图床</h1>

### Github 仓库
首先我们要在 Github 上创建一个图床仓库。<br>
在 [Github](https://github.com/) 上登陆自己的账户。然后点击右上角的加号( `+` )创建一个新的仓库，如下:<br>
![](https://github.com/illusorycat/MyPictureBase/blob/main/image/202403201009405.png?raw=true)<br>
在 Repository name * 输入框中填写图床的名称，比如笔者使用的是 **MyPictureBase** 。仓库权限选择 Public 。下面的 **Add a README file** 是可选项，选择了 Github 就会根据你的仓库描述自动生成一个 README 文件。然后点击 **Create repository** 按键创建仓库即可。<br>
![](https://github.com/illusorycat/MyPictureBase/blob/main/image/202403201011304.png?raw=true)<br>
#### 创建私人 Token
点击右上角的头像，选择 "Settings" 进入设置页。然后在侧边栏找到 "Developer settings" 进入开发者设置页。最后选择创建私人 Token(classic) 。<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202403201411328.png)<br>
设置 Token 的过期时间时可以尽可能的延长它或者直接选择不过期。勾选下面的 "repo" 项。最后点击最下方的 "Generate Token" 按键即可。<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202403201413750.png)<br>
然后你就可以看到你生成的私人 Token ，请确保不要让其他人知晓它，在离开这个界面之前复制并保存它，否则你将找不到它。<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202403201416467.png)<br>
### PicGo 工具安装使用
PicGo 工具笔者是直接在 Github 上下载作者生成的 release 文件，具体下载链接可[点击这儿](https://github.com/Molunerfinn/PicGo/releases)。<br>
笔者选用的是最新的稳定版本 2.3.1 正式版，同时笔者是在 MacOS 平台下使用的，所以下载了 PicGo-2.3.1-arm64.dmg 。<br>
#### 安装
下载之后直接双击打开就可以看到 PicGo 软件的可执行文件，将其拽拖到应用文件目录即可。<br>
但是，由于苹果的安全验证机制，并且 PicGo 并没有执行苹果要求的签证机制，因此我们一开始是无法打开的。<br>
![](https://github.com/illusorycat/MyPictureBase/blob/main/image/202403201028811.png?raw=true)<br>
从 PicGo 作者提供的 [FAQ](https://github.com/Molunerfinn/PicGo/blob/dev/FAQ.md) 中我们可以找到此问题的解决方案。<br>
打开终端并执行下述指令:<br>

1. 临时禁用系统完整性保护
    <code>sudo spctl --master-disable</code>
2. 手动放行 PicGo
    <code>xattr -cr /Applications/PicGo.app</code>

禁用系统完整性保护后，我们可以在系统设置中的隐私和安全中看到。<br>
![](https://github.com/illusorycat/MyPictureBase/blob/main/image/202403201036870.png?raw=true)<br>
#### 使用
PicGo 软件打开后会最小化到 MacOS 系统右上角。点击右上角图标 <img src="https://github.com/illusorycat/MyPictureBase/blob/main/image/202403201150958.png?raw=true" style="width:30px; height:auto;"></img> 打开软件，软件主界面如下。<br>
![](https://github.com/illusorycat/MyPictureBase/blob/main/image/202403201159299.png?raw=true)<br>
点击图床设置，选择 Github ，填写相应的信息并确认设置。<br>
![](https://github.com/illusorycat/MyPictureBase/blob/main/image/202403201354641.png?raw=true)<br>
仓库名称一般是`Github用户名/仓库名`，比如笔者是 `illusorycat/MyPictureBase`。<br>
分支名显而易见，比如笔者是 `main`。
Token 是在 GitHub 上创建的私人 token 。<br>
存储路径可不填。<br>
自定义域名也可不填。<br>
确认上述信息并将 Github 设置为默认图床即可开始上传了。