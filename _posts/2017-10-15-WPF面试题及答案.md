---
layout:     post
title:      WPF基础
subtitle:   WPF 基础面试题及答案
date:       2017-10-15
author:     JP
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - 面试题
    
---

>随便总结的一些WPF基础


##### 一. 什么是WPF<br>
WPF英文全称是Windows Presentation Foundation（Windows呈现基础），是创建桌面客户端应用程序的UI框架。

##### 二. 什么是XAML <br>
XAML是WPF技术中专门用于设计UI的语言。

##### 三. [WPF由哪两部分组成?](https://www.jobui.com/mianshiti/it/net/3033/)
WPF 由两个主要部分组成：引擎和编程框架。 <br>

1.引擎。WPF引擎统一了开发人员和设计人员体验文档、媒体和 UI 的方式，为基于浏览器的体验、基于窗体的应用程序、图形、视频、音频和文档提供了一个单一的运行时库。WPF使得应用程序不仅能够充分利用现代计算机中现有的图形硬件的全部功能，而且能够利用硬件将来的进步。例如，WPF的基于矢量的呈现引擎使应用程序可以灵活地利用高 DPI 监视器，而无需开发人员或用户进行额外的工作。 同样，当 WPF检测到支持硬件加速的视频卡时，它将利用硬件加速功能。<br>

2.框架。WPF框架为媒体、用户界面设计和文档提供大量的解决方案。WPF的设计考虑了可扩展性，使开发人员可以完全在WPF引擎的基础上创建自己的控件，也可以通过对现有WPF控件进行再分类来创建自己的控件。WPF框架的核心是用于形状、文档、图像、视频、动画、三维以及用于放置控件和内容的面板的一系列控件。这些“自有控件”为开发下一代用户体验提供了构造块。<br>

##### ~~四. Silverlight 和 WPF的异同。~~

