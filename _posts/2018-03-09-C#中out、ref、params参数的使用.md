---
layout:     post
title:      C#中传值/输出/引用/数组具名/可选参数/拓展方法的使用
subtitle:   C#中out、ref、params参数的使用
date:       2017-09-25
author:     JP
header-img: img/post-bg-debug.png
catalog: true
tags:
    
---

> C#中传值/输出/引用/数组具名/可选参数/拓展方法的使用


转载请注明出处：http://blog.csdn.net/hongjiapeng/article/details/78088302
****
#1、out参数。
如果你在一个方法中，返回多个相同类型的值的时候，可以考虑返回一个数组。但是，如果返回多个不同类型的值的时候，返回数组就不行了，那么这个时候，我们可以考虑使用out参数。out参数就侧重于在一个方法中可以返回多个不同类型的值。

**不使用out参数，写一个方法 求一个数组中的最大值、最小值、总和、平均值**

     static void Main(string[] args)
    {
        int[] numbers = { 1,2,3,4,5,6,7,8,9,10};	//将要返回的4个值，放到一个数组中返回
        int[] result= GetMaxMinSumAvg(numbers);
        Console.WriteLine("该数组的最大值为{0},最小值为{1},总和为{2}，平均值为{3}",result[0],result[1],result[2],result[3]);
        Console.ReadKey();
    }

    /// <summary>
    /// 求一个数组的最大值、最小值、总和、平均值
    /// </summary>
    /// <param name="nums">要求的数组</param>
    static int[] GetMaxMinSumAvg(int[] nums) 
    {
        int[] temp=new int[4];
        //假设res[0]为最大值 res[1]为最小值 res[2]为总和 res[3]为平均值
        temp[0] = nums[0];//max  
        temp[1] = nums[0];//min
        temp[2] = 0;//sum
        temp[3] = 0;//avg
        for (int i = 0; i < nums.Length; i++)
        {
            if (nums[i] > temp[0])
            {
                temp[0] = nums[i];
            } if (nums[i] < temp[1])
            {
                temp[1] = nums[i];
            }
            temp[2] += nums[i];//总和
        }
        temp[3] = temp[2] / nums.Length;//平均值
        return temp;
    }     

****
**计算结果如下：**

