---
layout: post
title: 如何使用C#优雅地在ViewModel中关闭Windows
subtitle:   
date:   2021-01-22
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
- C#
- WPF
---

>前言

当你在使用MVVM设计模式开发WPF应用程序时，第一条 **原则** 就是 **“ViewModel中没有UI元素”**，也就是UI与业务逻辑分离，以便于后期即使要修改UI，也无需更改ViewModel。好吧，如果不允许我们在ViewModel中使用UI元素，那么到底如何从ViewModel中关闭窗口呢？这时候就需要充分利用面向接口编程的优势了。

>正文

这篇Blog将教您如何利用接口的优势，怎样从ViewModel中抽象出Window对象，并且仍然能够从ViewModel中关闭Windows。

不仅如此，我们还可以利用抽象的优点，在ViewModel中使用业务逻辑，甚至在单击Window的关闭按钮（Window顶部右角的X）时也可以防止Window关闭。

我们也可以通过抽象将接口信息合并到附加属性（AttachedProperty）中来减少重复重复的代码，该属性可以在应用程序中的任何Window上使用。真正做到 **「Write once Run anywhere」**。

废话不多说了，代码直接撸起来。

首先在窗口中定义一个关闭窗口的按钮

>MainWindow.xaml


```cs

<Window
    x:Class="CloseWindowMvvm.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:local="clr-namespace:CloseWindowMvvm"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    Title="MainWindow"
    Width="800"
    Height="450"
    mc:Ignorable="d">

    <Window.DataContext>
        <local:MainWindowViewModel />
    </Window.DataContext>

    <Grid>
        <Button
            Width="125"
            Height="45"
            Command="{Binding CloseCommand}"
            Content="关闭" />
    </Grid>
</Window>


```

接着需要定义一个ICloseWindows接口，用来解耦View和ViewModel。这个接口中定义了一个关闭窗口的委托，和一个是否允许关闭窗口的方法。

>ICloseWindows.cs


```cs
  interface ICloseWindows
  {
  
     Action Close { get; set; }

     bool CanClose();
  }
```

MainWindow的ViewModel定义如下：

>MainWindowViewModel.cs

```cs
 public class MainWindowViewModel : ICloseWindows
 {
     public Action Close { get; set; }

     private ICommand _closeCommand;
     public ICommand CloseCommand => _closeCommand ?? (_closeCommand = new DelegateCommand(CloseWindow));

     private void CloseWindow()
     {
         Close?.Invoke();
     }

     public bool CanClose()
     {
         MessageBoxResult messageBoxResult = 
             MessageBox.Show("确定要关闭吗？", "提示", MessageBoxButton.OKCancel);

         return messageBoxResult == MessageBoxResult.OK;
     }
 }
```


接着我们在MainWindow的后台，窗体的Loaded事件中，先判断当前窗口的DataContext是不是ICloseWindows，由于我们在Window的Xaml代码中指定了当前Window的ViewModel，并且ViewModel继承自ICloseWindows。所以当窗体加载的时候，会注册ViewModel的Close委托，以及当前窗口的Closing事件。

当点击窗口中的 “关闭”按钮时，首先会执行ViewModel中定义的Close委托的匿名方法，关闭当前窗口，接着会触发当前窗口的Closing事件，会执行e.Cancel=!vm.CanClose();这一行代码，所以，我们就可以在ViewModel中通过设置CanClose()方法控制当前窗口是否允许关闭了。

```cs
using System.Windows;

namespace CloseWindowMvvm
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();

            Loaded += MainWindow_Loaded;
        }

        private void MainWindow_Loaded(object sender, RoutedEventArgs e)
        {
            if (DataContext is ICloseWindows vm)
            {
                vm.Close += () =>
                {
                    Close();
                };

                Closing += (s, e) =>
                {
                    e.Cancel = !vm.CanClose();
                };
            }
        }
    }
}

```

到这里，你会发现，没有在ViewModel中直接引用UI元素，也没有直接依赖ViewModel，真正做到了解耦，是不是感觉得很amazing。


##### 利用附加属性进一步优化

新建一个叫WindowCloser的类，并在此类中添加一个附加属性

>WindowCloser.cs

```cs
 public class WindowCloser
 {
     public static bool GetEnableWindowClosing(DependencyObject obj)
     {
         return (bool)obj.GetValue(EnableWindowClosingProperty);
     }

     public static void SetEnableWindowClosing(DependencyObject obj, bool value)
     {
         obj.SetValue(EnableWindowClosingProperty, value);
     }

     // Using a DependencyProperty as the backing store for EnableWindowClosing.  This enables animation, styling, binding, etc...
     public static readonly DependencyProperty EnableWindowClosingProperty =
         DependencyProperty.RegisterAttached("EnableWindowClosing", typeof(bool), typeof(WindowCloser), new PropertyMetadata(false, OnEnableWindowClosingChanged));

     private static void OnEnableWindowClosingChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
     {
         if (d is Window window)
         {
             window.Loaded += (s, e) =>
             {
                 if (window.DataContext is ICloseWindows vm)
                 {
                     vm.Close += () =>
                     {
                         window.Close();
                     };

                     window.Closing += (s, e) =>
                     {
                         e.Cancel = !vm.CanClose();
                     };
                 }
             };
         }
      }
 }
```

在MainWindow.xaml的Window节点下中新增一行代码

```cs
<Window

    /*
     * Other code ...
     */
     
    local:WindowCloser.EnableWindowClosing="True"/>

    /*
     * Other code ...
     */
    
    
</Window>

```


MainWindow的后台代码中，只留下下图所示代码，其余的全部删除掉

>MainWindow.cs


```cs
using System.Windows;

namespace CloseWindowMvvm
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
	{
		public MainWindow()
		{
			InitializeComponent();
		}
    }
}


```


再次运行，你会发现利用附加属性，实现了相同的效果，并且利用WindowCloser附件属性，也可以在别的窗口中复用，是不是很酷呢？



###### 感谢

[Prism](https://github.com/PrismLibrary/Prism)

