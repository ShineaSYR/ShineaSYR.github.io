---
layout: post
title: 使用replaceState提升用户体验
categories: [Javascript]
description: 通过replaceState改善多次点击重复的问题
keywords: replaceState, 用户体验
---

很早之前做过一个[网站](https://www.ncmchina.com/collections.html)，里面有个作品列表，是通过URL里的参数变更展示不同的数据信息。

原本实现的href跳转到`?id=XX`，该方案在频繁切换作品后，希望通过点击返回上一层时（未切换作品之前），需要点击许多次。

优化后的效果可以观看下方的视频录制，或点击[网站](https://www.ncmchina.com/collections.html)自行体验。
<video controls width="100%" src="{{ site.url }}/assets/images/replaceState.mp4"></video>

<!-- <iframe  width=100% src="imgs/replaceState.mp4"> -->

## 场景问题分析
**关键词：history删除历史记录、无刷新**

- 问：href链接直接跳转，为什么会出现这样的问题呢？

href每次跳转都会在当前的历史记录里新增一条记录，有点类似于进栈，点击返回则会在历史记录删除一条记录，类似于出栈。

- 那如果每次在href跳转的时候同时删除历史理解不就可以解决这个问题了吗？

不可以删除，是因为**history是不支持删除历史**。但是当跳转到下一个页面的时候，可以用loaction.repalce('urlA', 'urlB')，它可以用urlB替换urlA，相当于删除了当前页面的历史记录。虽然但是，这个方案还是有体验问题，它会页面刷新，如果想要无刷新，可以通过replaceState实现。[网站](https://www.ncmchina.com/collections.html)的作品详情页面就是通过无刷新的方式实现的。



**问题场景复现：** 假设用户从作品列表页面（[collections](https://www.ncmchina.com/collections.html)）点击跳转到页面详情页（[workTemplate](https://www.ncmchina.com/workTemplate.html?id=75)），TA切换查看了15部作品详情后，现在TA希望点击浏览器的返回到作品列表页面，TA需要点击15次才能回到作品列表。类似场景则可以通过`replaceState`提升用户体验，（此处不考虑其他埋点等因素～）。

可以点击demo，[githubPage](https://shineasyr.github.io/FELearning/repalceState.html?id=1)自行访问体验，具体效果可参考下方GIF点击
![优化前后对比]({{ site.url }}/assets/images/replaceState.gif)


那如何科学使用replaceState实现无刷新切换呢？接下来将有详细内容展开～～～


## replaceState
`replaceState`与`pushState`类似，其主要区别就是第一个参数，state和stateObj有略微不同，stateObj的值可以为null，state不可以。
```javascript
history.pushState(state, title [, url])
history.replaceState(stateObj, title, [url])
```
其中url参数限制必须为同源，否则会抛出异常。未设定时仍为当前的url。

上方的demo，核心代码见下方～
```html
<h3>优化后的方式-replace替换</h3>
<p class="jsRepalceTxt">当前为默认数据</p>
<div class="jsLink">
  <a class="page-link" href="?id=1">id =1</a>
  <a class="page-link" href="?id=2">id =2</a>
  <a class="page-link" href="?id=3">id =3</a>
  <a class="page-link" href="?id=4">id =4</a>
</div>
```
```javascript
var allDatainfo = ['id为1时，展示的内容', 'id为2时，展示的内容', 'id为3时，展示的内容', 'id为4时，展示的内容'];
var allLinks = document.querySelector('.jsLink');
var jsRepalceTxt = document.querySelector('.jsRepalceTxt');
allLinks.addEventListener('click', function (e) {
  e.preventDefault();
  var el = e.target;
  if (el && el.getAttribute('href') ) {
    var currId = parseInt(el.getAttribute('href').split("id=")[1]);
    jsRepalceTxt.innerText = allDatainfo[currId - 1];
    if (history.replaceState) {
      history.replaceState(null, document.title, location.href.replace(/id=\d+/, 'id=' + currId));
    } else {
      // 属性不兼容则降级体验
      window.location.herf = window.location.herf.replace(/id=\d+/, 'id=' + currId);
    }
  }
});
```

## 参考文章
* [SPA 路由三部曲之核心原理](https://zhuanlan.zhihu.com/p/348764966)
* [简单聊聊H5的pushState与replaceState](https://juejin.cn/post/6844903558576341000)
* [HTML5 简介（三）：利用 History API 无刷新更改地址栏](https://www.renfei.org/blog/html5-introduction-3-history-api.html)