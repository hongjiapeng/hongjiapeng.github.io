---
layout: post
title: MSIX是什么？
subtitle:   
date:   2020-07-10
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
- MSIX

---

## MSIX是什么

[MSIX](https://docs.microsoft.com/en-us/windows/msix/)是一种全新的安装包格式，其实是MSI(Microsoft Installer)的升级版，类似于Android的APK，IOS的IPA。它结合了MSI、appx、App-V、ClickOnce的功能，提供更加现代化且可靠的打包体验。

## MSIX的前世今生

从Windows 8 引入 Windows Store开始，在Windows系统中软件就分为两大类,一类是传统的.exe文件，称之为 “程序”（Program Files），一般装在 C:\Program Files下；另一类则称之为 “应用”（Application），大多安装在 C:\Program Files\WindowsApps 下

![](https://raw.githubusercontent.com/hongjiapeng/PicGoRes/main/img/notes/202207150004178.png)

Windows Installer 是在[Windows 2000](https://zh.wikipedia.org/wiki/Windows_2000)時提出，作为Windows操作系统中的安装程序开发标准的操作系统服务。它可以支持安装程序所需要的许多功能，并且可以支持交易式安装（Committable Installation），当安装程序发现错误或问题时，可以将安装程序中所做的任何修改（包含复制文件、修改配置等）全部回溯为未变更的状态。


![](https://raw.githubusercontent.com/hongjiapeng/PicGoRes/main/img/notes/202207142310345.webp)




Reference:

- [MSIX documentation](https://docs.microsoft.com/en-us/windows/msix/)

- [APK](https://zh.m.wikipedia.org/zh-hans/APK)
- [IPA](https://zh.m.wikipedia.org/zh-hans/IPA%E6%96%87%E4%BB%B6)
- [Windows Installer](https://zh.wikipedia.org/zh-cn/Windows_Installer)
- [The Future of Software on Windows: What is an MSIX File?](https://www.howtogeek.com/402021/the-future-of-software-on-windows-what-is-an-msix-file/)

- [MSIX是什么？详解Windows10系统下的MSIX](https://www.xitongtiandi.net/wenzhang/win10/32487.html)

- [MSIX Introduction: A comprehensive 24-chapter guide](https://www.advancedinstaller.com/msix-introduction.html#:~:text=MSIX%20is%20a%20new%20universal%20package%20format%20designed,AppX%20package%20%28initially%20used%20only%20for%20UWP%20apps%29.)