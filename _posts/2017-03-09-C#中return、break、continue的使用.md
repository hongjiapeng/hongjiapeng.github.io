---
layout:     post
title:      C#中return、break、continue的使用
subtitle:   return、break、continue的使用
date:       2017-03-09
author:     JP
header-img: img/post-bg-debug.png
catalog: true
tags:
    - c#关键字
---

> C#中return、break、continue的使用


****
# 1、break语句
break语句会使运行的程序立刻退出包含在最内层的循环或者退出一个switch语句。由于它是用来退出循环或者switch语句，所以只有当它出现在这些语句时，这种形式的break语句才是合法的。

**如果一个循环的终止条件非常复杂，那么使用break语句来实现某些条件比用一个循环表达式来表达所有的条件容易得多。**

      for (int i = 0; i <=10; i++)
		{
			if (i==6)
			{
				break;
			}
			Console.WriteLine(i);
		} 

**输出结果：**

12345


****
# 2、continue语句
continue语句和break语句相似。所不同的是，它不是退出一个循环，而是开始循环的一次新迭代。

**continue语句只能用在while语句、do/while语句、for语句、或者for/in语句的循环体内，在其它地方使用都会引起错误！**

      for (int i = 0; i <=10; i++)
		{
			if (i==6)
			{
				continue;
			}
			Console.WriteLine(i);
		} 

**输出结果：**

1234578910
****
# 3、return语句
return语句就是用于指定函数返回的值。return语句只能出现在函数体内，出现在代码中的其他任何地方都会造成语法错误！

**当执行return语句时，即使函数主体中还有其他语句，函数执行也会停止！**