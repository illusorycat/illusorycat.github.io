---
title: Flutter 入门指引 Ⅱ
date: 2024-05-23 15:55:16
tags:
---
# 目录
[前言](#1)
[界面交互与需求分析](#2)
[静态界面构建](#3)
[状态数据与界面更新](#4)
[动画](#5)

<h1 id="1">前言</h1>

本文基于掘金小册[《Flutter 入门教程》](https://juejin.cn/book/7212822723330834487)，是笔者学习了该小册后的记录。<br>
该记录仅作为知识记录，用于帮助笔者在日后快速回忆 Flutter 的使用。<br>
由于该小册内容较多，笔者在此将其输出为多篇文章。本文是第二篇，介绍第一个实践项目，猜数字。<br>

<h1 id="2">界面交互与需求分析</h1>

- 点击按钮生成 0～99 的随机数，并且将随机数以密文格式显示
- 头部的输入框，点击时弹出软键盘，可输入猜测的数字

<table>
    <tr>
        <th>点击生成随机数</th>
        <th>可输入文字</th>
    </tr>
    <tr>
        <td><img src="https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405231630336.awebp">
        <td><img src="https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405231712870.awebp">
    </tr>
</table>

- 点击右上角的运行按键，可以比较输入值与生成值，并给出动画提示。

<table>
    <tr>
        <th>比较结果：小了</th>
        <th>比较结果：大了</th>
        <th>比较结果：相等</th>
    </tr>
    <tr>
        <td><img src="https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405231746217.awebp">
        <td><img src="https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405231747405.awebp">
        <td><img src="https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405231747218.awebp">
    </tr>
</table>
