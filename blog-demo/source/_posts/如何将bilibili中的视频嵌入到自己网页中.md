---
title: bilibili视频嵌入方式
date: 2022/11/23
updated: 2022/11/23
cover: https://images5.alphacoders.com/935/thumbbig-935873.webp
top_img: 
description: 如何将bilibili中的视频嵌入到自己网页中
swiper_index: 15 #置顶轮播图顺序，非负整数，数字越大越靠前
categories: 算法
---

## 【LittleXi】如何将bilibili中的视频嵌入到自己网页中

### 第一步
在b站找到自己想要的视频，鼠标移到视频上，点击右键获取视频的地址。
### 第二步
[点击打开网站](https://www.ibilibili.com/)
### 第三步
将复制的地址粘到刚刚打开的网站中解码得到AID和CID
### 第四步
将下述网站的AID和CID替换成我们刚刚得到的值，替换完成后点开下述网站
<code>https://api.bilibili.com/x/player/playurl?avid=AID&cid=CID&qn=1&type=&otype=json&platform=html5&high_quality=1</code>
### 第五步
CV上述网站中的url地址，并把\u0026替换成&就是b站视频的标准地址了
### 第六步
将得到的标准地址直接嵌入到自己的网页中就行啦