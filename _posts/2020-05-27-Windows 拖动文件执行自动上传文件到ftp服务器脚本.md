---
layout: post
title: Windows 拖动文件执行自动上传文件到ftp服务器脚本
subtitle:   
date:   2020-05-27
author: hjp
header-img: img/post-bg-ioses.jpg
catalog: true
tags:
- 
---

> bat脚本实现自动上传文件到ftp服务器

    @Echo Off
    Echo open ip_address [port] >ftp.up
    Echo [username]>>ftp.up
    Echo [password]>>ftp.up
    Echo Cd .\ >>ftp.up
    Echo binary>>ftp.up
    Echo put "%1">>ftp.up
    Echo bye>>ftp.up
    FTP -s:ftp.up
    del ftp.up /q

注意：[port]不填的话就是默认端口号，注意上面的username和password后的>>之间不要有空格，否则用户名和密码就不正确了。

大体上就是将ftp用到的交互式命令写到一个临时文件中，执行完后并删除。如果要调试的话，可以在代码的最后一行加上pause，这样执行完会暂停，能看到执行结果。

代码比较简单，粘贴到一个XXX.bat文件中，把需要上传的文件拖入到XXX.bat文件上，就ok了。如果你想上传的文件是固定路径和名称的，并且你不想拖入，你可以更改下第七行的脚本，将 %1 改成你的文件路径就可以了。


参考文档<br>

1. [Windows下通过bat脚本实现自动上传文件到ftp服务器](https://www.cnblogs.com/jasondan/p/3642258.html)<br>

2. [windows 拖动文件执行 bat脚本](https://blog.csdn.net/u013394527/article/details/54344002)<br>