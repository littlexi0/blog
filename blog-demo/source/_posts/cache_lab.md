---
title: cache_lab
date: 2022/11/24
updated: 2022/11/25
cover: https://images5.alphacoders.com/765/thumbbig-765059.webp
top_img: https://images6.alphacoders.com/744/thumbbig-744516.webp
description: 本实验在学习csapp上的高速缓存cache的命中、不命中、LRU替换等机制后，在LINUX机器上使用C语言模拟缓存行为。
swiper_index: 1 #置顶轮播图顺序，非负整数，数字越大越靠前

---

---



# 【LittleXi】cache_lab

## part A

### lab 介绍

本实验在学习csapp上的高速缓存cache的命中、不命中、LRU替换等机制后，在LINUX机器上使用C语言模拟缓存行为。

ps：这个题看似是模拟cache的行为的题目，实际上是一个模拟LRU机制的算法题，阅读完实验PDF介绍后，我们可以将其转化为算法题来做

### 转化后

input

```
./csim-ref -v -s 4 -E 1 -b 4 -t traces/yi.
I 10，1
 L 10,1
 M 20,1 
 L 22,1
 S 18,1
 L 110,1
 L 210,1
 M 12,1
```

output

```
hits:4 misses:5 evictions:3
```

### 思路分析

这里我们简单介绍一下hits、misses、evictions数量的由来

第一行的I 10,1是初始化，不用管他

首先我们理解一下“地址”的概念

地址：标记标.......标记标记 组索引组.......索引组索引 块偏移......块块偏移

写代码中我们只需要将组索引和标记拿到手，然后用组索引去定位高速缓存阵列中的组，用标记去检测是否匹配就行了

首先我们观察一下第二行的 L 10,1这个可以拆分为<code>L  0000 0001 ,  1</code>四个部分，其中L是指的load data（加载数据），0000就是指的是target，0001就是指的组索引，逗号后面的部分是size不用去管它

然后就拿这个去匹配，为了方便匹配我们可以开一个二维数组记录高速缓存中的每行的的状态

