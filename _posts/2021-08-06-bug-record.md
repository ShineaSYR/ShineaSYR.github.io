---
layout: post
title: bug records🐞
categories: [bug]
description: 记录在学习工作中遇到的问题，各个方面可能都存在
keywords: bug
---

在学习和工作的过程中，好像总是会遇到一些奇奇怪怪的question or bug，或许可以解决，或许会成为小难题被遗忘，此帖主要是记录这些小奇葩们～

## 图片丢失旋转信息
最近在配合调整内部图片上传服务的时候，当用iPhone手机进行上传时，出现了黑边的情况。问题截图如下图所示，**问题有待时间去复现**。

![问题截图]({{ site.url }}/assets/images/bugrecord_pic_rotateinfo001.png)

底层方法是通过Canvas2Blob，对图片有做压缩处理，防止图片过大的情况。
`.HEIC`文件素材是通过iPhone手机拍摄的live photo，在拍摄时为竖屏拍摄。

目前猜测是在canvas绘制的时候，将原素材的旋转信息丢失了。然后后端在绘制的时候，又依旧原图的高度来创建，此时尾部就出现了黑色。且图片旋转倒置了。

PS： 原始图片素材，会有[exif信息]()，包含[Orientation参数](https://hkzww.com/%E5%9B%BE%E7%89%87%E6%98%BE%E7%A4%BA%E6%97%8B%E8%BD%AC%E9%97%AE%E9%A2%98.html)，图片经过第三方软件处理or传输，基本上会丢失旋转信息。
**附件：**
[素材原图1]({{ site.url }}/assets/images/bugrecord_pic_rotateinfo002.HEIC)
[素材原图1]({{ site.url }}/assets/images/bugrecord_pic_rotateinfo003.HEIC)

