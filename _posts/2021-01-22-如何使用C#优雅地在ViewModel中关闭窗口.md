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

当你在使用MVVM设计模式开发WPF应用程序时，第一条 **原则** 就是 **“ViewModel中没有UI元素”** 。好吧，如果不允许我在ViewModel中使用UI元素，那么到底如何从ViewModel中关闭窗口呢？这时候就需要借助于接口了。

这篇Blog将教您如何利用接口的功能从ViewModel中抽象出Window对象，并且仍然能够从ViewModel中关闭Windows。

不仅如此，我们还可以利用抽象的优点，在ViewModel中使用业务逻辑，甚至在单击Window的关闭按钮（Window顶部右角的X）时也可以防止Window关闭。

我们也可以通过抽象将接口信息合并到附加属性（AttachedProperty）中来减少重复重复的代码，该属性可以在应用程序中的任何Window上使用。真正做到 **「Write once Run anywhere」**。

废话不多说了，直接上代码。

>MainWindow.xaml


``` cs

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
    local:WindowCloser.EnableWindowClosing="True"
    mc:Ignorable="d">

    <Window.DataContext>
        <local:MainWindowViewModel />
    </Window.DataContext>

    <Grid>
        <Button
            Width="125"
            Height="45"
            Command="{Binding CloseCommand}"
            Content="Close Me" />
    </Grid>
</Window>


```
