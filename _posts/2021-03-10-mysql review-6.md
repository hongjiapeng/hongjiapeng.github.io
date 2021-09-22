---
layout: post
title: MySQL复习宝典-6
subtitle:   
date:   2020-03-10
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
- 面试题
- MySQL
---

## 其他

### 视图

一、含义

mysql5.1版本出现的新特性，本身是一个虚拟表，它的数据来自于表，通过执行时动态生成。

好处：

1、简化sql语句

2、提高了sql的重用性

3、保护基表的数据，提高了安全性

二、创建

```sql
create view 视图名
as
查询语句;
```

三、修改

方式一：

```sql
create or replace view 视图名
as
查询语句;
```

方式二：

```sql
alter view 视图名
as
查询语句
```

四、删除

```sql
drop view 视图1，视图2,...;
```

五、查看

```sql
desc 视图名;
show create view 视图名;
```

六、使用

1.插入
insert

2.修改
update

3.删除
delete

4.查看
select

注意：视图一般用于查询的，而不是更新的，所以具备以下特点的视图都不允许更新

①包含分组函数、group by、distinct、having、union、

②join

③常量视图

④where后的子查询用到了from中的表

⑤用到了不可更新的视图


七、视图和表的对比
		关键字		是否占用物理空间			使用
视图	view		占用较小，只保存sql逻辑		一般用于查询
表		table		保存实际的数据			增删改查

### 变量

分类

一、系统变量

说明：变量由系统提供的，不用自定义

语法：

①查看系统变量
show 【global|session 】variables like ''; 如果没有显式声明global还是session，则默认是session

②查看指定的系统变量的值
select @@【global|session】.变量名; 如果没有显式声明global还是session，则默认是session

③为系统变量赋值

方式一：
```sql
set 【global|session 】 变量名=值; 如果没有显式声明global还是session，则默认是session
```
方式二：
```sql
set @@global.变量名=值;
set @@变量名=值；
```

1、全局变量

服务器层面上的，必须拥有super权限才能为系统变量赋值，作用域为整个服务器，也就是针对于所有连接（会话）有效

2、会话变量

服务器为每一个连接的客户端都提供了系统变量，作用域为当前的连接（会话）

二、自定义变量
说明：

1、用户变量

作用域：针对于当前连接（会话）生效

位置：begin end里面，也可以放在外面

使用：

```sql
①声明并赋值：
set @变量名=值;或
set @变量名:=值;或
select @变量名:=值;
```

②更新值

方式一：

```sql
	set @变量名=值;或
	set @变量名:=值;或
	select @变量名:=值;
```

方式二：

```sql
	select xx into @变量名 from 表;
```

③使用

```sql
select @变量名;
```

2、局部变量

作用域：仅仅在定义它的begin end中有效

位置：只能放在begin end中，而且只能放在第一句

使用：

①声明
declare 变量名 类型 【default 值】;

②赋值或更新

方式一：

```sql
	set 变量名=值;或
	set 变量名:=值;或
	select @变量名:=值;
```

方式二：

```sql
select xx into 变量名 from 表;
```
③使用
select 变量名;


### 存储过程和函数

#### 存储过程

一、创建 ★
```sql
create procedure 存储过程名(参数模式 参数名 参数类型)
begin
		存储过程体
end
```
注意：
1.参数模式：in、out、inout，其中in可以省略
2.存储过程体的每一条sql语句都需要用分号结尾

二、调用

```sql
call 存储过程名(实参列表)
举例：
调用in模式的参数：call sp1（‘值’）;
调用out模式的参数：set @name; call sp1(@name);select @name;
调用inout模式的参数：set @name=值; call sp1(@name); select @name;
```

三、查看
```sql
show create procedure 存储过程名;
```

四、删除
```sql
drop procedure 存储过程名;
```


#### 函数

一、创建
```sql
create function 函数名(参数名 参数类型) returns  返回类型
begin
	函数体
end
```

注意：函数体中肯定需要有return语句
二、调用
```sql
select 函数名(实参列表);
```
三、查看
```sql
show create function 函数名;
```
四、删除
```sql
drop function 函数名；
```

### 流程控制结构

#### 分支结构

特点：

1、if函数

功能：实现简单双分支

语法：

if(条件，值1，值2)

位置：

可以作为表达式放在任何位置

2、case结构

功能：实现多分支

语法1：

```sql
case 表达式或字段
when 值1 then 语句1;
when 值2 then 语句2；
..
else 语句n;
end [case];
```

语法2：

```sql
case 
when 条件1 then 语句1;
when 条件2 then 语句2；
..
else 语句n;
end [case];

```

位置：

可以放在任何位置，
如果放在begin end 外面，作为表达式结合着其他语句使用
如果放在begin end 里面，一般作为独立的语句使用

3、if结构

功能：实现多分支

语法：

```sql

if 条件1 then 语句1;
elseif 条件2 then 语句2;
...
else 语句n;
end if;
```

位置：

只能放在begin end中

#### 循环结构

位置：
只能放在begin end中

特点：都能实现循环结构

对比：

①
这三种循环都可以省略名称，但如果循环中添加了循环控制语句（leave或iterate）则必须添加名称

②
loop 一般用于实现简单的死循环

while 先判断后执行

repeat 先执行后判断，无条件至少执行一次


1、while
语法：

```sql
【名称:】while 循环条件 do
		循环体
end while 【名称】;
```

2、loop
语法：

```sql
【名称：】loop
		循环体
end loop 【名称】;
```

3、repeat
语法：

```sql
【名称:】repeat
		循环体
until 结束条件 
end repeat 【名称】;
```

二、循环控制语句

leave：类似于break，用于跳出所在的循环<br>
iterate：类似于continue，用于结束本次循环，继续下一次