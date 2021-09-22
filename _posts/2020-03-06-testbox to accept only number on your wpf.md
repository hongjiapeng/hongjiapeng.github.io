---
layout: post
title: WPF's TextBox control only input number
subtitle:   
date:   2020-03-06
author: JP
header-img: img/post-bg-debug.jpg
catalog: true
tags:
- WPF
---

> How you can allow TextBox to accept only number on your WPF

###### You just need to write the code in "PreviewTextInput" event of TextBox just follow the steps what i am going to do. Code is below. 

###### xaml code:


```

 <TextBox Width="100"
          VerticalAlignment="Center"
          HorizontalAlignment="Center"
          PreviewTextInput="TextBox_PreviewTextInput"/>

```

###### cs code：

```
 private void TextBox_PreviewTextInput(object sender, System.Windows.Input.TextCompositionEventArgs e)
 {
    e.Handled = new System.Text.RegularExpressions.Regex("[^0-9]+").IsMatch(e.Text);
 }
```




