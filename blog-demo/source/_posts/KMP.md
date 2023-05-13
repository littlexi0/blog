---
title: 字符串的模式串匹配问题(含KMP)
date: 2022/11/24
updated: 2022/11/25
cover: https://images.alphacoders.com/101/thumbbig-1018909.webp
top_img: https://images5.alphacoders.com/106/thumbbig-1067196.webp
description: BF算法|KR（Karp-Rabin）算法|KMP算法
swiper_index: 2 #置顶轮播图顺序，非负整数，数字越大越靠前
categories: 算法
---

@[TOC](BF.KR.KMP匹配思路)

## 1.字符串的模式串匹配问题(含KMP)
 前置说明，字符串s长度m，模式串p长度n
#### 1.1BF算法

1.1.1利用循环进行暴力匹配，时间复杂度O(mn)

#### 1.2KR（Karp-Rabin）算法
1.2.1利用滑动窗口内容逐一匹配
1.2.2将滑动窗口内的m个字符的比较变为一个哈希值的比较\
时间复杂度O(n)
#### 1.3KMP算法
##### 1.3.1匹配思路：
匹配过程中，对于已经匹配好的p串的子串进行重复利用，i指针指向s串，j指针指向p串，正常匹配的话，i++，j++，当发生匹配失败的时候，我们仅仅需要向前移动j指针，即将j指针移动到j之前的那个子串的前缀能匹配后缀的最大长度的位置，那么怎样移动j指针呢？我们可以用一个next[]数组来记录将j往前移动的位置。

```cpp
    void get_next(int next[],string p)
    {
        next[0]=0;next[1]=0;//将前两个初值赋为-1和0
        int j=0;
        for(int i=2;i<p.size()+1;i++)
        {         
            if(p[j]!=p[i-1])//如果发现不匹配，则递归地项前寻找满足条件的最长前后公共子串，（如下图解所示）
            {
                while(j>0&&p[i-1]!=p[j])
                    j=next[j];
                next[i]=j;
            }
            else//否则加长匹配的公共子串，并向后循环
                next[i]=++j;
        }
    } 
```
##### 1.3.2图片描述：看图顺序为绿色->黑色->紫色->蓝色
绿色：是已经匹配好了的最长公共前后缀
黑色：是正在匹配的字符，并且是那种不匹配的字符
紫色：是不匹配的情况下，向前递归寻找的的可执行的位置
蓝色：是递归寻找到了的可移动到的位置
说明：所有相同颜色框起来的字符串都是相同的
j的位置已经标出来了，右下角空心箭头指的位置是i的位置
![在这里插入图片描述](https://img-blog.csdnimg.cn/0c650b04d69648d29e9313ba81b6b7c0.png#pic_center)