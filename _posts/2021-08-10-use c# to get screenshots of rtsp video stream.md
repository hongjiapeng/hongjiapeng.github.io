---
layout: post
title: 使用C#和FFmpeg获取RTSP视频截图
subtitle:   
date:   2021-08-10
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
- C#
- RTSP
- FFmpeg
---


[FFmpeg](https://ffmpeg.org/)，是一个开源的，用于录制、转换和流式传输音频和视频的多媒体框架。

### 下载

- 官方的下载地址：[Download FFmpeg](https://ffmpeg.org/download.html)
- GitHub中的下载地址：[codexffmpeg release](https://github.com/GyanD/codexffmpeg/releases)

### 解压

下载完解压后，打开bin目录，会发现三个exe文件：ffmpeg.exe，ffplay.exe，ffprobe.exe。我们发现每个exe的体积都很大，这是因为相关的dll已经被编译到exe里面去了。在当前目录下找到 ffmpeg.exe 
![解压](/static/posts/2021-09-21_10-39-12.png) 

### 主要参数[](https://ffmpeg.org/ffmpeg.html#Main-options)

```c
-i  //设置输入文件名
-f  //设置输出格式
-ss //从指定时间开始转换，以秒为单位
```

使用到的命令，
```
-i 视频地址 -ss 第几帧 -f image2 图片存放地址
```




参考：

- https://ffmpeg.org/ffmpeg.html#Main-options
- http://underpop.online.fr/f/ffmpeg/help/image2-1.htm.gz
