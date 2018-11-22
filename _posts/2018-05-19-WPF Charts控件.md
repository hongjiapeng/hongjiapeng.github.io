---
layout: post
title: 比较不错的开源WPF Chart控件
subtitle:   Chart控件
date:   2018-05-18
author: JP
header-img: img/post-bg-swift.jpg
catalog: true
tags:
- Chart
- WPF
- 自定义控件

---

>  **OpenSource WPF Chart Control**

### 1. ModernUIChart
[Code:http://modernuicharts.codeplex.com/](http://modernuicharts.codeplex.com/) <br>

##### Available Charts




- ColumnChart (ClusteredColumnChart, StackedColumnChart, StackedColumnChart100Percent)


- PieChart (PieChart and Dognut)


- BarChart (ClusteredBarChart, StackedBarChart, StackedBarChart100Percent)


- Doughnut Chart


- Radial Gauge Chart

##### Screenshots

- Default Layout



- Dark Layout


- Custom Palette (e.g.. with gradients or mono chrome)



- Easy Switch of Axes (same data, switched axes)


##### Intention 1
I needed Modern UI Charts for my own application which should run on Desktop (WPF), Web (Silverlight) and Windows 8 devices and I didn't want to use 3 different third party charting components. That’s why I created the charts from scratch and used them in the tool “SharePoint Code Analysis Framework (SPCAF)” [(http://go.spcaf.com/VSGallery)]((http://go.spcaf.com/VSGallery)) which I have developed with [Matthias Einig](https://archive.codeplex.com/).

##### Intention 2
I think that XAML is the greatest way to "describe” the UI of an application via a markup language. I don’t want to miss things like data binding, styling of controls, data templates, animation of state changes, easy re-use of custom controls, design support with Blend and many more. I don’t hope that HTML5 and JavaScript are the only future for our UIs. That’s why I wanted to discover how the same XAML could be used “cross-plattform” in WPF, Silverlight and Windows 8. For the charts I wanted to use as much as possible of the same XAML code which is available on all these plattforms. So finally the code for the charts uses the lowest common XAML subset of all three worlds. Check out the sample application in this project and see how it works.

##### Features
The charts have been developed from scratch with keeping in mind to fully support MVVM data binding, styling, retemplating, animation, dynamic series etc.

Dynamic binding of data
Animation after loading and after changes to underlying data
Custom Color Palette
Hidable Title and Legend
Switchable series
Light and dark layout support
Configurable font size
…
##### Try it out
The download contains the binaries, source code and test applications for WPF, Windows 8 and Silverlight. Download the release and try it out.

#####  	Important
This code is intended to be a sample how code can be created for WPF, Silverlight and Windows8. The code is an BETA release and still may have some bugs!

##### Screenshot Windows 8

 
##### Screenshot WPF


##### Screenshot Silverlight




### 2. Live-Charts

          


源码链接：[https://github.com/beto-rodriguez/Live-Charts](https://github.com/beto-rodriguez/Live-Charts)



##### Reference：[https://blog.csdn.net/derek518/article/details/49936255](https://blog.csdn.net/derek518/article/details/49936255)