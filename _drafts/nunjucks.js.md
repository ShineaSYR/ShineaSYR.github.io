---
layout: post
title: Nunjucks.js的使用注意
categories: [JavaScript]
description: 项目中有需要服务端直出的页面，使用了Nunjucks.js，在项目开发的过程中，会遇到小问题，记录以便后续有类似的场景。
keywords: Nunjucks.js, 使用注意,模板直出
---
项目中有需要服务端直出的页面，使用了Nunjucks.js，在项目开发的过程中，会遇到小问题，记录以便后续有类似的场景。
## 对象输出+字符转义
```node
{% set items = ["a", 1, { b : true}] %}
{{ items | dump }}
// ["a",1,{"b":true}]
```


