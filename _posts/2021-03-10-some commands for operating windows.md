---
layout: post
title: 一些操作Windows系统的命令
subtitle:   
date:   2020-03-10
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
- Tools
---

>随便整理的一些自用的Windwos上的指令


#### 查看硬盘参数

```
get-physicaldisk
```

#### DirectX诊断工具

系统标签页会显示DirectX的版本，该电脑的名称，操作系统版本，BIOS的信息等
```
dxdiag
```

#### 操作文件夹

```
列出当前文件夹内的所有文件
ls

删除指定目录
Remove-Item [folder path]
```

#### 彻底删除顽固文件的代码

```
DEL /F /A /Q \\?\%1
RD /S /Q \\?\%1
```