---
title: bvh转json解析器
date: 2022/12/21
updated: 2022/12/24
cover: https://images6.alphacoders.com/128/thumbbig-1282130.webp
top_img: 
description: bvh转json解析器
swiper_index: 8 #置顶轮播图顺序，非负整数，数字越大越靠前
categories: 
---



# bvh转json解析器

## bonus版本

## 基本思路

编写一个解析器，使得输入bvh文件，然后以json格式进行输出。根据已经给出的结构

```cpp
struct joint {
    string name;
    double offset_x, offset_y, offset_z;
    vector<joint*> children;
    vector<string> channels;
    vector<vector<double>> motion;
    string joint_type;
};
```

首先写一个<code>init()</code>函数读取sample.bvh中的字符串，我们可以将sample.bvh文件中的字符串读取到<code>vector<string> vec</code>中备用，然后将每帧的运动数据存储在<code>vector<vector<double>> mot;</code>中。

然后我们需要将vec中已经被读取到的若干字符串按照规范存在joint中，在这里，应用递归实现的一个函数<code>joint* dfs(int floor)</code>处理vec中的字符串，在读取完vec中的字符串后，然后在<code>void inputdatas(joint* root,int& sta)</code>函数中将mot中存储的每一帧的数据读取到joint的<code>  vector<vector<double>> motion;</code>中。

数据读取存储完毕后，然后就是把我们存储在joint中的数据以json的格式输出到"output.json"中，在输出的过程中，我们定义了一个函数<code>void generate_json(joint* root,int blank)</code>，其中blank表示每行输出时应该在前面输出的空白格的长度。

## 将信息存储进入vector<string> vec的字符串处理方案

这里我们采用文件流的读取方式，打开文件argv[1]，然后前面部分正常读取就行了，当读取到”Frames:"时，我们就中止当前读取，并进行下一步每一帧的运动数据的读取处理操作。在读取浮点数的时候，我们可以先将其以字符串的方式进行读取，然后利用C++的atof()函数将字符串转化为浮点数，存储到<code>vector<vector<double>> mot;</code>中。

### 实现代码：

```cpp
void init(char** argv)
{
    ifstream file(argv[1]);
    int Frames = 0x3f3f3f;
    int m = 0;
    for (int i = 0; i < Frames; i++)
    {
        string s;
        file >> s;
        if (vec.size() && vec.back() == "Frames:")
        {
            for (int j = 0; j < vec.size(); j++)
            {
                if (vec[j] == "CHANNELS")
                {
                    m += atoi(vec[j + 1].c_str());
                }
            }
            Frames = atoi(s.c_str());
            break;
        }
        vec.push_back(s);
    }
    vec.pop_back();
    vec.pop_back();
    string s;
    file >> s;
    file >> s;
    file >> s;
    Fra_time = atof(s.c_str());
    for (int i = 0; i < Frames; i++)
    {
        vector<double> temp(m);
        for (int j = 0; j < m; j++)
        {
            file >> temp[j];
        }
        mot.push_back(temp);
    }
    file.close();
}
```

## 将信息存储进入joint的字符串处理方案

核心的采用递归的方式进行遍历，并采取多叉树的方式进行存储，关键的利用{和}，结合栈的特点进行递归往下进行或者结束递归操作。因为我们是把sample.bvh中的字符存储在了<code>vector<string> vec</code>中，所以我们还应该用一个单指针p指向当前应该读取的数据。如果当前p指向的是{则读取{}中包含的各种信息，new一个子节点，并将所有的数据信息读取到这个子节点中，并向下一层进行递归。如果遇到}，那么就结束递归，return 当前的joint。

### 实现代码

```cpp
vector<string> vec;
vector<vector<double>> mot;
int p = 1;
double Fra_time = 0;

joint* dfs(int floor)
{
    joint* root = new joint;
    root->joint_type = vec[p];
    p++;
    root->name = vec[p];
    p++;
    string a(floor, ' ');
    if (vec[p] == "{")
    {
        p += 2;
        root->offset_x = atof(vec[p].c_str());
        root->offset_y = atof(vec[p + 1].c_str());
        root->offset_z = atof(vec[p + 2].c_str());
        if (root->joint_type == "End")
        {
            p += 4;
            return root;
        }
        p += 4;
        int up = atoi(vec[p].c_str());
        for (int i = 0; i < up; i++)
        {
            p++;
            root->channels.push_back(vec[p]);
        }
        p++;
        while (vec[p] != "}")
            root->children.push_back(dfs(floor + 1));
    }
    p++;
    return root;
}
```

