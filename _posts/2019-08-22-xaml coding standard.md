---
layout: post
title: XAML编码规范
subtitle:   
date:   2019-07-24
author: JP
header-img: img/post-bg-swift.jpg
catalog: true
tags:
- 编码规范
---

>  **XAML编码规范**

## 一、代码可读性

#### a) 每行放一个属性

```
<TextBlock FontWeight="Bold"
           Foreground="White" 
           HorizontalAlignment="Right" 
           Text="{Binding ArticlesCountText"
           TextWrapping="Wrap" />
```

#### b) 在元素内，按字母顺序排序属性

```
<Image Source="/Assets/Shares/NeutralImage.png"
       Height="125"
       HorizontalAlignment="Center"
       Width="125"
       Stretch="UniformToFill"    
       VerticalAlignment="Center" />
```

#### c)	将x：Name或x：Key作为第一个属性

```
<Button x:Name = "ButtonValidation"
        Content = "Hello world"
        Width="100"
        Height="30"/>
```

这些属性比任何其他属性更重要，因为：
- 它们确定控件的唯一性
- 它们确定控件是否被使用（故事板，代码隐藏，绑定，......）

#### d)	如果元素没有内容，请使用自闭元素

```
<Button x:Name = "ButtonValidation"
        Content = "Hello world"
        Width="100"
        Height="30"/>
```

当元素未声明子标记并使用显式结束标记时，会违反此规则。这导致太多的空间浪费。如下：

```
<Button x:Name = "ButtonValidation"
        Content = "Hello world"
        Width="100"
        Height="30"></Button>
```

#### e)	把关联的属性放在最前面并按字母顺序排列

```
<Button Grid.Column="1" 
        Grid.Row="2" 
        Command="{Binding ShowWriterCommand}" 
        CommandParameter="{Binding WriterAshley}"
        Style="{StaticResource HubTileButtonStyle}" />
```

Grid.Column / Grid.Row属性就是典型的例子

## 二、命名

#### a)	使用 Pascal Casing

```
<Button x:Name = "ButtonCheckout" 
        Content = "Checkout" />
```

## 三、可维护性

当属性值在不同位置使用超过2次时，为了获得更好的可维护性，必须将此属性值声明为资源。

- 将值移动到资源
- 用StaticResource绑定替换所有值使用。 

## 四、推荐工具
强烈推荐在VisualStudio中安装[XAML Styler](https://github.com/Xavalon/XamlStyler)插件，XAML Styler是Visual Studio扩展，它基于一组样式规则来格式化XAML源代码。此工具可以帮助我们更好的维护XAML编码样式以及提高XAML的可读性。<br/>
你可以在VisualStudio的拓展功能中，搜索XAML Styler并添加此插件。
除了该插件提供的默认格式化样式外，您也可以通过修改XAML默认样式[External Configurations](https://github.com/Xavalon/XamlStyler/wiki/External-Configurations)，来实现自定义代码风格。

Reference: 
- [XAML Coding guidelines](https://github.com/cmaneu/xaml-coding-guidelines)<br>
- [XamlStyler](https://github.com/Xavalon/XamlStyler)

