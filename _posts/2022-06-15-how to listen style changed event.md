---
layout: post
title: 如何监听JS中某一元素样式改变事件
subtitle:   
date:   2022-06-15
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
- java script

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

- [Web API Doc](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver#MutationRecord)