## 将joint中存储的数据以json格式输出的操作方案

joint中的数据使用的是多叉树的存储方式，所以我们在生成json文件的时候同样采用递归的方式进行。在遍历到每个树的节点的时候，我们优先输出这个节点保存的各种信息，然后进行往下递归处理子节点的各种信息。在输出json文件的时候，我们要考虑组织好输出的格式，在输出前面的空白缩进的时候，我们可以用一个blank记录当前需要输出的空白数目。

 ### 实现代码

```cpp
void generate_json(joint* root, int blank)
{
    string ba(blank, ' ');
    outfile << ba << "{\n";
    ba += "    ";
    outfile << ba << "\"type\": " << "\"" << root->joint_type << "\",\n";
    outfile << ba << "\"name\": " << "\"" << root->name << "\",\n";
    outfile << ba << "\"offset\": [" << root->offset_x << ", " <<
        root->offset_y << ", " << root->offset_z << "],\n";
    outfile << ba << "\"channels\": " << "[";
    for (int i = 0; i < root->channels.size(); i++)
    {
        outfile << "\"" << root->channels[i] << "\"";
        if (i != root->channels.size() - 1)
            outfile << ", ";
    }
    outfile << "],\n";
    outfile << ba << "\"motion\": [\n";
    ba += "    ";
    for (int i = 0; i < root->motion.size(); i++)
    {
        outfile << ba << "[";
        for (int j = 0; j < root->motion[i].size(); j++)
        {
            outfile << root->motion[i][j];
            if (j != root->motion[i].size() - 1)
                outfile << ", ";
        }
        outfile << "]";
        if (i != root->motion.size() - 1)
            outfile << ",";
        outfile << "\n";
    }
    for (int i = 0; i < 4; i++) ba.pop_back();
    outfile << ba << "],\n";
    outfile << ba << "\"children\": [\n";
    if (root->children.size() == 0)
        outfile << "\n";
    for (int i = 0; i < root->children.size(); i++)
    {
        auto g = root->children[i];
        generate_json(g, blank + 8);
        if (i != root->children.size() - 1)
            outfile << ",";
        outfile << "\n";
    }
    outfile << ba << "]" << "\n";
    for (int i = 0; i < 4; i++) ba.pop_back();
    outfile << ba << "}" << "\n";
}
```



## 完成PJ的收获

* 学习到了什么是bvh文件，对以后学习unity中人体骨骼运动具有很大的帮助
* 学习到了json文件的格式，以及如何调用json文件中的数据
* 熟练了建立多叉树的步骤，以及读取多叉树的步骤
* 学习到了Makefile文件的组织以及各行代码的意义



## 代码展示

### bvh_parser.h

```cpp
#ifndef BVH_PARSER_H
#define BVH_PARSER_H
#include<string>
#include<vector>

using std::string;
using std::vector;


struct joint {
    string name;
    double offset_x, offset_y, offset_z;
    vector<joint*> children;
    vector<string> channels;
    vector<vector<double>> motion;
    string joint_type;
};

struct META {
    int frame;
    double frame_time;
};



void jsonify(joint*, META);

#endif
```

### bvh_parser.cpp

