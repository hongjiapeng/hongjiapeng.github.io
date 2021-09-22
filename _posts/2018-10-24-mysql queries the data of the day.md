---
layout:     post
title:      MySQL查询当天当周当月的数据
subtitle:   
date:       2018-10-24
author:     郭新宇 
header-img: img/post-bg-debug.png
catalog: true
tags:
    - MySQL
    
---

> MySQL查询当天当周当月的数据

1、查询当天的数据

select * from 表名 where TO_DAYS(时间字段)=TO_DAYS(NOW());

2、查询当周的数据

select * from 表名 where YEARWEEK(DATE_FORMAT(时间字段,'%Y-%m-%d'))=YEARWEEK(NOW());

3、查询当月的数据

select * from 表名 where DATE_FORMAT(时间字段,'%Y%m')=DATE_FORMAT(CURDATE(),'%Y%m');

4、查询昨天的数据

select * from 表名 where TO_DAYS(NOW())-TO_DAYS(时间字段）=1;

5、查询最近7天的数据

select * from 表名 where DATE_SUB(CURDATE(),INTERVAL 7 DAY)<=DATE(时间字段);

6、查询当年的数据

select * from 表名 where YEAR(时间字段) =YEAR(NOW());

7、查询上周的数据

select * from 表名 whereYEARWEEK(DATE_FORMAT(时间字段,'%Y-%m-%d'))=YEARWEEK(NOW())-1;

8、查询上月的数据

select *from 表名 where PERIOD_DIFF(DATE_FORMAT(NOW(),'%Y%m'),DATE_FORMAT(时间字段,'%Y%m'))=1;


---
Refence：<br>

1. [MySQL数据库命名规范及约定](https://blog.csdn.net/sinat_29519243/article/details/70187040)<br>
2. [MySQL命名、设计及使用规范](https://www.biaodianfu.com/mysql-best-practices.html)<br>
3. [mysql 查询今天、昨天、上月、本月的数据](https://blog.csdn.net/wangjuan_01/article/details/51726588?utm_source=blogxgwz1)<br>
4. [mysql 查询当天当周当月的数据](https://blog.csdn.net/u010924845/article/details/51019483)<br>










