---
layout: post
title: 解决 VisualStudio 出现Error MC3000 给定编码中的字符无效问题
subtitle:
date:   2021-11-13
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
- Visual Studio
- Error MC3000
---

## Error MC3000

> 解决 VisualStudio 出现Error MC3000 给定编码中的字符无效问题

在使用Visual Studio写XAML时，有时候会出现 Error MC3000 错误，上网查了下，说是编码问题，需要在xaml文件中的最上方添加以下代码段，完美解决

```xml
<?xml version="1.0" encoding="utf-8"?>
```