Silverlight是一个跨浏览器的、跨平台的插件，在浏览器中运行。因[Silverlight 终止支持](https://support.microsoft.com/zh-cn/help/4511036/silverlight-end-of-support)，故删除此回答。

##### 五. 如何理解WPF体系架构？
WPF使用多层架构，类似于三层结构，最顶层部分为托管代码API，此层用于为开发人员编写WPF应用程序提供较高层次的服务，基于C#托管代码编写。转换.NET代码到DirectX的工作由中间层milcore.dll实现。中间层milcore是用非托管代码实现，因为它需要与DirectX紧密集成，对性能敏感，就是消耗的资源比较多，对性能影响较大。

##### 六. 在WPF中Binding的作用及实现语法？

典型的Binding具有四个重要组成部分：Binding目标对象（binding target object） 目标对象属性（target property） Binding数据源（binding source） Path(用于指明要从数据源中取得的值，就是我们通常写的属性名称）。

##### 七· 解释什么是依赖属性，它和以前的属性有什么不同？为什么在WPF会使用它？
1.WPF提供了一组服务，这些服务可用于扩展公共语言运行时 (CLR) 属性的功能，这些服务通常统称为 WPF 属性系统。由 WPF 属性系统支持的属性称为依赖项属性。

2.它和以往属性的不同之处有

(1)依赖属性是一种特定类型的属性。这种属性的特殊之处在于，其属性值受到 Windows 运行时中专用属性系统的跟踪和影响。

(2)依赖属性的用途是提供一种系统的方式，用来基于其他输入（在应用运行时其内部出现的其他属性、事件和状态）计算属性的值。

(3)依赖属性代表或支持编程模型的某种特定功能，用于定义 Windows 运行时应用，这种模型使用 XAML 编写 UI，使用 C#、Microsoft Visual Basic 或 Visual C++ 组件扩展 (C++/CX) 编写代码。

一般的属性没有这么复杂。

3.WPF使用它是有不少优点的

（1）优化了属性的存储，直接减少了不必要的内存使用。

（2）有属性变化通知 限制 验证等。

（3）可以储存多个值，配合Expression及Animation等，打造出更灵活的使用方法。

##### 八. WPF中什么是样式？

首先明白WPF中样式属于资源中重要的一种。

同时样式也是属性值的集合，能被应用到一个合适的元素中，或者说能将一组属性应用到多个元素。

WPF中样式可以设置任何依赖属性。

WPF中样式也支持触发器，通过属性的改变，触发一组活动，包括改变某个控件的样式。

WPF中元素只能使用一个样式。

样式有继承的特性，样式可以继承样式。

##### 九· WPF中什么是模板 ？

WPF中模板是用于定义或重定义控件结构，或者说对象的外观。

WPF中模板有两类，一个是控件模板（ControlTemplate) 另一个是数据模板（DataTemplate），它们都派生自FrameworkTemplate抽象类。

总共有三大模板 ControlTemplate，ItemsPanelTemplate，DataTemplate。

1.ControlTemplate 主要用途是更改控件的外观。它有两个重要属性：VisualTree（视觉树）内容属性和Triggers触发器，对于触发器可以不用过多考虑，触发器可有可无。VisualTree就是呈现我们所画的控件。Triggers可以对我们的视觉树上的元素进行一些变化。

2.ItemsPanelTemplate 是个特殊的空间模板，主要用来标明多条目控件如何显示它所包含的多项数据。也可以说是指定用于项的额布局的面板。多用于多个内容控件的目标。多为Panel属性或者Panel结尾的属性。

3.DataTemplate 主要用于数据的呈现。也被称为显示绑定数据对象的模板。

##### 十· 绑定（Binding ）的基础用法

WPF 里分三种：Binding,PriorityBinding,MultiBinding,这三种Binding的基类都是BindingBase，而BindingBase又继承于MarkupExtension。

常见的使用Binding方法是：

1.针对于继承于FrameworkElement控件。 SetBinding(DependencyProperty dp,String path),SetBinding(DependencyProperty dp,BindingBase binding),其中FrameworkElement中SetBinding只对DependencyProperty有效。

2.另一种是 BindingOperations.SetBinding(currentFolder,TextBlock.TextProperty,binding);

BindingOperations.SetBinding的原型是

public static BindingExpressionBase SetBinding(DependencyObject target,DependencyProperty dp,BindingBase binding)

3.清除Binding:

BindingOperations.ClearBinding(currentFolder,TextBlock.TextProperty);//删除currentFolder上的TextBlock.TextProperty绑定

BindingOperations.ClearAllBindings(currentFolder);//删除currentFolder上的所有绑定。

直接对Dependency Property赋值也可以解除binding，不过只对单向binding有效。

##### 十一、 视觉树 VS 逻辑树?

1.逻辑树是视觉树的子集，也就是视觉树基本上是逻辑树的一种扩展。

2.WPF通过逻辑树来解决依赖项属性继承和资源的问题，使用视觉树来处理渲染，事件路由，资源定位等问题。

3.逻辑树可以认为是XAML所见的，而视觉树包含了XAML元素内部的结构。

4.逻辑树的查找可以通过LogicalTreeHelper辅助类，视觉树的查找可以通过VisualTreeHelper辅助类，其中需要注意的是对ContentElement元素的查找，无法直接通过VisualTreeHelper进行查找，ContentElement元素并不继承Visual，而ContentElement元素的使用时需要一个ContentElement载体FrameworkContentElement。

 

##### 十二、属性变更通知(INotifyPropertyChanged 和 ObservableCollection<T>)

1.INotifyPropertyChanged向客户端发出某一属性值更改的通知。

2.ObservableCollection<T>类，它是实现 INotifyCollectionChanged 接口的数据集合的内置实现。表示一个动态数据集合，在添加项、移除项或刷新整个列表时，此集合将提供通知


##### 十三、 ResourceDictionary

提供包含元素和 WPF 应用程序的其他元素使用的 WPF 资源的一个哈希表/字典实现。

有利于项目中资源共享。

 

##### 十四 · 路由事件的三种方式/策略（冒泡 直接 隧道）

WPF中的路由事件是沿着VisualTree传递的，作用是用来调用应用程序的元素树上的各种监听器上的处理程序。

（1）冒泡，这种事件处理方式是从源元素向上级流过去，直到到达根节点即顶层节点，一般为最外层的控件。

（2）直接，这种处理方式是在源上处理，主要用在源元素上处理。通常setter和trigger中有所体现，我个人认为VisualState可视状态可能也是直接事件处理，也是依赖属性的状态改变。和Trigger有一定的重复，但是VisualState是通过生硬的动画间接实现依赖属性的改变。

（3）隧道，又称作Preview事件，和冒泡事件处理方式相反的。元素树的根位置调用事件处理程序，依次向下直到源元素位置。

隧道事件和冒泡事件一般成对出现。同一事件，执行时首先是隧道事件，然后是冒泡事件。

##### 十五 · Routed Events(路由事件) 与 Commands(命令)

Event 和 Command 是程序内部通信基础，Routed Events 能够发起多重控件，并且能有序和用户沟通。

Commands是.NET Framework 提供的核心构架，来激活和去除高级别任务。

由此衍生的Animation是events的更进一步。让你能够以友好互动的方式使用Event架构，来使用多重控件。

##### 十六 · 解释这几个类的作用及关系: Visual, UIElement, FrameworkElement, Control 。

它们四个的关系：从System.Windows.Controls命名空间中看，依次的继承关系是：

Visual继承UIElement,UIElement继承FrameworkElement,FrameworkElement继承Control。

1 Visual主要作用是为WPF提供2D呈现支持，主要包括输出显示，坐标转换，区域剪切等。

2 UIElement的主要作用是构建WPF元素和基本呈现特征的基类。例如其中定义很多与输入和焦点有关的特性，例如键盘事件，鼠标，还有一些与WPF事件模型有关的API。

3 FrameworkElement的主要作用是为定义的WPF元素添加一些功能。例如，布局定义 逻辑树 对象生命周期事件  支持数据绑定和动态资源引用 支持样式和动画。

4 Control的主要作用是为自定义应用程序控件提供基础。因为它是创建自定义应用程序控件的基类,作用就是可以重写Control类所提供的属性，方法，事件等，为自定义控件添加自定义逻辑。构建WPF应用程序页面的Window类也派生自它。

