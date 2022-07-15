---
layout: post
title: 将代码同时上传到github和gitlab
subtitle:   
date:   2020-05-27
author: JP
header-img: img/post-bg-kuaidi.jpg
catalog: true
tags:
- Git
---

Git是分布式版本控制系统，自然可以同步到多个远程库，所以一个本地库可以既关联GitHub又关联GitLab，当然还可以关联码云了。

使用多个远程库时，要注意git给远程库起的默认名称时origin，如果有多个远程库，我们需要用不同的名称来标识不同的远程库。假设我们有一个gittest项目，先删除已关联的名为origin的远程库：

`git remote rm origin`

然后先关联GitHub的远程库：

`git remote add github git@github.com:xxx/gittest.git`

接着关联GitLab的远程库：

`git remote add gitlab git@gitlab.com:xxx/gittest.git`

现在，我们用git remote -v查看远程库信息，可以看到两个远程库：

```
github git@github.com:xxx/gittest.git (fetch)
github git@github.com:xxx/gittest.git (push)
gitlab git@gitlab.com:xxx/gittest.git (fetch)
gitlab git@gitlab.com:xxx/gittest.git (fetch)
```
如果推送到github，使用命令：

`git push github master`

如果推送到gitlab，使用命令：

`git push gitlab master`

如果推送到码云，使用命令：

`git push gitee master`

这样一来，本地库就可以同时与多个远程库互相同步了。


Reference:<br>

[Git使用方法（精心整理，基本够用）](https://blog.csdn.net/xukai0110/article/details/80637902)



