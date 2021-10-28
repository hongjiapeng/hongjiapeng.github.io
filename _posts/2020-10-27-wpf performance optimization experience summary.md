---
layout: post
title: 如何在 Prism 8 WPF 应用程序中注册日志服务
subtitle:   
date:   2021-10-27
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
- WPF
- 性能优化
---

### Rendering Tier

根据硬件配置的不同，WPF采用不同的Rendering Tier做渲染。下列情况请特别注意，因为在这些情况下，即使是处于Rendering Tier 2的情况下也不会硬件加速。(不全，其余请查阅SDK)

### 布局和设计

- 尽量多使用Canvas等简单的布局元素，少使用Grid或者StackPanel等复杂的，越复杂性能开销越大。
- 建立逻辑树或者视觉树的时候，遵循Top-Down的原则。
  
### 图像

-  对Image做动画处理的时候（如调整大小等），可以使用这条语句RenderOptions.SetBitmapScalingMode(MyImage,BitmapScalingMode.LowQuality)，以改善性能。
- 用TileBrush的时候，可以CachingHint。

### 对象行为

- 访问CLR对象和CLR属性的效率会比访问DependencyObject/DependencyProperty高。注意这里指的是访问，不要和前面的绑定混淆了。但是，把属性注册为DependencyProperty会有很多的优点：比如继承、数据绑定和Style。

### 应用程序资源

- 在自定义控件，尽量不要在控件的ResourceDictionary定义资源，而应该放在Window或者Application级。因为放在控件中会使每个实例都保留一份资源的拷贝。
- 尽量使用Static Resources,但不能盲目使用。

### 文本

- 文字少的时候用TextBlock或者label,长的时候用FlowDocument.

- 使用元素TextFlow和TextBlock时，如果不需要TextFlow的某些特性，就应该考虑使用TextBlock，因为它的效率更高。

- 在TextFlow中使用UIElement（比如TextBlock）所需的代价要比使用TextElement（比如Run）的代价高.在FlowDocument中尽量避免使用TextBlock，要用Run替代。

- 在TextBlock中显式的使用Run命令比不使用Run命名的代码要高。
- 把Label（标签）元素的ContentProperty和一个字符串（String）绑定的效率要比把字符串和TextBlock的Text属性绑定的效率低。因为Label在更新字符串是会丢弃原来的字符串，全部重新显示内容。如果字符串不需要更新，用Label就无所谓性能问题。

- 在TextBlock块使用HyperLinks时，把多个HyperLinks组合在一起效率会更高。

- 显示超链接的时候，尽量只在IsMouseOver为True的时候显示下划线，一直显示下划线的代码高很多

- 尽量不使用不必要的字符串连接。

### 数据绑定

- 在使用数据绑定的过程中，如果绑定的数据源是一个CLR对象，属性也是一个CLR属性，那么在绑定的时候对象CLR对象所实现的机制不同，绑定的效率也不同。

  - 数据源是一个CLR对象，属性也是一个CLR属性。对象通过TypeDescriptor/PropertyChanged模式实现通知功能。此时绑定引擎用TypeDescriptor来反射源对象。效率最低。

  - 数据源是一个CLR对象，属性也是一个CLR属性。对象通过INotifyPropertyChanged实现通知功能。此时绑定引擎直接反射源对象。效率稍微提高。

  - 数据源是一个DependencyObject，而且属性是一个DependencyProperty。此时不需要反射，直接绑定。效率最高。

- 当一个CLR对象很大时，比如有1000个属性时，尽量把这个对象分解成很多很小的CLR对象。比如分成1000个只有一个属性的CLR对象。

- 当我们在列表（比如ListBox）显示了一个CLR对象列表（比如List）时，如果想在修改List对象后，ListBox也动态的反映这种变化。此时，我们应该使用动态的ObservableCollection对象绑定。而不是直接的更新ItemSource。两者的区别在于直接更新ItemSource会使WPF抛弃ListBox已有的所有数据，然后全部重新从List加载。而使用ObservableCollection可以避免这种先全部删除再重载的过程，效率更高。

- 尽量绑定IList而不是IEnumerable到ItemsControl。

### 其它性能建议

- 如果需要修改元素的Opacity属性，最后修改一个Brush的属性，然后用这个Brush来填充元素。因为直接修改元素的Opacity会迫使系统创建一个临时的Surface

- 用NavigationWindow的时候，尽量Update the client area by object,而不是URI

- 尽量不要使用ScrollBarVisibility=Auto

转载自：https://www.cnblogs.com/chiniao/archive/2010/08/09/1795499.html#!comments
