---
layout: post
title: C# yield关键字
subtitle:   
date:   2019-07-24
author: JP
header-img: img/post-bg-swift.jpg
catalog: true
tags:
- C#关键字

---

>  **C# yield关键字**


还是和以前一样，我先上代码，请大家先拿到我的代码或者你跟着敲，运行看效果，以及理解每行带代码的作用。我们要带着为什么要用Yield这个关键字，不用可以吗这个目的去学知识，我相信会更加的有意思。

首先我贴出平时正常输出偶数集合的办法

```c#
    class Program
    {
        static List<int> _numArray;

        static Program()
        {
            _numArray = new List<int>();
            for (int i = 0; i <= 10; i++)
                _numArray.Add(i);
        }

        static void Main(string[] args) => TestMethod();

        private static void TestMethod()
        {
            foreach (var item in GetAllEvenNumber())
                Console.WriteLine(item);
            Console.ReadKey();
        }

        private static IEnumerable<int> GetAllEvenNumber()
        {
            #region 不使用yield
            //var result = new List<int>();
            //foreach (var num in _numArray)
            //{
            //    if (num % 2 == 0)
            //        result.Add(num);
            //}
            //return result;
            #endregion

            #region 使用yield
            foreach (var num in _numArray)
            {
                if (num % 2 == 0)
                    yield return num;
            }
            yield break;//当前集合已经遍历完毕，我们就跳出当前函数，其实你不加也可以
            //这个作用就是提前结束当前函数，就是说这个函数运行完毕了。 
            #endregion
        }
    }

```
大家测试了2个代码结果没，是不是都可以正确拿到全部偶数集合，具体我需要你们测，这样进步快，才会真是学会。只看不练假把戏。

现在我们说他们的区别： 
这个才是真正要学的地方 
我们需要下断点


Reference: [彻底搞懂C#之Yield Return语法的作用和好处](https://blog.csdn.net/qq_33060405/article/details/78484825)<br>