---
title:  最多能完成排序的块II
date: 2022/8/13
updated: 2022/8/13
cover: https://images4.alphacoders.com/129/thumbbig-1290963.webp
top_img: 
description: 力扣刷题
swiper_index: 13#置顶轮播图顺序，非负整数，数字越大越靠前
categories: leetcode
---

# 【LittleXi】最多能完成排序的块 II

[题目来源](https://leetcode.cn/problems/max-chunks-to-make-sorted-ii/)
### 题目描述:
这个问题和“最多能完成排序的块”相似，但给定数组中的元素可以重复，输入数组最大长度为2000，其中的元素最大为10**8。

arr是一个可能包含重复元素的整数数组，我们将这个数组分割成几个“块”，并将这些块分别进行排序。之后再连接起来，使得连接的结果和按升序排序后的原数组相同。

我们最多能将数组分成多少块？

#### 实例1：
```
输入: arr = [5,4,3,2,1]
输出: 1
解释:
将数组分成2块或者更多块，都无法得到所需的结果。
例如，分成 [5, 4], [3, 2, 1] 的结果是 [4, 5, 1, 2, 3]，这不是有序的数组。 
```

#### 实例2：
```
输入: arr = [2,1,3,4,4]
输出: 4
解释:
我们可以把它分成两块，例如 [2, 1], [3, 4, 4]。
然而，分成 [2, 1], [3], [4], [4] 可以得到最多的块数。 
```

#### 注意：
```
arr的长度在[1, 2000]之间。
arr[i]的大小在[0, 10**8]之间。
```
### 图解如图所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/a2182171891e4bddad25ab9f1133700a.jpeg#pic_center)
### C++代码：
```
class Solution {
public:
    static bool pre(pair<int, int>& p1, pair<int, int>& p2)
    {
        return p1.first < p2.first;
    }
    int maxChunksToSorted(vector<int>& arr) {
        unordered_map<int, queue<int>> hashmap;
        vector<int> v = arr;
        sort(v.begin(), v.end());
        for (int i = 0; i < v.size(); i++)
        {
            hashmap[v[i]].push(i);
        }
        vector<pair<int, int>> segment;
        for (int i = 0; i < arr.size(); i++)
        {
            segment.push_back({ i,hashmap[arr[i]].front() });
            hashmap[arr[i]].pop();
        }
        sort(segment.begin(), segment.end(), pre);
        int ans = 1;
        int ma = segment[0].second;
        for (int i = 1; i < segment.size(); i++)
        {
            ans += segment[i].first > ma;
            ma = max(ma, segment[i].second);
        }
        return ans;
    }
};
```
##### @author:LittleXi
##### [bilibili主页](https://space.bilibili.com/524432272?spm_id_from=333.337.0.0)
##### [Leetcode主页](https://leetcode.cn/u/stupefied-7umierebon/)
##### [github主页](https://github.com/LittleXi01)



