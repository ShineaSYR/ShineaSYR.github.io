---
layout: post
title: 浏览器版本
categories: [Javascript]
description: 通过一些方法来判断浏览器的情况，IE、Safari、Mobile Phone、
keywords: keyword1, keyword2
---

## JS
### IE
``` javascript
// 判断是否为IE浏览器
function isIE() {
  if (!!window.ActiveXObject || "ActiveXObject" in window) {
    return true;
  } else {
    return false;
  }
}

// 判断IE版本
function IEversion() { 
  if (document.all && !window.XMLHttpRequest) return 6; 
  if (document.all && !document.querySelector) return 7;
  if (document.all && !document.addEventListener) return 8;
  if (document.all && !window.atob) return 9;
  if (document.all && document.addEventListener && window.atob) return 10;
  return 11;
}

// 其他
if (!('open' in document.createElement('details'))) {
  //该浏览器为IE浏览器
}
if (!document.addEventListener) {
  // 该浏览器为IE8及IE8以下版本
}
if (!history.pushState) {
  // 该浏览器为IE9及IE9以下版本
}
```
<!-- ### Safari
### Mobile Phone -->

## CSS
```css
/* 区分IE和其他浏览器——::-webkit-scrollbar */
xxx {
    /*IE*/
    color: #aaa;
}
.cl, xxx::-webkit-scrollbar {
    /*chrome、firefox*/
    color: #bbb;
}
/* 区分IE8-和IE9+ ——:root/:enabled */
/* :root选择器会提高优先级 */
xxx {
  color: #aaa;  /* IE8- */
}
.cl:enabled, xxx {
  color: #bbb;  /* IE9+ */
}

/* 区分IE9-和IE10+ ——:invalid */
xxx {
  color: #aaa;  /* IE9- */
}
.cl:invalid, xxx {
  color: #bbb;  /* IE10+ */
}
/* IE9背景图特殊 */
xxx {
    /*一倍图兜底*/
    background-image: url("");
    /*IE9+ 二倍图 or svg*/
    background-image: url(""), none;
}


```