如果冲突了就采用LRU替换策略，如果不懂LRU策略，可以去刷刷[这个题](https://leetcode.cn/problems/lru-cache/)

### 代码实现

前期准备工作做好了之后，我们就可以开始写代码了

首先定义一个记录每行信息的结构体block,在这里为了实现LRU缓存，我们增加一个时间戳time

```c
struct block
{
    bool valid;
    long long target;
    int time;
};
```

然后就可以定义我们需要的一系列变量了

```c
int hit=0; //命中的次数
int miss=0;//错过的次数
int evictions=0;//冲突并替换的次数
int nowtime=1;//现在的时间
int m=(sizeof(void *)*8);//地址的大小
int verbose=false;//是否打印每次去匹配的log
char str[256];//获取输入的字符串
struct block ***cache;//我们定义的二维数组，数组内容是结构体
int s,E,b,S,B,t,C;//高速缓存的大小参数
FILE *fp;//即将读取的文件
```

接着读取初始化数据

```c
void getoptions(int argc,char* argv[])
{
    int op;
    while((op=getopt(argc,argv,"hvs:E:b:t:"))!=-1)
    {
        switch(op)
        {
            case 'h':
                printf("This is help~");
                exit(0);
            case 'v':
                verbose=true;
                break;
            case 's':
                s=atoi(optarg);
                S=1<<s;
                break;
            case 'E':
                E=atoi(optarg);
                break;
            case 'b':
                b=atoi(optarg);
                B=1<<b;
                break;
            case 't':
                fp=fopen(optarg,"r");
                if(fp==NULL)
                {
                    printf("CAN'T OPREN THE FILE!");
                    exit(0);
                }
                break;
            default:
                break;
        }
    }
    t=m-s-b;
    C=B*E*S;
}
```

获取数据后对cache进行初始化

```c
void init()
{
    cache=malloc(sizeof(void*)*S);
    for(int i=0;i<S;i++)
    {
        cache[i]=malloc(sizeof(void*)*E);
        for(int j=0;j<E;j++)
        {
            struct block *temp=malloc(sizeof(struct  block));
            temp->target=0;
            temp->time=0;
            temp->valid=false;
            cache[i][j]=temp;
        }
    }
}
```

初始化了cache，在结束时当然要释放内存

```c
void freecache()
{
    for(int i=0;i<S;i++)
    {
        for(int j=0;j<E;j++)
        {
            free(cache[i][j]);
        }
        free(cache[i]);
    }
    free(cache);
}
```

<font color="red">关键部分:利用LRU机制去尝试命中高速缓存</font>

```c
void m_visit_cache()
{
    char op;
    unsigned long long add;
    int size;
    sscanf(str," %c %llx,%d",&op,&add,&size);
    long long se=(add<<t)>>(t+b);
    long long tar=add>>(s+b);
    int min_time_po=0;
    int temp_time=cache[se][0]->time;
    for(int i=0;i<E;i++)
    {
        if(cache[se][i]->target==tar&&cache[se][i]->valid)
        {
            if(verbose)
            {
                printf("%s ",str);
                printf("hit\n");
            }
            hit++;
            cache[se][i]->time=nowtime;
            return;
        }
        if(cache[se][i]->time<=temp_time)
        {
            temp_time=cache[se][i]->time;
            min_time_po=i;
        }
    }
    if(verbose)
    {
        printf("%s ",str);
        printf("miss ");
    }    
    miss++;
    if(cache[se][min_time_po]->time!=0)
    {
        if(verbose)
            printf("evictions");
        evictions++;
    }
    if(verbose&&op!='M')
        printf("\n");
    cache[se][min_time_po]->target=tar;
    cache[se][min_time_po]->time=nowtime;
    cache[se][min_time_po]->valid=true;
}
```

从文件中读取每一行操作

```c
void m_input()
{

    while(fgets(str,256,fp))
    {
        nowtime++;
        if(str[strlen(str)-1]=='\n') 
            str[strlen(str)-1]='\0';
        if(strlen(str)==0) return;
        if(str[0]=='I')
            continue;
        else if(str[1]=='M')   
        {
            m_visit_cache();
            m_visit_cache();
        }      
        else
            m_visit_cache();
    }
}
```

主函数：调用上面的函数，并将结果返回到需要的函数中

```c
int main(int argc, char* argv[])
{
    getoptions(argc,argv);
    init();
    m_input();
    fclose(fp);
    freecache();
    printSummary(hit, miss, evictions);
    return 0;
}
```

### 完整AC代码

```c
#include"stdio.h"
#include"string.h"
#include"stdbool.h"
#include"stdlib.h"
#include"getopt.h"
#include"unistd.h"
#include "cachelab.h"

//定义需要的变量
struct block
{
    bool valid;
    long long target;
    int time;
};

int hit=0;
int miss=0;
int evictions=0;
int nowtime=1;
int m=(sizeof(void *)*8);
int verbose=false;
char str[256];
struct block ***cache;
int s,E,b,S,B,t,C;
FILE *fp;
//读取初始换内容
void getoptions(int argc,char* argv[])
{
    int op;
    while((op=getopt(argc,argv,"hvs:E:b:t:"))!=-1)
    {
        switch(op)
        {
            case 'h':
                printf("This is help~");
                exit(0);
            case 'v':
                verbose=true;
                break;
            case 's':
                s=atoi(optarg);
                S=1<<s;
                break;
            case 'E':
                E=atoi(optarg);
                break;
            case 'b':
                b=atoi(optarg);
                B=1<<b;
                break;
            case 't':
                fp=fopen(optarg,"r");
                if(fp==NULL)
                {
                    printf("CAN'T OPREN THE FILE!");
                    exit(0);
                }
                break;
            default:
                break;
        }
    }
    t=m-s-b;
    C=B*E*S;
}
//初始化高速缓存阵列
void init()
{
    cache=malloc(sizeof(void*)*S);
    for(int i=0;i<S;i++)
    {
        cache[i]=malloc(sizeof(void*)*E);
        for(int j=0;j<E;j++)
        {
            struct block *temp=malloc(sizeof(struct  block));
            temp->target=0;
            temp->time=0;
            temp->valid=false;
            cache[i][j]=temp;
        }
    }
}
//释放高速缓存chenlie
void freecache()
{
    for(int i=0;i<S;i++)
    {
        for(int j=0;j<E;j++)
        {
            free(cache[i][j]);
        }
        free(cache[i]);
    }
    free(cache);
}
//尝试去命中高速缓存
void m_visit_cache()
{
    char op;
    unsigned long long add;
    int size;
    sscanf(str," %c %llx,%d",&op,&add,&size);
    long long se=(add<<t)>>(t+b);
    long long tar=add>>(s+b);
    int min_time_po=0;
    int temp_time=cache[se][0]->time;
    for(int i=0;i<E;i++)
    {
        if(cache[se][i]->target==tar&&cache[se][i]->valid)
        {
            if(verbose)
            {
                printf("%s ",str);
                printf("hit\n");
            }
            hit++;
            cache[se][i]->time=nowtime;
            return;
        }
        if(cache[se][i]->time<=temp_time)
        {
            temp_time=cache[se][i]->time;
            min_time_po=i;
        }
    }
    if(verbose)
    {
        printf("%s ",str);
        printf("miss ");
    }    
    miss++;
    if(cache[se][min_time_po]->time!=0)
    {
        if(verbose)
            printf("evictions");
        evictions++;
    }
    if(verbose&&op!='M')
        printf("\n");
    cache[se][min_time_po]->target=tar;
    cache[se][min_time_po]->time=nowtime;
    cache[se][min_time_po]->valid=true;
}
//输入字符串
void m_input()
{

    while(fgets(str,256,fp))
    {
        nowtime++;
        if(str[strlen(str)-1]=='\n') 
            str[strlen(str)-1]='\0';
        if(strlen(str)==0) return;
        if(str[0]=='I')
            continue;
        else if(str[1]=='M')   
        {
            m_visit_cache();
            m_visit_cache();
        }      
        else
            m_visit_cache();
    }
}

int main(int argc, char* argv[])
{
    getoptions(argc,argv);
    init();
    m_input();
    fclose(fp);
    freecache();
    printSummary(hit, miss, evictions);
    return 0;
}
```

### AC效果展示

在linux机器中输入

<code>make</code>

<code>./test-csim</code>

得到一份满分的AC图

```c
                        Your simulator     Reference simulator
Points (s,E,b)    Hits  Misses  Evicts    Hits  Misses  Evicts
     3 (1,1,1)       9       8       6       9       8       6  traces/yi2.trace
     3 (4,2,4)       4       5       2       4       5       2  traces/yi.trace
     3 (2,1,4)       2       3       1       2       3       1  traces/dave.trace
     3 (2,1,3)     167      71      67     167      71      67  traces/trans.trace
     3 (2,2,3)     201      37      29     201      37      29  traces/trans.trace
     3 (2,4,3)     212      26      10     212      26      10  traces/trans.trace
     3 (5,1,5)     231       7       0     231       7       0  traces/trans.trace
     6 (5,1,5)  265189   21775   21743  265189   21775   21743  traces/long.trace
    27

TEST_CSIM_RESULTS=27
```

至此part_A就结束啦

