```cpp
#include<fstream>
#include"bvh_parser.h"

// a naive bvh parser

using std::ifstream;

vector<string> vec;
vector<vector<double>> mot;
int p = 1;
double Fra_time = 0;

joint* dfs(int floor)
{
    joint* root = new joint;
    root->joint_type = vec[p];
    p++;
    root->name = vec[p];
    p++;
    string a(floor, ' ');
    if (vec[p] == "{")
    {
        p += 2;
        root->offset_x = atof(vec[p].c_str());
        root->offset_y = atof(vec[p + 1].c_str());
        root->offset_z = atof(vec[p + 2].c_str());
        if (root->joint_type == "End")
        {
            p += 4;
            return root;
        }
        p += 4;
        int up = atoi(vec[p].c_str());
        for (int i = 0; i < up; i++)
        {
            p++;
            root->channels.push_back(vec[p]);
        }
        p++;
        while (vec[p] != "}")
            root->children.push_back(dfs(floor + 1));
    }
    p++;
    return root;
}

void init(char** argv)
{
    ifstream file(argv[1]);
    int Frames = 0x3f3f3f;
    int m = 0;
    for (int i = 0; i < Frames; i++)
    {
        string s;
        file >> s;
        if (vec.size() && vec.back() == "Frames:")
        {
            for (int j = 0; j < vec.size(); j++)
            {
                if (vec[j] == "CHANNELS")
                {
                    m += atoi(vec[j + 1].c_str());
                }
            }
            Frames = atoi(s.c_str());
            break;
        }
        vec.push_back(s);
    }
    vec.pop_back();
    vec.pop_back();
    string s;
    file >> s;
    file >> s;
    file >> s;
    Fra_time = atof(s.c_str());
    for (int i = 0; i < Frames; i++)
    {
        vector<double> temp(m);
        for (int j = 0; j < m; j++)
        {
            file >> temp[j];
        }
        mot.push_back(temp);
    }
    file.close();
}

void inputdatas(joint* root, int& sta)
{
    for (int i = 0; i < mot.size(); i++)
    {
        vector<double> temp;
        for (int j = sta; j < sta + root->channels.size(); j++)
        {
            temp.push_back(mot[i][j]);
        }
        root->motion.push_back(temp);
    }
    sta += root->channels.size();
    for (auto& g : root->children)
    {
        inputdatas(g, sta);
    }
}

int main(int argc, char** argv) {
    joint* root = new joint;
    META meta_data;

    //do something
    init(argv);
    root = dfs(1);
    int sta = 0;
    inputdatas(root, sta);
    meta_data.frame = mot.size();
    meta_data.frame_time = Fra_time;

    jsonify(root, meta_data);
    return 0;
}
```

### json.cpp

```cpp
#include<iostream>
#include<fstream>
#include"bvh_parser.h"

using std::ofstream;
ofstream outfile;
void generate_json(joint* root, int blank)
{
    string ba(blank, ' ');
    outfile << ba << "{\n";
    ba += "    ";
    outfile << ba << "\"type\": " << "\"" << root->joint_type << "\",\n";
    outfile << ba << "\"name\": " << "\"" << root->name << "\",\n";
    outfile << ba << "\"offset\": [" << root->offset_x << ", " <<
        root->offset_y << ", " << root->offset_z << "],\n";
    outfile << ba << "\"channels\": " << "[";
    for (int i = 0; i < root->channels.size(); i++)
    {
        outfile << "\"" << root->channels[i] << "\"";
        if (i != root->channels.size() - 1)
            outfile << ", ";
    }
    outfile << "],\n";
    outfile << ba << "\"motion\": [\n";
    ba += "    ";
    for (int i = 0; i < root->motion.size(); i++)
    {
        outfile << ba << "[";
        for (int j = 0; j < root->motion[i].size(); j++)
        {
            outfile << root->motion[i][j];
            if (j != root->motion[i].size() - 1)
                outfile << ", ";
        }
        outfile << "]";
        if (i != root->motion.size() - 1)
            outfile << ",";
        outfile << "\n";
    }
    for (int i = 0; i < 4; i++) ba.pop_back();
    outfile << ba << "],\n";
    outfile << ba << "\"children\": [\n";
    if (root->children.size() == 0)
        outfile << "\n";
    for (int i = 0; i < root->children.size(); i++)
    {
        auto g = root->children[i];
        generate_json(g, blank + 8);
        if (i != root->children.size() - 1)
            outfile << ",";
        outfile << "\n";
    }
    outfile << ba << "]" << "\n";
    for (int i = 0; i < 4; i++) ba.pop_back();
    outfile << ba << "}" << "\n";
}

void jsonify(joint* root, META meta_data) {

    outfile.open("output.json");

    // output the root and meta_data
    outfile << "{\n";
    outfile << "    \"frame\": " << meta_data.frame << ",\n";
    outfile << "    \"frame_time\": " << meta_data.frame_time << ",\n";
    outfile << "    \"joint\":\n";
    generate_json(root, 8);
    outfile << "}\n";

    outfile.close();
}
```

