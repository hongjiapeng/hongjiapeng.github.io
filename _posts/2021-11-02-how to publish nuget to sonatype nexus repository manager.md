---
layout: post
title: 如何将 Nuget 发布到 Sonatype Nexus Repository Manager 
subtitle: 如何搭建私有的NuGet库
date:   2011-11-02
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
- Nuget
- Maven
---

## Nuget是什么

如果你了解python，那么它类似pip

如果你了解python，那么它类似pip

如果你了解ruby，那么它类似gem

[Nuget](https://docs.microsoft.com/zh-cn/nuget/what-is-nuget) 其实是[.Net平台](https://dotnet.microsoft.com/)的一个包管理工具，适用于任何现代开发平台的基本工具可充当一种机制，通过这种机制，开发人员可以创建、共享和使用有用的代码。通常，此类代码捆绑到“包”中，其中包含编译的代码（如 DLL）以及在使用这些包的项目中所需的其他内容。

Nuget包的本质是一个以.nupkg为后缀名的zip压缩文件，其中包含了编译后的Dll，与该代码相关的其他文件以及描述性清单（包含包版本号等信息）等。下图显示nuget包从创建，上传到被使用的流程。

![img](https://docs.microsoft.com/zh-cn/nuget/media/nuget-roles.png)

## Nuget是什么






参考：

- [NuGet 简介](https://docs.microsoft.com/zh-cn/nuget/what-is-nuget)
- [NuGet是什么？理解与使用（上）](https://zhuanlan.zhihu.com/p/36207092)