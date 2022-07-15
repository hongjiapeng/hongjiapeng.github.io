---
layout: post
title: 如何使用 JavaScript 获取一个月中的天数
subtitle:   
date:   2022-06-15
author: JP
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
- Java Script

---


```
function getDaysInMonth(year, month) {
  return new Date(year, month, 0).getDate();
}

const date = new Date();
const currentYear = date.getFullYear();
const currentMonth = date.getMonth() + 1; // 👈️ months are 0-based

// 👇️ Current Month
const daysInCurrentMonth = getDaysInMonth(currentYear, currentMonth);
console.log(daysInCurrentMonth);

// 👇️ Other Months
const daysInJanuary = getDaysInMonth(2025, 1);
console.log(daysInJanuary); // 👉️ 31

const daysInSeptember = getDaysInMonth(2025, 9);
console.log(daysInSeptember); // 👉️ 30

```

Reference:

- [Get the Number of Days in a Month using JavaScript](https://bobbyhadz.com/blog/javascript-get-number-of-days-in-month)
- [Get the First Day of Next Month](https://bobbyhadz.com/blog/javascript-get-first-day-of-next-month)