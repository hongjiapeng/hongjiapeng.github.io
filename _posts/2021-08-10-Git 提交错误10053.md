---
layout: post
title: Git 提交错误10053
subtitle:   
date:   2021-08-10
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
- Git
---

下午使用Git提交代码时，出现下面的错误
```
Unable to access ‘https://github.com/**/**/‘: OpenSSL SSL_read: Connection was aborted, errno 10053
```
上网查了下，说是Git默认限制推送的大小的原因，可尝试下运行命令更改限制大小，执行了下面的命令，解决了

```
git config --global http.postBuffer 524288000
```

另外一种解决方式是解除https的ssl的凭证
```
git config --global http.sslVerify false
```



