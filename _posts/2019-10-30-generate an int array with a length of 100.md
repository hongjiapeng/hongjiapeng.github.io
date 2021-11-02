---
layout: post
title: 产生一个int数组，长度为100，并向其中随机插入1-100，并且不能重复
subtitle:   
date:   2019-10-30
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
- 算法
---
记录下一道面试题


### 使用集合的方式实现

```c#
 public static int[] GenerateNumbers(int length)
    {
        var list = new List<int>();
        var random = new Random();

        while (true)
        {
            if (list.Count == length)
            {
                break;
            }
            var number = random.Next(1, length + 1);
            if (!list.Contains(number))
            {
                list.Add(number);
            }
        }
        return list.ToArray();
    }
```

上面的代码，可以实现题目的需求，但如果要生成的随机数非常多，如100万或者更多，这样的每次判断是否包含这样的一个数，势必会影响到性能。

网上找到一种更好的实现方式：

1. 把N个数放到容器A(int数组)中.

1. 从N个数中随机取出1个数放入容器B(int数组)中.

1. 把容器A中最后一个数与随机抽取的数对调 或者 把容器A中最后一个数覆盖随机抽取出来的数.

1. 这时从容器A(假设N个数,索引0 到 索引N-2)之间随机取一个数.再放入容器B中,重复此步骤.

说明:也就是第二次是从容器A中 第一个元素到倒数第二个元素 中随机取一个数.

这种好处是,随机数所取范围逐步缩小,而且杜绝了大数据时集合执行删除操作时产生的瓶颈.


### 使用集合的方式实现

```csharp
public static int[] GenerateNumbersPlus(int length)
    {
        int[] arrayA = new int[length];
        int[] arrayB = new int[length];

        for (int i = 0; i < length; i++)
        {
            arrayA[i] = i + 1;
        }

        var random = new Random();
        var end = length - 1;
        var minValue = 0;

        for (int i = 0; i < length; i++)
        {
            int maxValue = end + 1;
            int ranIndex = random.Next(minValue, maxValue);
            arrayB[i] = arrayA[ranIndex];
            arrayA[ranIndex] = arrayA[end];
            end--;
        }

        return arrayB;
    }
```

### 测试

```csharp

{       
        {
            Stopwatch stopwatch = new Stopwatch();
            stopwatch.Start();
            var numbers = GenerateNumbers(10000);
            stopwatch.Stop();
            Console.WriteLine($"普通方法，用了{stopwatch.ElapsedMilliseconds} ms");
        }

        {
            Stopwatch stopwatch = new Stopwatch();
            stopwatch.Start();
            var numbers = GenerateNumbersPlus(10000);
            stopwatch.Stop();
            Console.WriteLine($"优化后，用了{stopwatch.ElapsedMilliseconds} ms");
        }
}

```

结果：

```csharp
普通方法，用了100 ms
优化后，用了0 ms
```

### 总结

实现方式有很多种，但是如果能用高效的方式就用高效的方式实现。这种生成无重复的随机数，可以在运用在抽奖系统中。

参考：https://www.cnblogs.com/wolf-sun/