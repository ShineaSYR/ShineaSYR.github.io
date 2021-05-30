---
layout: post
title: 智能打点
categories: [CSS]
description: some word here
keywords: max-width, css, ellipsis
---

案例来自于GitHub题目css-quiz-9，[点击此处可查看](https://github.com/zhangxinxu/quiz/issues/34)以及业务场景（内容和Tags紧贴）。

## float+overflow/max-width（IE8+）
调整dom顺序，使用`float:right;`以及`overflow：hideen；`即可实现。而紧凑模式则可以通过对外层容器添加`max-width:100%;`实现。
在线访问[codePen-页面稍慢请耐心等待kkk](https://codepen.io/shineasyr/pen/QWGXoVM?editors=1100)

**一左一右，内容适配，打点为右侧①**
主要是调整dom顺序，以及添加`float:right;`。
相关代码👇
``` html
<div class="ellipsis-between">
    <div class="tag-info"><span class="">tag1</span><span>tag2</span></div>
    <div class="main-con">曲奇的第二本书都市商业爽文流】苏叶穿越到平行世界互联网行业刚刚崛起的时代中，一个苦逼小运营获得每天只要</div>
</div>
```
```css
/* 关键结构 */
.tag-info {
  float: right;
}
.main-con {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* 非关键css */
.ellipsis-between {
  font-size: 16px;
  line-height: 24px;
}
.tag-info { font-size: 0;}
.tag-info span {
  font-size: 12px;
  line-height: 20px;
  display: inline-block;
  color: hotpink;
  border: 1px solid currentColor;
  margin: 1px;
  vertical-align: top;
}
```
**一左一右，内容适配，打点为开头**
与上述①基本一致，唯一区别是开头打点是通过`direction:rtl;`
```css
.ellipsis-between-rtl {
  direction: rtl;
}
```
**紧贴布局，tag为撑满状态**
主要是通过对外层容器添加`display:inline-block;max-width:100%;`使得外容器的宽度适配内部总宽度之和，又不会超出。
```html
<div class="ellipsis-close">
    <div class="tag-info"><span class="">tag1</span><span>tag2</span></div>
    <div class="main-con">曲奇的第二本书都市商业爽文流】苏叶穿越到平行世界互联网行业刚刚崛起的时代中，一个苦逼小运营获得每天只要</div>
</div>
```
```css
/* 关键结构 */
.tag-info {
  float: right;
}
.main-con {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.ellipsis-close {
  display: inline-block;
  max-width: 100%;
}

/* 非关键css */
.tag-info { font-size: 0;}
.tag-info span {
  font-size: 12px;
  line-height: 20px;
  display: inline-block;
  color: hotpink;
  border: 1px solid currentColor;
  margin: 1px;
  vertical-align: top;
}
```
## flex

## table



**参考数据**
css-quiz-9图片如下所示
<img src="{{ site.url }}/assets/images/samrt_ellipsis001.png" width="400px;">
紧贴效果示意图片如下所示
![紧贴效果示意]({{ site.url }}/assets/images/samrt_ellipsis002.png)
