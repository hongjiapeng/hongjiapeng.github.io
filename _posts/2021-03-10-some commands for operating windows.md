---
layout: post
title: 一些操作Windows系统的命令
subtitle:
date: 2022-03-10
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
  - Tools
---

> 随便整理的一些自用的 Windwos 上的指令

#### 查看硬盘参数

```
get-physicaldisk
```

#### DirectX 诊断工具

系统标签页会显示 DirectX 的版本，该电脑的名称，操作系统版本，BIOS 的信息等

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

#### 查看连接过 WIFI 的名称

```
netsh wlan show profiles
```

#### 查看连接过 WIFI 的密码

通过执行 `netsh wlan show profiles` 命令，可以查看已连接过的 WIFI 名称。<br/> 如下图，可以看到，我这台电脑连接过 3 个 WIFI 设备(包括手机热点)

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

输入 `netsh wlan show profiles WiFi名称 key=clear` <br/>

（如：netsh wlan show profiles Linux_5G key=clear）可查看该 WiFi 名称的详细信息，包括该 WiFi 的密码（关键内容后的就是 WiFi 密码）。如下图所示：

![](https://raw.githubusercontent.com/hongjiapeng/PicGoRes/main/img/notes/202207151809263.png)

#### Windows 家庭版中的 Windows 功能没有启用 Hyper-V 选项

```
pushd "%~dp0"
dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt
for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"
del hyper-v.txt
Dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL
```

把上面的脚本，copy 到文本文件，重命名为 xxx.cmd 即可

Reference：

- [通过 Windows 命令行（CMD）创建文件](http://www.markjour.com/article/cmd-create-file.html)
