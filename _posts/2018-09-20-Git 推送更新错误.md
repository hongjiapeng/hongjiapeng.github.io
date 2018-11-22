---
layout:     post
title:      Git 推送更新出现错误： Updates were rejected because the remote contains work that you do
subtitle:   
date:       2018-09-20
author:     JP
header-img: img/post-bg-debug.png
catalog: true
tags:
    - Git    
    
---

每次在家更新完Blog，回到公司再更新，总会出现这样的错误，真令人头疼。后来仔细查阅了文档，原来是基础没打牢啊。git pull命令的作用是，取回远程主机某个分支的更新，再与本地的指定分支合并。废话不多说，看下面步骤：

git 提交的步骤： <br>

1. git init //初始化仓库
2. git add .(文件name) //添加文件到本地仓库
3. git commit -m “first commit” //添加文件描述信息
4. git remote add origin + 远程仓库地址 //链接远程仓库，创建主分支
5. git push -u origin master //把本地仓库的文件推送到远程仓库


提交之后就会出现以下错误 

![](http://peng-image.oss-cn-beijing.aliyuncs.com/18-9-20/78803209.jpg)

要想解决以上错误，只需要在5，6之间使用git pull origin master即可

正确步骤： 

1. git init //初始化仓库
2. git add .(文件name) //添加文件到本地仓库
3. git commit -m “first commit” //添加文件描述信息
4. git remote add origin + 远程仓库地址 //链接远程仓库，创建主分支
5. git pull origin master // 把本地仓库的变化连接到远程仓库主分支
6. git push -u origin master //把本地仓库的文件推送到远程仓库


OK!