![计算结果](http://img.blog.csdn.net/20170925200418660?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaG9uZ2ppYXBlbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

****
**但是这样写有一个问题，上面我们返回的这4个值都是int类型，所以可以放到int类型的数组里面。假如在上面那个GetMaxMinSumAvg(int[] nums)函数里面，我想要你再额外的返回给我一个字符串，该怎么做呢？**

```
.
.
.
temp[0] = nums[0];//max  
temp[1] = nums[0];//min
temp[2] = 0;//sum
temp[3] = 0;//avg
//假如还需要返回得调用者一个字符串和一个布尔类型的值（下面两行代码），该怎么做呢？
string userName="admin";
bool b=true;
for (int i = 0; i < nums.Length; i++)
.
.
.
            
```
****
**要返回的多个值是同一种类型可以用数组，但要返回的类型不是一种，该怎么做呢？这就需要用到我们的out参数。**
**下面再写一个方法**
```
 static void Main(string[] args)
        {
            int[] numbers = { 1,2,3,4,5,6,7,8,9,10};
            int max1;//这里可以只定义、不赋值，因为在调用下面的TestOutPram函数的时候已经给赋了
            int min1;
            int sum1;
            int avg1;
            bool b;
            string s;
            double d;
            //上面6个参数的值是从TestOutPram函数多余返回来的，out是多余返回的值
            TestOutPram(numbers, out max1, out min1, out sum1, out avg1, out b, out s, out d);
            Console.WriteLine("该数组的最大值为{0},最小值为{1},总和为{2}，平均值为{3},布尔值为{4},字符串为{5},浮点值为{6}", max1, min1, sum1, avg1, b, s, d);
            Console.ReadKey();
        }

        /// <summary>
        /// 计算一个整数数组的最大值、最小值、平均值、总和
        /// </summary>
        /// <param name="nums">要求值得数组</param>
        /// <param name="max">多余返回的最大值</param>
        /// <param name="min">多余返回的最小值</param>
        /// <param name="sum">多余返回的总和</param>
        /// <param name="avg">多余返回的平均值</param>
        public static void TestOutPram(int[] nums, out int max, out int min, out int sum, out int avg, out bool b, out string s, out double d)
        {
            //out参数要求在方法的内部必须为其赋值
            max = nums[0];
            min = nums[0];
            sum = 0;
            for (int i = 0; i < nums.Length; i++)
            {
                if (nums[i] > max)
                {
                    max = nums[i];
                }
                if (nums[i] < min)
                {
                    min = nums[i];
                }
                sum += nums[i];
            }
            avg = sum / nums.Length;

            b = true;
            s = "123";
            d = 3.13;
        }
```
**使用out参数后，运行结果：**
![使用out参数后](http://img.blog.csdn.net/20170925194443077?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaG9uZ2ppYXBlbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
****
**下面使用out参数做一个简单的登录案例**

```
 static void Main(string[] args)
{
    //分别的提示用户输入用户名和密码
    //你写一个方法来判断用户输入的是否正确
    //返回给用户一个登陆结果，并且还要单独的返回给用户一个登陆信息
    //如果用户名错误，除了返回登陆结果之外，还要返回一个 "用户名错误"
    //如果密码错误，除了返回登陆结果之外，还要返回一个 "密码错误"
    Console.WriteLine("请输入用户名");
    string userName = Console.ReadLine();
    Console.WriteLine("请输入密码");
    string userPwd = Console.ReadLine();
    string msg;
    bool b = IsLogin(userName, userPwd, out msg);
    Console.WriteLine("登陆结果{0}", b);
    Console.WriteLine("登陆信息{0}", msg);
    Console.ReadKey();
}

/// <summary>
/// 判断登陆是否成功
/// </summary>
/// <param name="name">用户名</param>
/// <param name="pwd">密码</param>
/// <param name="msg">多余返回的登录信息</param>
/// <returns>返回登录结果</returns>
static bool IsLogin(string name,string pwd,out string msg) 
{
    if (name=="admin"&&pwd=="123456")
    {
        msg = "登录成功！";
        return true;
    }
    else if (name=="admin")
    {
        msg = "密码错误！";
        return false;
    }
    else if(pwd=="123456")
    {
        msg = "用户名不正确！";
        return false;
    }
    else
    {
        msg = "未知错误";
        return false;
    }
}
```
**1、用户名和密码都不正确**

![这里写图片描述](http://img.blog.csdn.net/20170925195855718?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaG9uZ2ppYXBlbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**2、用户名正确，密码都正确**

![这里写图片描述](http://img.blog.csdn.net/20170925195759705?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaG9uZ2ppYXBlbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**3、用户名不正确，密码正确**

![这里写图片描述](http://img.blog.csdn.net/20170925200233771?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaG9uZ2ppYXBlbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**4、用户名、密码都正确**

![这里写图片描述](http://img.blog.csdn.net/20170925200102937?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaG9uZ2ppYXBlbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

****
**下面自己写一个方法，来实现和TryParse函数一样的功能。**

```
 class Program
    {        
        static void Main(string[] args)
        {
            int num1;//这里没有必要赋初值，num的值是从方法里多余返回的
            int num2;
            //将字符串转换成int类型，转换后的结果返回布尔类型，将成功或失败后的值 赋值给rusult
            bool a = int.TryParse("123a",out num1);          
            Console.WriteLine(a);
            Console.WriteLine(num1);
            bool b = MyTryParse("1244", out num2);
            Console.WriteLine(b);            
            Console.WriteLine(num2);
            Console.ReadKey();
        }
        /// <summary>
        /// 将字符串转Int
        /// </summary>
        /// <param name="s">要转换的字符串</param>
        /// <param name="result">多余返回的值</param>
        /// <returns>返回成功或失败</returns>
        static bool MyTryParse(string s,out int result) 
        {
             result = 0;
             try
             {
                 result = Convert.ToInt32(s);
                 return true;
             }
             catch 
             {
                 return false;
             }
        }
    }
```
![这里写图片描述](http://img.blog.csdn.net/20170926093026215?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaG9uZ2ppYXBlbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
****
#2)、ref参数
能够将一个变量带入一个方法中进行改变，改变完成后，再将改变后的值带出方法。ref参数要求在方法外必须为其赋值，而方法内可以不赋值。
****

```
 class Program
    {
        static void Main(string[] args)
        {
            //假设小明的月薪为5000
            double salary = 5000;
            Reward(salary);
            Console.WriteLine(salary);
            Console.ReadKey();
        }
        /// <summary>
        /// 奖金
        /// </summary>
        /// <param name="s">薪水</param>
        static void Reward(double s) 
        {
            s += 500;
        }
        /// <summary>
        /// 罚款
        /// </summary>
        /// <param name="s">薪水</param>
        static void Fine(double s)
        {
            s -= 500;
        }
```
**上面代码运行后打印的结果：**

![这里写图片描述](http://img.blog.csdn.net/20170926095721900?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaG9uZ2ppYXBlbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
****
**可以看到，月薪还是5000，因为调用的方法无返回值，可以更改下函数返回值类型，然后再调用。但这样感觉还是不是太方便，这时候就可以使用ref参数。**

```
 static void Main(string[] args)
        {
            //假设小明的月薪为5000
            double salary = 5000;
            Reward(ref salary);
            Console.WriteLine("小明实际月收入为："+salary);
            Console.ReadKey();
        }
        /// <summary>
        /// 奖金
        /// </summary>
        /// <param name="s">薪水</param>
        static void Reward(ref double s) 
        {
            s += 500;
        }
        /// <summary>
        /// 罚款
        /// </summary>
        /// <param name="s">薪水</param>
        static void Fine(double s)
        {
            s -= 500;
        }
```
![这里写图片描述](http://img.blog.csdn.net/20170926100840417?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaG9uZ2ppYXBlbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**同样是调用无返回值的函数，加了ref参数后就可以实现想要的结果。**
****
**下面做一个练习，使用ref交换两个int类型的变量**

```
 static void Main(string[] args)
        {
            //使用方法来交换两个int类型的变量
            int n1 = 10;
            int n2 = 20;
            //int temp = n1;
            //n1 = n2;
            //n2 = temp;
            ExchangeVariables(ref n1, ref  n2);
            Console.WriteLine(n1);
            Console.WriteLine(n2);
            Console.ReadKey();
            //n1 = n1 - n2;//-10 20
            //n2 = n1 + n2;//-10 10
            //n1 = n2 - n1;//20 10

        }

        /// <summary>
        /// 交换两个数的值
        /// </summary>
        /// <param name="n1">变量1</param>
        /// <param name="n2">变量2</param>
        public static void ExchangeVariables(ref int n1, ref  int n2)
        {
            int temp = n1;
            n1 = n2;
            n2 = temp;
        }
```
****
**上面的代码假设ExchangeVariables()函数的参数不加ref的话，打印后的结果肯定还是n1=10，n2=20；加ref参数后，皆可以将n1、n2这两个变量带入一个方法中进行改变，改变完成后，再将改变后的值带出方法给调用者。**

![这里写图片描述](http://img.blog.csdn.net/20170926102714261?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaG9uZ2ppYXBlbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
****
#3)、params可变参数（数组参数）
将实参列表中跟可变参数数组类型一致的元素都当做数组的元素去处理。params可变参数必须是形参列表中的最后一个元素。<br>
举例：String.Format方法和String.Split方法
****
**下面写一个方法，求一个班级学生的总成绩。**
```
 static void Main(string[] args)
        {
            int[] s = { 99, 88, 77 };
            GetStudentsScores("张三", 10, s);
            GetStudentsScores("李四", 11, 100, 100, 100);
            Console.ReadKey();
        }
        /// <summary>
        /// 求班级学生的总成绩
        /// </summary>
        /// <param name="name">姓名</param>
        /// <param name="id">学号</param>
        /// <param name="score">成绩</param>
        public static void GrtStudentsScores(string name, int id, params int[] score)
        {
            int sum = 0;
            for (int i = 0; i < score.Length; i++)
            {
                sum += score[i];
            }
            Console.WriteLine("{0}的学号是{1}，这次考试的总成绩是{2}", name, id,sum);
        }
```

**执行结果：**

![这里写图片描述](http://img.blog.csdn.net/20170926110241861?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaG9uZ2ppYXBlbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
****
**可以看出在主函数里调用GrtStudentsScores函数的时，使用了params参数后，成绩的实参可以传int类型的数组，也可以直接传int类型的变量。但最好是直接传int类型的变量，因为在程序中尽可能的少定义变量，减少内存的负载。**
****
**下面使用params参数，求任意数组的和**
```
        static void Main(string[] args)
        {          
            //求任意长度数组的和 整数类型的
            int[] nums = { 1, 2, 3, 4, 5 };
            int sum = GetSum(8, 9);
            Console.WriteLine("sum="+sum);
            Console.ReadKey();
        }

        //public static int GetSum(params int[] intArray)
        //{
        //    int sum = 0;
        //    for (int i = 0; i < intArray.Length; i++)
        //    {
        //        sum += intArray[i];
        //    }
        //    return sum;
        //}
        
        //遍历的时候也可以用foreach
        static int GetSum(params int[] intArray)
        {
            int sum = 0;
            foreach (var item in intArray)
            {
                sum += item;
            }
            return sum;
        }
```

**运行结果：**

![这里写图片描述](http://img.blog.csdn.net/20170926111105198?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaG9uZ2ppYXBlbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


**数组参数：**

下面使用数组参数分割特定字符串,代码如下：

    string str = "Tim;Tom.Amy,Lisa";
    string[] result = str.Split(';',',','.');
    foreach (var item in result)
    {
        Console.WriteLine(item);
    }

![](http://peng-image.oss-cn-beijing.aliyuncs.com/18-4-23/63758470.jpg)


# 4)、具名参数

首先我们写一个不具名调用：

    static void Main(string[] args)
    {
        PrintInfo("hjp",18);
    }

    static void PrintInfo(string name,int age)
    {
        Console.WriteLine("Hello {0},you are {1}",name,age);
    }
    
具名调用就要在修改PrintInfo方法的参数列表，如下：

     PrintInfo(name:"hjp",age: 18);
     
![](http://peng-image.oss-cn-beijing.aliyuncs.com/18-4-23/71095105.jpg)

你会发现，写具名参数程序也能够正常运行，有同学可能会问，你具名调用和不具名调用在结果上完全一样，那你写这么啰嗦有什么用呢，其实呢具名调用的两个优点：<br>
1. 提高代码的可读性（可以很清楚的看出姓名是hjp，年龄18。如果是非具名调用，那我只能去猜获取跳转到方法的定义出去看参数的名字）
2. 当你为参数加上名字后，参数的位置就不受参数列表顺序的约束了
        


    PrintInfo(age: 18,name:"hjp"); //你会发现程序也能正常运行    
    
严格意义上说，具名参数不是参数的某个种类，而是参数的使用方法。

# 5)、可选参数

可选参数就是当你调用一个方法的时候，这个参数可写可不写。因为你在声明这个方法的时候，这个参数是带有默认值的，对于带有默认值的参数，如果你在调用它的时候不写这个参数，那么这个参数自动获得声明时的默认值。下面对刚才那个例子进行修改，
把PrintInfo()方法的参数去掉。

- 参数因为具有默认值而变得“可选”
- 不推荐时间可选参数


    static void Main(string[] args)
    {
     PrintInfo();
    }

    static void PrintInfo(string name="hjp",int age=18)
    {
        Console.WriteLine("Hello {0},you are {1}",name,age);
    }

![](http://peng-image.oss-cn-beijing.aliyuncs.com/18-4-23/71095105.jpg)

运行代码后你会发现，程序可以正常执行，而且参数使用的是声明时的默认值，参数可选就是因为在声明的时候有默认值，这就是我们的可选参数。

# 6)、拓展方法（this参数）

- 方法必需是共有、静态的，即被public static 所修饰
- 必需是形参列表中的第一个，由this修饰
- 必需由一个静态类（一般类名为SomeTypeExtension）来统一收纳对SomeType类型的拓展方法
- 举例：LINQ方法

下面一段代码求double类型x值的保留4位小数

    double x = 3.141592;
    double y = Math.Round(x,4);
    Console.WriteLine(y);

![](http://peng-image.oss-cn-beijing.aliyuncs.com/18-4-23/76026863.jpg)

可以看出结果是正确的，但是有一点我们没有实现，就是我们的double类型没有Round方法，这时候我们就可以使用double这个类型的拓展方法了。拓展方法应该怎么写呢？首先应该声明一个静态类，使用这个静态类来拓展我们double这个数据类型。


    static void Main(string[] args)
    {
        double x = 3.141592;
        double y = x.Round(4);//这里只接受一个参数，是因为 . 前面的x就是我们Round方法的第一个参数
        Console.WriteLine(y);
    }

    /// <summary>
    /// double类型的拓展方法
    /// </summary>
    static class DoubleExtension
    {
        /// <summary>
        /// 将双精度浮点值四舍五入到指定的小数部分
        /// </summary>
        /// <param name="input">要求的值</param>
        /// <param name="digits">精确的位数</param>
        /// <returns></returns>
        public static double Round(this double input, int digits)
        {
            double result = Math.Round(input, digits);
            return result;
        }
    }

通过这个demo可以知道，当我们无法对一个类型的源码进行修改的时候，可以使用拓展方法为这种目标类型来追加方法，有了拓展方法这个功能，我们C#编程就变得非常牛B，非常方便。在未来的编程学习当中你会学习很多基于拓展方法的功能，其中就包括功能非常强大的LINQ

![](http://peng-image.oss-cn-beijing.aliyuncs.com/18-4-23/21453362.jpg)

下面有个逻辑需要我们实现，写个方法，这个方法需要一个集合类型的参数，然后判断一下这个集合类型的参数是不是都大于10。

        static void Main(string[] args)
        {
            bool b = AllGreaterThanTen(new List<int> { 11,21,12});
            Console.WriteLine(b);
        }

        /// <summary>
        /// 判断所有的int集合参数都大于10
        /// </summary>
        /// <param name="intList">int类型的集合</param>
        /// <returns></returns>
        static bool AllGreaterThanTen(List<int> intList)
        {
            foreach (var item in intList)
            {
                if (item <= 10)
                {
                    return false;
                }
            }
            return true;
        }
        
![](http://peng-image.oss-cn-beijing.aliyuncs.com/18-4-23/49112584.jpg)

可以看出，程序是没有问题的。下面为大家展示下LINQ的强大功能，首先添加System.Linq名称空间。

    using System.Linq;

    List<int> myList = new List<int> { 11, 21, 12 };
    bool b = myList.All(i=>i>10);//All就是个拓展方法
    Console.WriteLine(b);

上述代码可以实现AllGreaterThanTen()方法同样的效果，把光标移动到List集合上，按下F12键跳转到List集合的定义，看其方法，发现并没用All这个拓展方法。

![](http://peng-image.oss-cn-beijing.aliyuncs.com/18-4-23/75463400.jpg)


那么这个叫All的方法是从哪来的呢？同样把光标移动到All这个方法上，按下F12键，你会发现All这个方法是存在 Enumerable 这个静态类当中的，并且第一个参数就是用list来修饰的参数，所以说这个All是一个拓展方法。但是在这它并没有遵照命名规范--什么什么类+Extension，总之要记住All这个方法并不属于List这个类，而是一个拓展方法。而且这个方法隶属于Enumerable这个静态类，除了All这个方法外LINQ方法还有很多。比如还有Any这个方法，用来判断集合中是不是有某个值符合某个条件。还有球平均值的拓展方法，总之我们的LINQ方法有非常多，以后也会写几篇LINQ的博客。

![](http://peng-image.oss-cn-beijing.aliyuncs.com/18-4-23/98057888.jpg)
---

###### 各种参数的使用场景总结

- 传值参数：参数的默认传递方式
- 输出参数：用于除返回值外还需要输出的场景
- 引用参数：用于需要修改实际参数值的场景
- 数组参数：用于简化方法的调用
- 具名参数：提高可读性
- 可选参数：参数具有默认值
- 拓展方法（this参数）：为目标数据类型“追加”方法