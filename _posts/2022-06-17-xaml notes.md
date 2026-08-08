---
layout: post
title: Record some forgettable attribute of TextBlock in xaml
description:
date: 2022-06-17
author: JP
image: /img/post-bg-coffee.jpeg
toc: true
tags:
- XAML

---

XAML – Add Newline or line break in the Text attribute of TextBlock

```
<TextBlock Text ="DeveloperPublish.com &#x0a; @isenthil"/>
```

XAML - Use LineBrake

```
<TextBlock>
DeveloperPublish.com
<LineBreak/>
@isenthil
</TextBlock>
```

XAML - How to display a text in XAML which contains double and single quotation marks

You should encode the special characters:

```
<TextBlock Text='You shouldn&apos;t choose &quot;Copy if New&quot;:'/>
```

```
- &apos; for '
- &quot; for "
```


Reference:<br>

- [how-to-display-a-text-in-xaml-which-contains-double-and-single-quotation-marks](https://stackoverflow.com/questions/1103337/how-to-display-a-text-in-xaml-which-contains-double-and-single-quotation-marks/1103345#1103345)