---
layout: post
title: 使用Markdown语法编写博客内容
date: 2024-07-08
Author: 阎子君
categories: Markdown
tags: [Github, Markdown]
comments: true
--- 

**本中文版Markdown语法简介来自博主LOFFER的博客[Markdown语法简介](https://fromendworld.github.io/LOFFER/chinese-markdown-cheatsheet/#%E4%BB%A3%E7%A0%81%E5%9D%97/"LOFFER's Blog").**

**[Mrakdown完整文档](https://daringfireball.net/projects/markdown/syntax/"Mrakdown完整文档"),整理本文为了写博客时作为参考。**

## 文章开头

    ---
    layout: post
    title: 使用Markdown语法编写博客内容
    date: 2024-07-08
    Author: 阎子君
    categories: Markdown
    tags: [Github, Markdown]
    comments: true
    ---

## 标题

    #一级标题
    ##二级标题
    ...
    ######六级标题

## 分段与分行

以一个或多个空行来隔开段落；以两个或多个空格来段内换行。

## 列表 

列表项占一行，以“*”、“+”、“-”开头：

    + 一
    + 二
    + 三
    
效果：
+ 一
+ 二
+ 三

有序列表只需要将上述标记符换成数字加句点。而且顺序由书写顺序决定，与数字无关，但数字需要从1开始。例如：

    1.阎
    2.子
    3.君
    
效果：
1.阎
2.子
3.君

## 代码块

每一行前面缩进四个或以上个空格，就认为是开始了一段代码块。代码块内原样输出。
效果：

    void main()
    {
        while(1)
    }

## 横线

三个或更多个*、-（它们之间可以有空格）会产生横线：

## 链接

    [链接文字](链接地址"链接描述")
    例如：
    [google](http://google.com/ "Google")
    
效果：
[google](http://google.com/ "Google")

    <地址>
    例如：
    <http://google.com/>
    
效果：
<http://google.com/>

## 强调

    单个*或_产生斜体，两个（**、__）则产生粗体。例如：
    *斜体* _斜体_
    **粗体** **粗体**
    ***又粗又斜*** ___又粗又斜___

## 内嵌代码

    内嵌代码'地久天长'
    
效果：
内嵌代码'地久天长'

## 图片

语法和链接相似，只是前面多个“!”

    ![替代文字](图片url "图片说明")

    <img src="/images/.../xxx.jpg(或png)"/>

## 转义字符

如果需要使用以上标记字符而不被Markdown理解为格式标记，需要用\转义：例如\\，效果为\。

