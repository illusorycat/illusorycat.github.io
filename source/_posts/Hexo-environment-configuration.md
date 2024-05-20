---
title: Hexo 环境配置
date: 2024-05-20 09:13:17
tags: 工具
---
# 目录
[前言](#1)
[本地环境](#2)
[云端环境](#3)

<h1 id="1">前言</h1>

本文记录笔记使用 Hexo 搭建个人博客网站的过程。<br>

<h1 id="2">本地环境</h1>

## 运行环境
本地需要先安装 Git 和 Node.js ，具体安装可以参考[此文档](https://hexo.io/docs/)。
前置环境安装完成后后，执行下述语句安装 Hexo ：<br>
```shell
sudo npm install -g hexo-cli
```
如果有遇到没有文件夹访问权限问题，可以使用下述语句：<br>
```shell
sudo chown -R `whiami` path
```
## 本地博客
使用下述指令创建博客工作空间：<br>
```shell
hexo init <folder>
```
一个比较常见的博客工作空间结构如下：<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405202026908.png)
创建完成后，我们可以修改 _config.yml 文件中的内容来简单的自定义我们的博客网站。<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405202023026.png)
比较主要的字段是 **title** 和 **author** 。**language** 字段是在语言国际性下标识默认语言。**timezone**字段会自动识别，如果需要也可以静态固定。<br>
source 文件夹一般用于存放文章， **_posts** 存放要发表的文章， **_drafts** 存放草稿。<br>
themes 文件夹存放我们需要的主体，可以在[官网](https://hexo.io/themes/)查找需要的主题然后应用到自己的博客网站上。但是一般刚搭建时不推荐大家去过多的关注主题，而是先完成网站搭建并运作起来。<br>
使用下述语句创建新文章，默认会存放在 _posts 文件夹中：<br>
```shell
hexo new "article name"
```
使用下述指定编译并本地运行博客网站：<br>
```shell
hexo clean
hexo g
hexo server
```
然后就可以通过 `http://localhost:4000/` 访问。<br>

<h1 id="3">云端环境</h1>

搭建个人博客网站肯定是希望被别人看见，那我们就需要把博客站点部署在云端，才可以让别人通过浏览器访问到。<br>
笔者选择了在 Github Pags 部署，方便且快捷。<br>
## github 账户设置
你需要有一个 [github](https://github.com) 账户。
接着你需要创建一个私人访问 Token ，如果你已经有就不需要重新创建。<br>
点击 github 网站右上角的头像，选择 "Settings" 进入设置页。然后在侧边栏找到 "Developer settings" 进入开发者设置页。最后选择创建私人 Token(classic) 。<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202403201411328.png)<br>
设置 Token 的过期时间时可以尽可能的延长它或者直接选择不过期。勾选下面的 "repo" 项。最后点击最下方的 "Generate Token" 按键即可。<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202403201413750.png)<br>
然后你就可以看到你生成的私人 Token ，请确保不要让其他人知晓它，在离开这个界面之前复制并保存它，否则你将找不到它。<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202403201416467.png)<br>
## github 仓库
假设你到 GitHub 账户名为 myName ，你需要新建一个 Github 仓库，仓库名为 `myName.github.io`。勾选添加 README 文件。<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405202042714.png)
在本地进入你的博客工作空间，然后将github 仓库克隆到此空间：<br>
```shell
cd myBlog/
git clone https://github.com/myName/myName.github.io.git ./
```
`https://github.com/myName/myName.github.io.git` 是 github 上创建的仓库路径，`./` 是表示克隆到当前目录，也就是博客工作空间文件夹。<br>
在第一次执行 git 指令时，一般会让你输入相关信息，输入你的 github 账户名称和邮箱，已经上述提到的私人 Token 即可。如果你需要一个可视化操作界面，也可以直接下载 github Desktop 客户端。<br>
执行下述语句编译博客网站：<br>
```shell
hexo clean
hexo g
```
执行下述语句将修改提交到 github 上：<br>
```shell
git add .
git commit -m "first modify"
git push
```
这样子你的修改就上传到 github 了。然后你就可以通过 myName.github.io 访问到你的博客网站了。默认一开始就有一篇 hello 文章在其中。<br>
之后只需要在 _posts 文件夹中撰写新的文章，然后重新上述流程上传即可。<br>