---
layout:     post
title:      Git的使用
subtitle:   Git的使用
date:       2018-04-04
author:     JP
header-img: img/post-bg-debug.png
catalog: true
tags:
    - Git
    
---

>  **使用Git进行版本控制** 

### 1.Git简介
Git，简单说是一套版本控制工具。为什么不说是代码管理工具，因为除了代码可以管理外，它还可以管理文件的版本，写文章时也可以用Git来管理，你可以很清楚这一章这一节“两个版本”之间都有什么变化等。<br>

凡是文本的东西，你拿它管理都是可以的，音频、视频除外（它们的变化都是二进制代码的变化，我们的肉眼也看不明白），所以，Git简单来说就是一套文本的管理工具。版本管理工具有很多，SVN/TFS/VSO/Git，一般都是Client/Server成对存在的，Client一般都有CLI和GUI被集成。<br>

软件这个行业，它是拒绝单打独斗的，即便是独立软件，也不是一 个人开发的，而是一个团队开发的。开发的时候，我们总是要在团队里协同代码、管理版本的。横向协同就是人与人之间的合作，纵向的协同就是版本的管理，这一横一纵全都是靠代码管理工具。

Github也是用的Git这套版本管理系统，是Git的服务端，如果只用Client的话，你可以在“本地”管理你自己的代码库，进行代码的增删查改，版本变更的跟踪，一旦本地工作完成后，你可以把你编辑好的代码上传到服务器去，这样分散在各地的程序员就可以在Server上进行版本的协同了，所以说一般情况下，版本管理工具都是客户端和服务器的。

### 2.下载安装Git
Git下载地址：[https://git-scm.com](https://git-scm.com/)，安装过程中按照默认选项一路 next 就可以了，打开CMD，输入：

    git --version

可以查看git是否安装及git的版本，如果出现下图，说明安装成功；也可以在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！。

![](http://peng-image.oss-cn-beijing.aliyuncs.com/18-4-1/45501961.jpg)

### 3.管理Git
#### 3.1版本库
版本库又名仓库，英文名repository,你可以简单的理解一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改，删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻还可以将文件”还原”。所以创建一个版本库也非常简单。<br>

在我们安装完Git后，我们就可以使用git进行对本机代码库的管理了。首先我们要学会在本机上进行代码管理，然后才能在网上的代码库进行沟通，跟其他队员进行版本协同。<br>

下面我们就看看如何管理本机的代码库，第一件要做的事就是在本机建立一个代码库，建立代码库的时候，我们每一个文件夹都可以是一个代码库。下面我在E盘下建立了一个Projects的文件夹，在开始菜单里找到“Git”->“Git Bash”,点进去（也可以在文件夹的空白区域，点击鼠标右键，选择Git Bash Here），此时会出现一个命令行的窗口，在命令行输入：

    $ cd E:(找到E盘)
    $ mkdir Projects（新建名叫Projects的文件夹）
    $ cd Projects（进入Projects文件夹）
    $ pwd （显示当前目录）
    /e/Projects
然后在Projects文件夹下建一个HelloGit的文件夹，用来存放我们的项目。

    $ mkdir HelloGit（新建名叫HelloGit的文件夹）
    $ cd HelloGit（进入Projects文件夹）
    $ pwd（显示当前目录）
    /e/Projects/HelloGit

#### 3.2初始化版本库
我们要做的第一件事就是把我们的 HelloGit 文件夹初始化为一个Git代码库，在命令行窗口输入：

    git init

命令，此时会在 HelloGit 文件夹下生成一个.git的隐藏文件夹。

![](http://peng-image.oss-cn-beijing.aliyuncs.com/18-4-4/41524580.jpg)

如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用 ls -ah 命令就可以看见,或者点击任务栏中的查看，把文件扩展名和隐藏的项目显示出来就好了。<br>

![](https://peng-image.oss-cn-beijing.aliyuncs.com/18-4-1/%28QHT%29%29H4B9P1%298%60WFJX2%28WF.png)

.git 这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

为了方便以后提交代码，我们需要在命令行输入: <br>

    git config --global user.name "Your Name"
    git config --global user.email "email@example.com"

来设置自己的用户名和邮箱， 邮箱最好和github用的邮箱一致，因为Git是分布式版本控制系统，所以每个机器都必须自报家门：你的名字和Email地址。

注意 git config 命令的 --global 参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。


---