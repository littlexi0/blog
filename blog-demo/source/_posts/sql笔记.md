---
title: SQL
date: 2022/12/02
updated: 2022/12/02
cover: https://images3.alphacoders.com/973/thumbbig-973586.webp
top_img: 
description: sql学习笔记
swiper_index: 4 #置顶轮播图顺序，非负整数，数字越大越靠前
categories: sql
---



数据类型

INT 整数型

VARCHAR 字符串

DECIMAL(a,b) 浮点型（a代表位数，b代表小数位位数）

BLOB 图片、影片、档案

DATA 日期 `XXXX-MM-DD`

TIMESTAMP 记录时间 `XXXX-MM-DD  HH-MM-SS`

创建数据库

<code>CREATE DATABASE `sql_tutorial`;</code>

查看数据库

<code color="red">SHOW DATABASES;</code>

移除数据库

<code>DROP DATABASE `sql_tutorial`;</code>

使用数据库

<code>USE `sql_tutorial`;</code>

创建表单

```sql
CREATE TABLE student(
	`student_id` INT auto_increment, --auto_increment指可以自动修改键的值
    `name` varchar(20) not null,
    `major` varchar(20) unique,
    primary key(`student_id`) --primary key 指索引关键字
);
```

删除表单

<code>drop table `student`;  </code>

显示表单

<code>describe `student`;</code>

插入表单内容

<code>alter table `student` add gpa decimal(3,2);</code>

删除表单内容

<code>alter table `student` drop column gpa;</code>

显示表单具体内容

<code>select * from `student`;</code>

插入表单具体内容的三种方式

```sql
insert into `student` values(1,"小夕","家妻修养指南");

insert into `student` values(2,"德克萨斯","剑雨");

insert into `student` (`name`,`major`) values("羽毛笔","卖萌"); --仅限primary key有自动自增的时候
```

更新表单中的内容

```sql
update`student`
set `major` = "近战"
where `major` = "术士";
```

```sql
update `student`
set `major` = "近战"
where `student_id` =  1;
```

删除表单

```sql
delete from `student`
where `student_id` in (1,3);
```

```sql
delete from `student`
where `student_id` % 2 = 1;
```

从表单中取得资料(三种方法)

```sql
-- 取得资料
select `name` , `major` from `student`;

select * from `student` 
order by `student_id` desc  -- 默认asc由低到高，desc由高到低
limit 2; -- 限制取回的资料的数目

select * from `student` 
order by `student_id` , 'score' desc; -- 先根据student_id排序，如果student_id 一样的话，就按照score进行排序                                 
```

aggregate functions 聚合函数

-- 1、取得员工人数
```select count(*) from `employee`;```

-- 2、取得所有出生日1970-01-01之后的女性员工人数
```select count(*) from `employee` where `birth_data` > "1970-01-01" and `sex`='F';```

-- 3、取得所有员工的平均薪水
```select avg(`salary`) from `employee`;```

-- 4、取得所有员工薪水的总和
```select sum(`salary`) from `employee`;```

-- 5、取得薪水最高的员工
```select max(`salary`) from `employee`;```

-- 6、取得薪水最低的员工
```elect min(`salary`) from `employee`;```



wildcards 万用字元   %代表多个字元 ， _ 代表一个字元

1、取得电话号码位数是114514的客户

```sql
select *
from `client`
where `phone` like "%114514";
```

2、取得姓小的用户

```sql
select *
from `client`
where `client_name` like "小%";
```

3、取得生日在12月的员工

```sql
select *
from `employee`
where "birth_data" like "_____12%";  -- 2000-12-19
```

union 联集

1、员工名字 union 客户名字

```sql
select `name`
from `employee`
union 
select `client_name`
from `client`
union
select `branch_name`
from `branch`;
```

2、员工id + 员工名字 union 客户id + 客户名字

```sql
select `emp_id` as `changed_id`, `name` as `changed_name`
from `employee`
union 
select `client_id`,`client_name`
from `client`;
```

3、员工薪水 union 销售金额

```sql
select `salary` as `changed_salary`
from `employee`
union 
select `total_sales`
from `work_with`;
```



join 连接:可以帮助我们把两个表格连接在一起

取得所有部门经理的名字

* left : 条件成不成立都会传回左边的表格
* right ；条件成不成立都会传回右边的表格

```sql
select `emp_id`,`name`,`branch_name`
from `employee` left join `branch`
on `employee`.`emp_id` = `branch`.`manager_id`;
```

subquery 子查询
1、找出研发部门的经理名字

```sql
select `name`
from `employee`
where `emp_id` = (
	select `manager_id`
	from `branch`
	where `branch_name` = "研发"
);
```

2、找出对单一客户销售金额超过50000的员工的名字

```sql
select `name`
from `employee`
where `emp_id` in (
	select `emp_id`
	from `works_with`
	where `total_sales` > 50000
);
```

on delete

删除之后把联系的table设置为null

```sql
create table `branch`(
	`brach_id` int primary key,
    `branch_name` varchar (20),
    `manager_id` int ,
    foreign key(`manager_id`) references `employee`(`emp_id`) on delete set null
);
```

删除之后把联系的table对应元素也删除了

```sql
create table `works_with`(
	`brach_id` int primary key,
    `branch_name` varchar (20),
    `manager_id` int ,
    foreign key(`manager_id`) references `emp_id` on delete cascade,
	foreign key(`client_id`) references `emp_id` on delete cascade
);
```







