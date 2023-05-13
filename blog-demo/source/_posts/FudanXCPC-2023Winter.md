---
title: FudanXCPC-2023Winter-x
date: 2023/1/03/18

updated: 2023/1/03/18
cover: https://img2.baidu.com/it/u=2040714338,1704198621&fm=253&fmt=auto&app=120&f=JPEG?w=518&h=324
top_img: 
description: 寒假5天训练赛结束了，收获很大，前几天题目很难，后面两天信心场，做得很快乐
swiper_index: 12 #置顶轮播图顺序，非负整数，数字越大越靠前
categories: ACM
---

# FudanXCPC-2023Winter-x

## 摘要：

寒假5天训练赛结束了，收获很大，前几天题目很难，后面两天信心场，做得很快乐

## FudanXCPC-2023Winter-1_解题报告

[A](https://codeforces.com/gym/102365/problem/A)

### 题目大意：

输入E，将s所有字母往后移动s位，输入D往前移动s位

### 解题思路：

按题意模拟即可

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    char ch;
    cin >> ch;
    int s;
    cin >> s;
    string w;
    cin >> w;
    if (ch == 'E')
    {
        for (int i = 0; i < w.size(); i++)
        {
            int g = w[i] - 'a';
            g = (g + s) % 26;
            w[i] = (char)(g + 97);
        }
    }
    if (ch == 'D')
    {
        for (int i = 0; i < w.size(); i++)
        {
            int g = w[i] - 'a';
            g = (g - s + 26) % 26;
            w[i] = (char)(g + 97);
        }
    }
    cout << w;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[B](https://codeforces.com/gym/102365/problem/B)

### 题目大意：

输入n个人，每个人有生命、攻击、防御值，输出能形成A->B->C->A的三元组

### 解题思路：

按题意三层枚举判断，注意正向和反向判断

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

bool check(int hp1,int ak1,int de1, int hp2, int ak2, int de2)
{
    int di1 = max(0, ak2 - de1);
    int di2 = max(0, ak1 - de2);
    if (di1 == 0 && di2 == 0)
        return 0;
    if (di1 == 0)
        return 1;
    if (di2 == 0)
        return 0;
    int r1 = hp1 / di1 + (hp1 % di1 >0);
    int r2 = hp2 / di2 + (hp2 % di2 > 0);
    return r1 > r2;
}


void solve()
{
    int n;
    cin >> n;
    vector<string> name(n);
    vector<int> hp(n);
    vector<int> ak(n);
    vector<int> de(n);
    for (int i = 0; i < n; i++)
    {
        cin >> name[i];
        cin>> hp[i] >> ak[i] >> de[i];

    }
    vector<vector<int>> ans;
    for (int i = 0; i < n; i++)
    {
        for (int j = i+1; j < n; j++)
        {
            for (int k = j+1; k < n; k++)
            {
                int f1 = 1, f2 = 1;

                if (check(hp[i], ak[i], de[i], hp[j], ak[j], de[j]) == false)
                    f1=0;
                if (check(hp[j], ak[j], de[j], hp[k], ak[k], de[k]) == false)
                    f1=0;
                if (check(hp[k], ak[k], de[k], hp[i], ak[i], de[i]) == false)
                    f1=0;

                if (check(hp[j], ak[j], de[j], hp[i], ak[i], de[i]) == false)
                    f2 = 0;
                if (check(hp[k], ak[k], de[k], hp[j], ak[j], de[j]) == false)
                    f2 = 0;
                if (check(hp[i], ak[i], de[i], hp[k], ak[k], de[k]) == false)
                    f2 = 0;
                if(f1||f2)
                    ans.push_back({ i,j,k });
            }
        }
    }
    cout << ans.size() << endl;
    for (int i = 0; i < ans.size(); i++)
    {
        cout << name[ans[i][0]] << " " << name[ans[i][1]] << " " << name[ans[i][2]] << endl;
    }

}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    while (t--)
    {
        solve();
    }
}
```

[C](https://codeforces.com/gym/102365/problem/C)

### 题目大意：

交互题，给定n个节点的树，每次询问x->y是否经过z，不超过n次询问，找到叶子节点

### 解题思路：

维护一个n长度的动态数组，数组中第一次只有1，2，记最左边为l，最右边为r，然后每次询问l->r- >c是否成立，如果成立，则让r贴近c否则让l贴近c

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 

void solve()
{
    int n = 0;
    cin >> n;
    deque<int> dq;
    if (n <= 2)
    {
        cout << "! " << 1 << endl;
        return;
    }
    cout << "? 1 2 3" << endl;
    int in = 0;
    cin >> in;
    if (in == 1)
    {
        dq.push_back(1);
        dq.push_back(2);
        dq.push_back(3);
    }
    else
    {
        dq.push_back(2);
        dq.push_back(1);
        dq.push_back(3);
    }
    for (int i = 3; i <= n - 1; i++)
    {
        cout << "? " << i - 1 << " " << i << " " << i + 1 << endl;
        int g = 0;
        cin >> g;
        if (g == 1)
        {
            if (dq.back() == i)
                dq.push_back(i+1);
            else
                dq.push_front(i + 1);
        }
        else
        {
            if (dq.back() == i)
                dq.push_front(i + 1);
            else
                dq.push_back(i + 1);
        }
    }
    int a = dq.front(),b=dq.back();
    int c = 1;
    if (c == a || c == b)
        c++;
    if (c == a || c == b)
        c++;
    cout << "? " << c << " " << a << " " << b<<endl;
    int g = 0;
    cin >> g;
    if (g)
        cout << "! " << b << endl;
    else
        cout << "! " << a << endl;

}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    while (t--)
    {
        solve();
    }
}
```

[E](https://codeforces.com/gym/102365/problem/E)

### 题目大意：

给定n个数字的数组，划分为k个区间，每个区间的权值为该区间最大数，求最大权值和

### 解题思路：

取数组中最大的k个数字即可

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

int dp[2010][1010] = { 0 };
int ma[2010][1010] = { 0 };

void solve()
{
    int n, k;
    cin >> n >> k;
    vector<int> a(n);
    for (int i = 0; i < n; i++)
        cin >> a[i];
    int ans = 0;
    sort(a.begin(), a.end());
    for (int i = n - 1; i >= n-1-k+1; i--)
    {
        ans += a[i];
    }
    cout << ans << endl;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[H](https://codeforces.com/gym/102365/problem/H)

### 题目大意：

给定数字C，求最小D，使得CD^3为完全平方数

### 解题思路：

D^2可以直接忽略，分解C，只需要找出C的素数中个数为奇数的数字即可，注意到有大于1e7的素数，开平方特判即可

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

vector<ll> getp(ll c)
{
    vector<ll> pri;
    for (ll i = 2;i*i<=c; i++)
    {
        if (i == 1e7)
        {
            pri.push_back(c);
            return pri;
        }
        if (c % i == 0)
        {
            pri.push_back(i);
            c /= i;
            i = 1;
        }
    }
    pri.push_back(c);
    return pri;
}

void solve()
{
    ll c;
    cin >> c;
    vector<ll> pri = getp(c);
    ll b = pri.back();
    ll sq = (ll)sqrt(b);
    if (sq * sq == b)
    {
        pri.pop_back();
        pri.push_back(sq);
        pri.push_back(sq);
    }

    unordered_map<ll, ll> m;
    for (ll x : pri)
    {
        m[x]++;
    }
    ll ans = 1;
    for (auto p : m)
    {
        if (p.second & 1)
            ans *= p.first;
    }
    cout << ans << endl;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

## FudanXCPC-2023Winter-2_解题报告

[A](https://codeforces.com/gym/104120/problem/A)

### 题目大意：

签到题，给定步长，判断走满3000步需要的时间

### 解题思路：

按题意模拟即可，注意特判

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    int n;
    cin >> n;
    int t = 3000 / n + (3000 % n > 0);
    cout << min(t, 15);
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[B](https://codeforces.com/gym/104120/problem/B)

### 题目大意：

给n*m的矩阵，走过每个位置需要种类为M_ij的通行证，问从起点到终点至少需要多少种通行证

### 解题思路：

状态压缩+dfs

将10种通行证枚举情况用state的位表示

在state的情况下进行DFS，看能否到达终点

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

int grid[110][110] = { 0 };
int n, m;
int vis[101][101] = {0};
int ans = 1e9;

bool ifon(int x, int y, int n, int m)
{
    return x >= 0 && x < n&& y >= 0 && y < m;
}

void dfs(int x,int y,int ex,int ey,int sta)
{
    //cout << x << " " << y << endl;
    if (x == ex && y == ey)
    {
        int cnt = 0;
        for (int i = 0; i < 12; i++)
            cnt += (sta >> i) & 1;
        ans = min(ans, cnt);
        return;
    }
    int dx[4] = { -1,1,0,0 };
    int dy[4] = { 0,0,-1,1 };
    for (int i = 0; i < 4; i++)
    {
        int tox = x + dx[i];
        int toy = y + dy[i];
        if (!ifon(tox, toy, n, m))
            continue;
        if (vis[tox ][toy])
            continue;
        if (((sta >> (grid[tox][toy] - 1)) & 1) == 0)
            continue;
        vis[tox ][ toy] = 1;
        dfs(tox, toy, ex, ey, sta);
    }
}

void solve()
{
    //ll
    cin >> n >> m;
    int sx, sy, ex, ey;
    cin >> sx >> sy >> ex >> ey;
    sx--; sy--; ex--; ey--;
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < m; j++)
        {
            cin >> grid[i][j];
        }
    }
    for (int sta = 0; sta < 1024; sta++)
    {
        if (((sta >> (grid[sx][sy] - 1)) & 1) == 0)
            continue;
        for (int i = 0; i < n; i++)
            for (int j = 0; j < m; j++)
                vis[i][j] = 0;
        vis[sx][sy] = 1;

        dfs(sx, sy, ex, ey, sta);
    }
    cout << ans;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```



[C](https://codeforces.com/gym/104120/problem/C)

### 题目大意：

给定n个数字，q次询问，每次询问将所有数字削减到q_i，求和所有数字

### 解题思路：

排序+维护所有值为q_i的数字的个数

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"


void solve()
{
    //ll
    ll n, m;
    cin >> n >> m;
    vector<ll> a(n);
    for (ll i = 0; i < n; i++)
        cin >> a[i];
    sort(a.begin(), a.end());
    ll p = n - 1;
    ll sum = accumulate(a.begin(), a.end(), (ll)0);
    vector<ll> q(m);
    for (ll i = 0; i < m; i++)
        cin >> q[i];
    for (ll i = 0; i < m; i++)
    {
        ll up = q[i];
        if (i >= 1)
        {
            sum -= (q[i - 1] - q[i]) * (n - 1 - p);
        }
        while (p>=0&&a[p] >= up)
        {
            sum -= a[p] - up;
            p--;
        }
        cout << sum << endl;
    }


}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[G](https://codeforces.com/gym/104120/problem/G)

### 题目大意：

给一个n*n的矩阵，有abcd四种翻转方式，经过若干不同翻转之后，问xx位置的数字是什么

### 解题思路：

矩阵的变换注意AB!=BA

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    ll n, q;
    cin >> n >> q;
    ll a[4] = { 0 };
    while (q--)
    {
        char op;
        cin >> op;
        if (op == 'r')
        {
            char dir;
            cin >> dir;
            a[dir - 'a']++;
            a[dir - 'a'] %= 2;
            if (dir == 'b' || dir == 'd')
                swap(a[0], a[2]);
        }
        else
        {
            ll x, y;
            cin >> x >> y;


            if (a[0]) swap(x, y);
            if (a[2])
            {
                swap(x, y);
                x = n + 1 - x;
                y = n + 1 - y;
            }
            if (a[1]) y = n + 1 - y;
            if (a[3]) x = n + 1 - x;
            ll ans = (x - 1) * n + y;
            cout << ans << endl;
        }
    }
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    while (t--)
    {
        solve();
    }
}
```

[K](https://codeforces.com/gym/104120/problem/K)

### 题目大意：

模拟电话座机的数字与字母的对应方式，给一串字母，询问q次数字，问该串数字对应的字符串有多少个

### 解题思路：

将字符串映射为数字串，然后用字典树统计数量即可

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    ll n, q;
    cin >> n >> q;
    int hm[26] = { 0 };
    vector<string> v = { "abc","def","ghi","jkl","mno","pqrs","tuv","wxyz" };
    memset(nex, 0, sizeof(nex));
    memset(exist, 0, sizeof(exist));
    cnt = 0;
    for (ll i = 0; i < v.size(); i++)
    {
        for (ll j = 0; j < v[i].size(); j++)
        {
            hm[v[i][j] - 97] = i + 2;
        }
    }
    while (n--)
    {
        string s;
        cin >> s;
        ll val = 0;
        for (ll i = 0; i < s.size(); i++)
        {
            val *= 10;
            val += hm[s[i]-'a'];
           
        }
        s = to_string(val);
        insert(s);
//        m[val]++;
    }
    while (q--)
    {
        string val;
        cin >> val;
        
        cout << search(val) << endl;
    }


}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[L](https://codeforces.com/gym/104120/problem/L)

### 题目大意：

给定一棵树，从1出发，前往终点end，若遇到end的邻点，则可以直接到达end，问最多走多少步

### 解题思路：

DFS

从起点出发，去寻找end的邻点，每走一步cnt+=2，起点到end的邻点的距离是p，输出cnt-p+1

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

vector<vector<int>> adj;
vector<int> vis;
int n, tar;
int dis = 0, link = 0;
int cnt = 0;
void dfs(int po,int len)
{
    for (int i = 0; i < adj[tar].size(); i++)
    {
        int li = adj[tar][i];
        if (po == li)
        {
            dis = len;
            link = po;
            return;
        }
    }

    for (int i = 0; i < adj[po].size(); i++)
    {
        int to = adj[po][i];
        if (vis[to])
            continue;
        cnt += 2;
        vis[to] = 1;
        dfs(to, len + 1);
    }
}


void solve()
{
    cin >> n >> tar;
    tar--;
    adj.resize(n);
    vis.resize(n, 0);
    for (int i = 0; i < n - 1; i++)
    {
        int l, r;
        cin >> l >> r;
        l--; r--;
        adj[l].push_back(r);
        adj[r].push_back(l);
    }
    vis[0] = 1;
    dfs(0, 0);
    cnt -= dis - 1;
    cout << cnt;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```



## FudanXCPC-2023Winter-3_解题报告

[A](https://codeforces.com/gym/103048/problem/A)

### 题目大意：

给定两个矩阵A,B,C，通过适当的矩阵乘法变换获得C

### 解题思路：

因为ac-bd=1，且至少含有一个0，那么abcd中一定含有1，0，1，x或者0，1，-1，x，对这2种情况进行分类讨论就好了

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    int a, b, c, d;
    cin >> a >> b >> c >> d;
    int t = 0;
    while (c && d)
    {
        swap(a, c);
        swap(b, d);
        a = -a;
        b = -b;
        t++;
    }
    if (t==0) t = 4;
    if (c == -1 && d == 0)
        cout << 3 << endl << "B " << t << endl << "A " << -a << endl << "B " << 1 << endl;
    else if (c == 0 && d == -1)
        cout << 3 << endl << "B " << t << endl << "A " << -b << endl << "B " << 2 << endl;
    else if (c == 1 && d == 0)
        cout << 3 << endl << "B " << t << endl << "A " << a << endl << "B " << 3 << endl;
    else if (c == 0 && d == 1)
        cout << 3 << endl << "B " << t << endl << "A " << b << endl << "B " << 4 << endl;

}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[C](https://codeforces.com/gym/103048/problem/C)

### 题目大意：

计算April 10, 2021到October 16 in 2021有多少天

### 解题思路：

签到题，手动计算完直接输出就好啦

### 解题代码：

```python
print(189)
```

[D](https://codeforces.com/gym/103048/problem/D)

### 题目大意：

给定a，b判断a能否整除b，a=Πx，b=Πy，x、y分别是一个区间内的所有整数

### 解题思路：

首先用欧拉筛把所有的1e7范围内的素数筛出来，然后暴力枚举每一个素数，看a是否为b的子集，时间复杂度nlogn（调和级数求和）

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

#define maxn 10000009


int vis[maxn] = { 0 };
int pri[100000] = { 0 };
int cnt = 0;

void init(ll n) {
    for (ll i = 2; i <= n; ++i) {
        if (!vis[i]) {
            pri[cnt++] = i;
        }
        for (ll j = 0; j < cnt; ++j) {
            if (1ll * i * pri[j] > n) break;
            vis[i * pri[j]] = 1;
            if (i % pri[j] == 0) {
                break;
            }
        }
    }
}


void solve()
{

    ll l1, r1, l2, r2;
    cin >> l1 >> r1 >> l2 >> r2;
    for (ll i = 0; i < cnt; i++)
    {
        if (pri[i] > r1) break;
        ll cnt1 = 0, cnt2 = 0;
        for (ll m = pri[i]; m <= max(r1, r2); m *= (ll)pri[i])
        {
            cnt1 += (r1 / m) - (l1 - 1) / m;
            cnt2 += (r2 / m) - (l2 - 1) / m;
        }
        if (cnt1 > cnt2)
        {
            cout << "No" << endl;
            return;
        }

    }
    cout << "Yes"<<endl;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    cin >> t;
    init(10000001);
    while (t--)
    {
        solve();
    }
}
```

[E](https://codeforces.com/gym/103048/problem/E)

### 题目大意：

树上两个人，A先动B后动，谁被“踩”了谁就输了，判断A是否稳赢

### 解题思路：

DFS AB之间的距离就好了，如果是奇数A胜利，否则B胜利

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

int n;
vector<vector<int>> vec(n);
int dis = 0;
int a, b;
vector<int> vis(n, 0);
void dfs(int p, int cnt)
{
    if (dis)
        return;
    if (p == b)
    {
        dis =cnt;
        return;
    }
    for (int i = 0; i < vec[p].size(); i++)
    {
        int to = vec[p][i];
        if (vis[to])
            continue;
        vis[to] = 1;
        dfs(to, cnt + 1);
        vis[to] = 0;
    }
}

void solve()
{
    cin >> n;
    vec.resize(n);
    vis.resize(n);
    for (int i = 0; i < n-1; i++)
    {
        int l, r;
        cin >> l >> r;
        l--; r--;
        vec[l].push_back(r);
        vec[r].push_back(l);
    }
    cin >> a>>b;
    a--; b--;
    vis[a] = 1;
    dfs(a, 0);
    //cout << dis << endl;
    if (dis & 1)
    {
        cout << "Yes";
    }
    else
        cout << "No";
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[G](https://codeforces.com/gym/103048/problem/G)

### 题目大意：

n个玩家被分为m组，每个人可以禁用一张地图，问至少需要准备多少张地图

### 解题思路：

分类讨论，当n==m时，需要准备2张地图，当m==1时需要准备n+1张地图，否则3张地图就够了

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

int n;
vector<vector<int>> vec(n);
int dis = 0;
int a, b;
vector<int> vis(n, 0);
void dfs(int p, int cnt)
{
    if (dis)
        return;
    if (p == b)
    {
        dis = cnt;
        return;
    }
    for (int i = 0; i < vec[p].size(); i++)
    {
        int to = vec[p][i];
        if (vis[to])
            continue;
        vis[to] = 1;
        dfs(to, cnt + 1);
        vis[to] = 0;
    }
}

void solve()
{
    int n, m;
    cin >> n >> m;

    if (m == n)
    {
        cout << 2 << endl;
        return;
    }
    if (m == 1)
    {
        cout << n + 1 << endl;
        return;
    }
    cout << 3 << endl;

}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[I](https://codeforces.com/gym/103048/problem/I)

### 题目大意：

给定s、t，问s中是否包含t

### 解题思路：

双指针+贪心移动

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    string s, t;
    cin >> s >> t;
    int p = 0;
    for (int i = 0; i < s.size(); i++)
    {
        if (s[i] == t[p])
            p++;
        if (p == t.size())
        {
            cout << "Yes";
            return 0;
        }
    }
    cout << "No";


}
```

[K](https://codeforces.com/gym/103048/problem/K)

### 题目大意：

给定l，k问[l,l+2k)中是否有超过一半的素数

### 解题思路：

分类讨论,l==2且k<=3时有超过一半是素数，否则没有

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int l, k;
    cin >> l >> k;
    if (l == 2 && k <=3)
    {
        cout << "Yes";
        return 0;
    }
    cout << "No";
}
```

## FudanXCPC-2023Winter-4_解题报告

[A](https://codeforces.com/gym/104101/problem/A)

### 题目大意：

输出"fengqibisheng, yingyueerlai!"（不包含引号）

### 解题思路：

略

### 解题代码：

```python
print("fengqibisheng, yingyueerlai!")
```

[B](https://codeforces.com/gym/104101/problem/B)

### 题目大意：

处理m个事件，三种类型，输出最后hp

### 解题思路：

根据题目含义模拟就好啦

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

int sol(string s)
{
    string s1 = s.substr(0, 2);
    string s2 = s.substr(3, 2);
    int t = atoi(s1.c_str())*60 + atoi(s2.c_str());
    return t;
}

void solve()
{
    int h1, h2, q;
    cin >> h1 >> h2 >> q;
    int f = 0;
    int t = 0;
    int sok[5] = { 0 };
    while (q--)
    {
        string s;
        int type = 0;
        cin >> s >> type;
        int nowt = sol(s);
        if (type == 1)
        {
            f = 1;
            h1+=800;
        }
        if (type == 2)
        {
            h1+=h2;
        }
        if (type == 3)
        {
            int x;
            cin >> x;
            x--;
            if (f && nowt >= sok[x])
            {
                h1 += (125 + 0.06 * h1) * 0.1;
                sok[x] = nowt + 30;
            }
        }
    }
    cout << h1;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[C](https://codeforces.com/gym/104101/problem/C)

### 题目大意：

给定n个整数，求有多少对满足a+9=b

### 解题思路：

用哈希表存储出现的数字的个数

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    int n = 0;
    cin >> n;
    vector<int> a(n);
    unordered_map<int, int> m;
    for (int i = 0; i < n; i++)
    {
        cin >> a[i];
        m[a[i]] = 1;
    }
    int cnt = 0;
    for (int i = 0; i < n; i++)
    {
        if (m[a[i] + 9] == 0)
            cnt++;
    }
    cout << cnt;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[F](https://codeforces.com/gym/104101/problem/F)

### 题目大意：

n个人有a_i的血量，每秒减少b_i的血量，你可以给每个人分配c_i的血量，分配次数有限制，求最多能救活的人

### 解题思路：

贪心，根据需要的治疗次数进行排序

### 解题代码：

```cpp
#include<iostream>
#include<string>
#include<vector>
#include<list>
#include<algorithm>
#include<deque>
#include<set>
#include<map>
#include<queue>
#include<math.h>
#include<unordered_set>
#include<unordered_map>
#include<numeric>
#include<stack>
#include<functional>
#include<cstring>
#include<sstream>
#pragma warning(disable:4996)
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    ll n, m, k;
    cin >> n >> m >> k;
    vector<ll> a(n), b(n), c(n);
    for (int i = 0; i < n; i++)
        cin >> a[i];
    for (int i = 0; i < n; i++)
        cin >> b[i];
    for (int i = 0; i < n; i++)
        cin >> c[i];
    for (int i = 0; i < n; i++)
        a[i] -= m * b[i];
    vector<ll> t(n,0);
    
    for (int i = 0; i < n; i++)
    {
        if (a[i] < 0)
        {
            a[i] *= -1ll;
            t[i] = a[i] / c[i];
            t[i]++;
            a[i] *= -1ll;
        }
        else if (a[i] == 0)
        {
            t[i] = 1ll;
        }
        //cout << t[i] << " ";
    }
    //cout << endl;
    sort(t.begin(), t.end());
    for (int i = 0; i < n; i++)
    {
        if (k - t[i] >= 0)
        {
            k -= t[i];
            continue;
        }
        else
        {
            cout << i;
            return;
        }
    }
    cout << n;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[G](https://codeforces.com/gym/104101/problem/G)

### 题目大意：

有一颗树，树节点被染色为红色和黑色，现给定两条规则若叶子为黑色，则它下面两片叶子也要为黑色。若它下面两片叶子都是黑色，则它也要为黑色。

### 解题思路：

将完全二叉树看成L形的“斜塔”，最上层的黑色节点数目取决于于最底层的染色情况，求最底层 的染色情况，然后利用等差数列的思路求答案即可

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    //ll
    ll n, k;
    cin >> n >> k;
    vector<ll> vis(n, 0);
    vector<pair<ll, ll>> a(k);
    for (ll i = 0; i < k; i++)
    {
        ll x, y;
        cin >> x >> y;
        a[i] = { y,y + n - x };
    }
    sort(a.begin(), a.end(), [&](pair<ll, ll>& p1, pair<ll, ll>& p2) {return p1.first < p2.first; });
    ll l = a[0].first;
    ll r = a[0].second;
    ll cnt = 0;
    for (ll i = 0; i < a.size(); i++)
    {
        ll c = a[i].first,d=a[i].second;
        if (c-1 <= r)
        {
            r = max(r, d);
            continue;
        }

        cnt += (1 + r - l + 1) * (r - l + 1) / 2;
        l = c;
        r = d;
    }
    cnt += (1 + r - l + 1) * (r - l + 1) / 2;
    cout << cnt;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[H](https://codeforces.com/gym/104101/problem/H)

### 题目大意：

给定s，和t的长度，求生成t的数量，生成方式1：t中每个字母两两不同且均出现在s中，生成方式2：t的首字母出现在s中，且t中每个字母呈字典序上升方式

### 解题思路：

生成方式1：求s中出现的字母的次数，然后利用排列组合的方式求数目

生成方式2：确定t的首字母后用背包dp求后面的生成次数

注意：去重！！！

### 解题代码：

```cpp
#include<iostream>
#include<string>
#include<vector>
#include<list>
#include<algorithm>
#include<deque>
#include<set>
#include<map>
#include<queue>
#include<math.h>
#include<unordered_set>
#include<unordered_map>
#include<numeric>
#include<stack>
#include<functional>
#include<cstring>
#include<sstream>
#pragma warning(disable:4996)
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

ll comb(ll m,ll n)
{
    if (m == n)
        return 1ll;
    ll re = 1ll;
    for (ll i = 0; i < n; i++)
        re *= (m - i);
    for (ll i = 1; i <= n; i++)
        re /= i;
    return re;
}

ll cau(char c,ll n)
{
    vector<ll> dp(18, 0);
    dp[c-'a'] = 1;
    for (ll i = 1; i < n; i++)
    {
        ll sum = 0;
        vector<ll> b(18, 0);
        for (ll j = 0; j < 18; j++)
        {
            b[j] = sum;
            sum += dp[j];
        }
        swap(b, dp);
    }
    //for (int i = 0; i < dp.size(); i++)
    //    cout << dp[i] << " ";
    //cout << endl;
    return accumulate(dp.begin(), dp.end(), 0ll);
}

void solve()
{
    //ll
    string s;
    ll n;
    cin >> s >> n;
    if (n > 18)
    {
        cout << 0 << endl;
        return;
    }
    vector<ll> a(18, 0);
    for (ll i = 0; i < s.size(); i++)
        a[s[i] - 'a'] =1;
    ll m = accumulate(a.begin(), a.end(), 0ll);
    ll ans = 0;
    if (m >= n)
    {
        ans += comb(m, n);
        for (ll x = 1; x <= n; x++)
            ans *= x;
    }
    for (ll i = 0; i < 18; i++)
    {
        if (a[i])
        {
            ans += cau((char)(i+'a'), n);
            ll cnt = 0;
            for (int j = 17; j > i; j--)
                cnt+=a[j];
            if(cnt>=n-1)
                ans -= comb(cnt, n - 1);
        }
    }
    cout << ans<<endl;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[I](https://codeforces.com/gym/104101/problem/I)

### 题目大意：

有三个二进制数 x*,*y*,*z(*y*≤*x*,*z*=*x*−*y*) (可能含有前导零)。现在你知道 x 和 y的二进制表示中都有 a 个 11，b 个 00 (*a*+*b*>0)，另外你还知道 z 的二进制表示中有 c 个 11。

请问你能否找出一组合法的 ,x*,*y 满足上述条件, 若不存在输出"-1"(不包含引号)。

### 解题思路：

利用1111100000   和xxxxx00001可以多生成1，分类讨论即可

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    int a, b, c;
    cin >> a >> b >> c;
    if (a == 0)
    {
        if (c == 0)
        {
            for (int i = 0; i < b; i++)
                cout << "0";
            cout << endl;
            for (int i = 0; i < b; i++)
                cout << "0";
            return;
        }
        cout << -1;
        return;
    }
    if (b == 0)
    {
        if (c == 0)
        {
            for (int i = 0; i < a; i++)
                cout << "1";
            cout << endl;
            for (int i = 0; i < a; i++)
                cout << "1";
            return;
        }
        cout << -1;
        return;
    }
    if (a + b - 1 < c)
    {
        cout << -1;
        return;
    }
    if (c < a)
    {
        for (int i = 0; i < b - 1; i++)
            cout << "0";
        for (int i = 0; i < a; i++)
            cout << "1";
        cout << "0" << endl;
        for (int i = 0; i < b - 1; i++)
            cout << "0";
        for (int i = 0; i < a - c; i++)
            cout << "1";
        cout << "0";
        for (int i = 0; i < c; i++)
            cout << "1";
        cout << endl;
        return;
    }
    int p = c - a;
    p += 1;
    for (int i = 0; i < b - p; i++)
        cout << "0";
    for (int i = 0; i < a; i++)
        cout << "1";
    for (int i = 0; i < p; i++)
        cout << "0";
    cout << endl;
    for (int i = 0; i < b - p + 1; i++)
        cout << "0";
    for (int i = 0; i < a - 1; i++)
        cout << "1";
    for (int i = 0; i < p - 1; i++)
        cout << "0";
    cout << "1" << endl;

}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[J](https://codeforces.com/gym/104101/problem/J)

### 题目大意：

给定n个数字，Alice和Bob轮流拿，最终S1-S2是奇数，那么Alice获胜，否则Bob获胜

### 解题思路：

统计奇数的个数，如果是偶数个，那么Alice获胜，否则Bob获胜

注意0的特判

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    int n = 0;
    cin >> n;
    vector<int> a(n);
    int odd = 0;
    for (int i = 0; i < n; i++)
    {
        cin >> a[i];
        odd += a[i] & 1;
    }
    if (odd == 0)
    {
        cout << "Bob";
        return;
    }
    int he = n - odd;
    odd -= 1;
    if (odd % 2 == 0)
    {
        cout << "Alice";
        return;
    }
    if (odd % 2 == 1)
    {
        cout << "Bob";
    }

}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[K](https://codeforces.com/gym/104101/problem/K)

### 题目大意：

给定r，从[0,r]中选择一个数字，使得经过特定的三种操作之后，得到的数字最大

### 解题思路：

对int的32位比特位进行三种运算，然后遍历数字的时候进行贪心获取

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    int n, q;
    cin >> n >> q;
    vector<int> a(30, 1), b(30, 0);
    for (int i = 0; i < n; i++)
    {
        int op,c;
        cin >> op>>c;
        if (op == 1)
        {
            for (int j = 0; j < 30; j++)
            {
                a[j] &= ((c >> j) & 1);
                b[j] &= ((c >> j) & 1);
            }
        }
        if (op == 2)
        {
            for (int j = 0; j < 30; j++)
            {
                a[j] |= ((c >> j) & 1);
                b[j] |= ((c >> j) & 1);
            }
        }
        if (op == 3)
        {
            for (int j = 0; j < 30; j++)
            {
                a[j] ^= ((c >> j) & 1);
                b[j] ^= ((c >> j) & 1);
            }
        }
    }
    while (q--)
    {
        int r = 0;
        cin >> r;
        int ans = 0;
        for (int i = 29; i >= 0; i--)
        {
            int temp = ans;
            temp |= (1 << i);
            if (temp <= r)
            {
                if (b[i] == 1)
                {
                    continue;
                }
                if (a[i] == 1)
                {
                    ans = temp;
                }
            }
        }
        cout << ans << endl;
    }

}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[L](https://codeforces.com/gym/104101/problem/L)

### 题目大意：

两队老头轮流报数，报道m的倍数的时候交换两队老头的位置

### 解题思路：

根据题意进行简单模拟就好啦

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    int n, m,k;
    cin >> n >> m >> k;
    vector<int> a(n), b(n);
    for (int i = 1; i <= n; i++)
    {
        a[i - 1] = i;
        b[i - 1] = i + n;
    }
    for (int i = 1; i <= k; i++)
    {
        int p=(i-1) % n;
        if (i % m == 0)
        {
            swap(a[p], b[p]);
        }
    }
    for (int x : a)
        cout << x << " ";
    for (int x : b)
        cout << x << " ";
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

## FudanXCPC-2023Winter-5_解题报告

[A](https://codeforces.com/gym/102569/problem/A)

### 题目大意：

对一个数组nums进行取哈希值，并且改变数组中的某些值，重求哈希

### 解题思路：

利用数学方法，检测一下什么时候哈希值会改变就好啦

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"


void solve()
{
    //ll
    ll n;
    cin >> n;
    ll a=0, b=0;
    for (int i = 1; i <= n; i++)
    {
        ll x;
        cin >> x;
        if (i % 2 == 1)
            a += x;
        else
            b -= x;
    }
    int q;
    cin >> q;
    while (q--)
    {
        ll l, r, v;
        cin >> l >> r >> v;
        if ((r - l + 1) % 2 == 1)
        {
            if (l % 2 == 1)
            {
                a += v;
            }
            else
                b-=v;
        }
        ll ans = a + b;
        if (n % 2 == 0)
            ans *= -1;
        cout << ans<<endl;

    }


}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[B](https://codeforces.com/gym/102569/problem/B)

### 题目大意：

初始位于起点0，左右某些位置有奖品，要求花费最少的步数拿到最多的奖品

### 解题思路：

将左右的奖品位置分离出来，然后遍历左区间，对右区间进行二分

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

ll cau(vector<ll> p1,vector<ll> p2,ll t)
{
    ll re = 0;
    for (ll i = -1; i < (int)p1.size(); i++)
    {
        ll c = t;
        if(i>=0)
            c = t - 2*p1[i];
        if (c < 0)
            break;
        ll l = -1, r = p2.size();
        while (l + 1 < r)
        {
            ll m = (l + r) / 2;
            if (p2[m] <= c)
                l = m;
            else
                r = m;
        }
        re = max(re, i + 1 + l + 1);
    }
    //cout << re<<endl;
    return re;
}

void solve()
{
    //ll
    ll n, t;
    cin >> n >> t;
    vector<ll> p(n);
    for (ll& x : p)
        cin >> x;
    ll ans = 0;
    ll f = 0;
    vector<ll> p1, p2;
    for (ll i = 0; i < p.size(); i++)
    {
        if (p[i] == 0)
            f ++;
        if (p[i] < 0)
            p1.push_back(-p[i]);
        if (p[i] > 0)
            p2.push_back(p[i]);
    }
    sort(p1.begin(),p1.end());
    sort(p2.begin(),p2.end());
    ans = max(ans, cau(p1, p2, t)+f);
    ans = max(ans, cau(p2, p1, t)+f);
    cout << ans;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[E](https://codeforces.com/gym/102569/problem/E)

### 题目大意：

给定n个数字，行走的过程中hp增加nums[i]，为了hp不能小于0，求最小初始hp

### 解题思路：

最小前缀和

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"


void solve()
{
    //ll
    ll n;
    cin >> n;
    ll sum = 0;
    ll ans = 0;
    for (ll i = 0; i < n; i++)
    {
        ll x;
        cin >> x;
        sum += x;
        ans = min(ans, sum);
    }
    cout << -ans;
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[F](https://codeforces.com/gym/102569/problem/F)

### 题目大意：

给定n个窗户，目标在某个窗户后面看，当没shoot中目标时，目标会往右移动一格，给出射击策略

### 解题思路：

可以跳着射击，只射击奇数位和最后的窗户。

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    //ll
    ll n;
    cin >> n;
    if (n == 1)
    {
        cout << 1 << endl;
        cout << 1;
        return;
    }
    if (n == 2)
    {
        cout << 2 << endl;
        cout << "1 2";
        return;
    }
    cout << (n)/2+1 << endl;
    for (int i= 1; i <= n - 1; i+=2)
    {
        cout << i << " ";
    }
    cout << n << endl;
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[H](https://codeforces.com/gym/102569/problem/H)	

### 题目大意：

给定一棵树，每次选择两个点，将这两个点之间的路标记为走过，问至少操作多少次，使得树上所有点都走过

### 解题思路：

可以发现，选择叶子节点标记的最多，所以只需要考虑（叶子节点的数量+1）/2就行了

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    //ll
    ll n;
    cin >> n;
    vector<int> du(n, 0);
    //vector<vector<int>> vec(n);
    for (int i = 0; i < n - 1; i++)
    {
        int l, r;
        cin >> l >> r;
        l--; r--;
        du[l]++;
        du[r]++;
    }  
    int cnt = 0;
    for (int i = 0; i < n; i++)
    {
        if (du[i] == 1)
            cnt++;
    }
    int ans = cnt / 2 + (cnt % 2);
    cout << ans;
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[I](https://codeforces.com/gym/102569/problem/I)

### 题目大意：

给定n个数字，每个数字都有颜色，相邻数字可以交换，但是相同颜色不能交换，问能否交换任意次，使得数字有序

### 解题思路：

被标记了相同颜色的数字不能互相交换

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    //ll
    ll n;
    cin >> n;
    vector<int> v;
    vector<pair<int, int>> vec(n);
    for (auto& p : vec)
    {
        cin >> p.first >> p.second;
        v.push_back(p.first);
    }
    unordered_map<int, vector<int>> m;
    for (int i = 0; i < n; i++)
    {
        m[vec[i].second].push_back(vec[i].first);
    }
    for (auto& p : m)
    {
        for (int i = 0; i < p.second.size() - 1; i++)
        {
            if (p.second[i] > p.second[i + 1])
            {
                cout << "NO";
                return;
            }
        }
    }
    cout << "YES";

}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[J](https://codeforces.com/gym/102569/problem/J)

### 题目大意：

A有k个数字，B有k个数字，1vs1时，A获胜几率大，2vs2时，B获胜几率大，3vs3时，C获胜几率大

### 解题思路：

构造答案

3

3 3 4

3

1 1 7



### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"


void solve()
{
    //ll
    cout << "3" << endl;
    cout << "3 3 4" << endl;
    cout << "3" << endl;
    cout << "1 1 7" ;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[K](https://codeforces.com/gym/102569/problem/K)

### 题目大意：

给定4个数字，判断这四个数字是否能成为桌子的腿

### 解题思路：

分类讨论即可

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    ll a, b, c, d;
    vector<ll> v(4);
    for (ll i = 0; i < 4; i++)
        cin >> v[i];
    sort(v.begin(), v.end());
    a = v[0];
    b = v[1];
    c = v[2];
    d = v[3];
    if (a == b && c == d)
    {
        cout << "YES";
        return;
    }
    if (b == c && a + d == 2 * b)
    {
        cout << "YES";
        return;
    }
    if (b-a==d-c)
    {
        cout << "YES";
        return;
    }
    cout << "NO";
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[K](https://codeforces.com/gym/102569/problem/K)

### 题目大意：

给定n个数字，获取的价值为nums[i]-第几次，求最大获取价值

### 解题思路：

贪心，从大到小排序

### 解题代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    //ll
    ll n;
    cin >> n;
    vector<ll> v(n);
    for (ll& x : v)
        cin >> x;
    sort(v.begin(), v.end(), [&](ll a, ll b) {return a > b; });
    ll cnt = 0;
    for (ll i = 0; i < v.size(); i++)
    {
        if (v[i] - i - 1 <= 0)
            break;
        cnt += v[i] - i - 1;
    }
    cout << cnt;
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

[M](https://codeforces.com/gym/102569/problem/M)

### 题目大意：

给定n个电视节目的播放时间和播放时长，求看完所有电视的结束时间

### 解题思路：

模拟观看时间即可

### 解题代码：

```cpp
#include<iostream>
#include<string>
#include<vector>
#include<list>
#include<algorithm>
#include<deque>
#include<set>
#include<map>
#include<queue>
#include<math.h>
#include<unordered_set>
#include<unordered_map>
#include<numeric>
#include<stack>
#include<functional>
#include<cstring>
#include<sstream>
#pragma warning(disable:4996)
using namespace std;
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define _for(i,a,b) for(int i=(a);i<(b);++i)
#define _rep(i,a,b) for(int i=(a);i<=(b);++i)
#define inf 0x7fffffff
#define ll long long 
#define endl "\n"

void solve()
{
    //ll
    ll n;
    cin >> n;
    ll t = 0;
    for (int i = 0; i < n; i++)
    {
        ll l, r;
        cin >> l >> r;
        if (t < l)
        {
            t = l + r;
        }
        else
            t += r;
    }
    cout << t;
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);
    int t = 1;
    //cin >> t;
    while (t--)
    {
        solve();
    }
}
```

