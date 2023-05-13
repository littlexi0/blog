---
title: 阿克曼函数(AKM)
date: 2022/9/22
updated: 2022/9/22
cover: 
top_img: 
description: 阿克曼函数的递归定义和用栈模拟实现
swiper_index: 14 #置顶轮播图顺序，非负整数，数字越大越靠前
categories: 数据结构
---

@[TOC](阿克曼函数AKM)

# 阿克曼函数(AKM)
## [定义](https://baike.baidu.com/item/%E9%98%BF%E5%85%8B%E6%9B%BC%E5%87%BD%E6%95%B0/10988285?fr=aladdin)：
阿克曼函数（Ackermann）是非原始递归函数的例子。它需要两个自然数作为输入值，输出一个自然数。它的输出值增长速度非常快，仅是对于(4,3)的输出已大得不能准确计算。
## 1.递归表达式
$$
akm(m,n) = \begin{cases}\quad \text {n+1 \textcolor{red}{(m=0)}}  \\
\quad \text{akm(m-1,1) \textcolor{red}{(m不等于0,n=0)};}\\
 \quad\text{akm(m-1,akm(m,n-1)) \textcolor{red}{(m不等于0,n不等于0)}}
\end{cases} 
$$
## 2.代码实现
### 2.1递归写法：
思路：太easy了，略
```cpp
int akm1(int m, int n)
{
    if (!m)
        return n + 1;
    if (!n)
        return akm1(m - 1, 1);
    return akm1(m - 1, akm1(m, n - 1));
}
```
### 2.2栈模拟写法：
思路：进行栈操作的时候，可以把即将需要操作的m放在栈顶，n放在栈顶下面，当进行akm第三项递归的时候，我们可以把多余的m-1放在栈里面（方便之后操作），再放即将被操作的m，n，当遇到m为0的时候，直接舍去m，把之前栈里面的m取出来，再和n-1配成一对进行操作。
```cpp
int akm2(int m, int n)
{
    stack<int> sta;
    sta.push(n);
    sta.push(m);
    int ans = 0;
    while (sta.empty()!=1)
    {
        
        if (sta.size() == 1)
            ans = sta.top();
        int m = sta.top();
        sta.pop();
        int n = sta.top();
        if (!m)
        {
            if (sta.size() == 1)
                return sta.top() + 1;
            sta.pop();
            m = sta.top();
            sta.pop();
            sta.push(n + 1);
            sta.push(m);
            continue;
        }
        if(n == 0)
        {
            sta.pop();
            if (sta.size() == 1)
                ans = sta.top();
            sta.push(1);
            sta.push(m - 1);
            continue;
        }
        sta.pop();
        if (sta.size() == 1)
            ans = sta.top();
        sta.push(m - 1);
        sta.push(n - 1);
        sta.push(m);
        if (sta.size() == 1)
            ans = sta.top();
    }
    return ans;
}
```