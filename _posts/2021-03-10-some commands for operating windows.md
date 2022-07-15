---
layout: post
title: 一些操作Windows系统的命令
subtitle:   
date:   2022-03-10
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

#### 操作文件

一、建立空文件的几种方式

- cd . > a.txt
- copy nul a.txt
- type nul > a.txt
- echo a 2> a.txt
- fsutil file createnew d:\a.txt 0


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

#### 查看连接过WIFI的名称

```
netsh wlan show profiles
```

#### 查看连接过WIFI的密码

通过执行 ``` netsh wlan show profiles ``` 命令，可以查看已连接过的WIFI名称。<br/> 如下图，可以看到，我这台电脑连接过3个WIFI设备(包括手机热点)

```
接口 WLAN 上的配置文件:


组策略配置文件(只读)
---------------------------------
    <无>

用户配置文件
-------------
    所有用户配置文件 : iPhone
    所有用户配置文件 : TP-LINK_C001
    所有用户配置文件 : Linux_5G
```


输入 ``` netsh wlan show profiles WiFi名称 key=clear ``` <br/>

（如：netsh wlan show profiles Linux_5G key=clear）可查看该WiFi名称的详细信息，包括该WiFi的密码（关键内容后的就是WiFi密码）。如下图所示：

![](https://raw.githubusercontent.com/hongjiapeng/PicGoRes/main/img/notes/202207151809263.png)

参考：

- [通过 Windows 命令行（CMD）创建文件](http://www.markjour.com/article/cmd-create-file.html)