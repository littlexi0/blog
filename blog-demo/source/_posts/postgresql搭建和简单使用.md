---
title: postgresql搭建
date: 2023/3/14
updated: 2023/3/14
cover: https://images4.alphacoders.com/130/thumbbig-1304840.webp
top_img: 
description: postgresql搭建及其简单使用
swiper_index: 3 #置顶轮播图顺序，非负整数，数字越大越靠前
categories: sql
---

# 实验报告

## 实验环境准备

从postpresql下载安装，下载最新版好像会报错，选择了一个版本稍微低一点的，安装的时候不要按照默认的位置安装，不然也会报错（😂），安装好了，直接点开pgAdmin 4.exe 文件，就可以在浏览器中显示啦

## 实验要求

### （1）熟悉PostgreSQL的基本操作

### （2）使用图形化界面创建数据库、数据表、插入数据

首先创建数据库

![](https://pic.leetcode.cn/1678799517-zXhNfT-%E6%8D%95%E8%8E%B7.PNG)

,

然后创建表

![](https://pic.leetcode.cn/1678799517-LSMift-20230304154902.png)

输入数据和数据类型，就创建成功啦

然后往表中填入数据,首先创建一个脚本文件

![](https://pic.leetcode.cn/1678799517-PnRQlZ-20230304155039.png)

然后利用脚本的语法插入数据

![](https://pic.leetcode.cn/1678799517-PclWCG-fsafa.png)

就完成了表的创建和插入数据了

### （3）使用SQL命令完成创建数据库、数据表、插入数据

首先打开shell，其它跳过，只输入口令* * * * * * *

![](https://pic.leetcode.cn/1678799517-zkHtfm-4d8a4.png)

这里我的shell好像用不了，就直接用script来写啦（😂）

首先创建表格

![](https://pic.leetcode.cn/1678799517-QeJqcl-hrsga.png)

然后和上面一样插入数据就行啦

### （4）数据库备份和恢复

选中要备份的表格，点击右键，进入backup模式

![](https://pic.leetcode.cn/1678799517-rWlkUN-gaasds.png)

然后按如图输入好内容就行了

![](https://littlexi.oss-cn-shanghai.aliyuncs.com/images/gaasds.png)

注意备份的位置不是安装的位置，要自己去找备份文件的地址

## 思考

### 如何设置主键自增

<code>*alter* table students auto_increment = 1;</code>

### 用命令备份和恢复

备份：<code>pg_dump -h localhost  -U postgres postgres > D:\postgres.bak</code>

恢复:  <code>psql -h localhost -U postgres -d test < D:\postgres.bak</code>