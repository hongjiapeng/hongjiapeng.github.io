---
layout: post
title: 如何使用JS监听某一元素样式的变化
subtitle:   
date:   2022-06-15
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
- Java Script

---

使用下面的js脚本，即可监听element样式的变化
```
var observer = new MutationObserver(function(mutations) {
    mutations.forEach(function(mutationRecord) {
        console.log('style changed!');
    });    
});

var target = document.getElementById('myId');
observer.observe(target, { attributes : true, attributeFilter : ['style'] });
```

Reference:<br>

- [Web API Doc ](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver#MutationRecord)