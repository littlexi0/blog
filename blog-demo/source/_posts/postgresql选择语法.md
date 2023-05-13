---
title: postgresql查询语法
date: 2023/3/14
updated: 2023/3/14
cover: https://images3.alphacoders.com/128/thumbbig-1280700.webp
top_img: 
description: postgresql查询语法简单使用
swiper_index: 3 #置顶轮播图顺序，非负整数，数字越大越靠前
categories: sql
---

# 数据库查询语法

## （1） 数据处理--用 select 语句验证以上语句的正确性 

① 将 STU 表中指定行(sid='16300240002')的手机号字段 MOBILE、班级号 CLSNO 字段用 update 命令修改为自己的 手机号及所在的班号 

② 将自己的分数减少 2 分 

```postgresql
update stu 
set mobile = '17882566220', clsno='计算机科学与技术1班'
where sid='16300240002';

update grade
set score = score-2
where sid='16300240002';
```

## （2）单表查询

 ① 查询指定列：

 · 查询全体学生的姓名、学号、班级编号 

```postgresql
select sname , sid , clsno from stu;
```

· 查询全体学生的全部信息 

```postgresql
select * from stu;
```

② 查询经过计算的列

 · 查询全体学生的学号、姓名及出生年份

```postgresql
select sid , sname,birth  from stu;
```

 · 查询全体学生的学号、姓名及年龄 

```postgresql
select sid , sname,2023-to_number(to_char(birth, 'YYYY'),'9999') as age from stu;
```

③ 查询若干元组

 · 查询所有有成绩的学生学号（not null）

```postgresql
select sid from grade where score is not NULL; 
```

· 上述查询去掉重复的行（用 distinct）

```postgresql
select distinct sid from grade where score is not NULL;
```

 ④ 条件查询

 · 查询课程编号为 C01 的所有学生成绩

```postgresql
select score from grade where cid='C01';
```

 · 查询考试分数在 85 分以上的所有成绩 

```postgresql
select score from grade where score>85;
```

⑤ 集合查询 

· 查询课程编号为 C01、C02、C03 的所有成绩 （要求用 二种方法，in 及 … or …）

```postgresql
select score from grade where cid='C01' or cid='C02' or cid='C03';
select score from grade where cid in ('C01','C02','C03');
```

 ⑥ LIKE 匹配

· 查询包含“设计”的课程编号和名称 

```postgresql
select cid , cname from course where cname like '%设计%';
```

⑦ NULL 值查询 

· 查询缺少考试成绩（成绩为空或者没有成绩）的学生学 号及课程号

```postgresql
select sid , cid from grade where score is null;
```

 ⑧ 多重条件查询

 · 查询课程编号为 C01 且分数在 90 分以上的学生成绩 

```postgresql
select score from grade where cid='C01' and score>90;
```

⑨ 其他查询

 · 查询各学生考试分数，按考试成绩降序排列

```postgresql
select score from grade where score is not null order by score desc ;
```

 · 查询学生总数（用 count）

```postgresql
select count(*) from stu;
```

 · 计算 C01 课程所有考试成绩分数总和（用 sum）、平 均分（用 avg） 

```postgresql
select sum(score) , avg(score) from grade where cid='C01' and score is not null;
```

· 计算每门课程平均分、最高分、最低分（用 avg、 max、min，并结合 group by） 

```postgresql
select cid,avg(score),max(score),min(score) from grade group by cid;
```

· 查询学生数在 5 人以上的班级编号及学生数（用 count 结合 group by…HAVING）

```postgresql
select clsno,count(*) as student_number from stu group by clsno having count(*)>5;
```

### (3)、 拓展与思考：多表查询 

① 等值连接 ·查询每个学生及其学习成绩的情况（用自然连接和外连接二种 SQL 语句）

```postgresql
select * from stu,grade where stu.sid=grade.sid;


select * from stu left join grade on stu.sid=grade.sid
union
select * from stu right join grade on stu.sid=grade.sid;
```

 ② 复合条件连接（WHERE 子句中含多个连接条件） ·查询选修 C01 课程且成绩在 85 分以上的所有学生 ·查询每个学生的学号、姓名、选修的课程名及成绩

```postgresql
select * from grade where cid='C01' and score > 85;
```
