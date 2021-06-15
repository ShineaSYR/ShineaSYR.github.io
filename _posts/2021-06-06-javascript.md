---
layout: post
title: JavaScript的经典场景及解决方案
categories: [JavaScript]
description: 汇总JavaScript的经典场景问题以及方案
keywords: JavaScript, Algorithms
---

## 数字格式化
**场景**：有些场景需要将数字展示千分位，如12787646，展示为12,787,646
**解决方案**：
```
function toThousands(num) {
     return (num || 0).toString().replace(/(\d)(?=(?:\d{3})+$)/g, '$1,');
}
```
参考链接：https://blog.csdn.net/sushauai/article/details/52958162


