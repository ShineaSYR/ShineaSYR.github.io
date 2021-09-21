---
layout: post
title: 🌈web color 话我知 
categories: [Javascript]
description: 多种颜色模式互相转换
keywords: replaceState, 用户体验
---

“五彩斑斓的黑、五彩斑斓的白”，网页上展示的颜色千千万万，总离不开RGB、HEX、HSL。作为前端开发的你我他，总有可能会有一天遇到不同颜色模式转换的魔幻场景。

## 颜色模式概念
### RGB
三原色光模式 (RGB color model)，主要为计算机等电子设备的色彩模式，通过红（Red）、绿（Green）、蓝（Blue）的三个原色光，色光以不同的比例相加，以合成产生各种色彩光。

目前主流为24比特模式，使用三个8位无符号整数（0到255）表示红色、绿色和蓝色的强度。
### HEX
HEX可以理解为对三原色的十六进制（hexadecimal）展示。
### HSL
HSL是一种将RGB色彩模型中的点在圆柱坐标系中的表示法。该表示法试图做到比基于笛卡尔坐标系的几何结构RGB更加直观。

HSL即色相、饱和度、亮度（英语：Hue, Saturation, Lightness）。
色相（H）是色彩的基本属性，就是平常所说的颜色名称，如红色、黄色等。
饱和度（S）是指色彩的纯度，越高色彩越纯，低则逐渐变灰，取0-100%的数值。
明度（V），亮度（L），取0-100%。

![RGB2HSL]({{ site.url }}/assets/images/color-exchange001.png)
![HSL2RGB]({{ site.url }}/assets/images/color-exchange002.png)

## 模式转换
### RGB&HEX
```javascript
// rgb/rgba颜色转hex
function cRgbToHex (rgb) {
  if (!rgb || !(/^rgb\((\d+),\s*(\d+),\s*(\d+)/i.test(rgb) || /^rgba\((\d+),\s*(\d+),\s*(\d+),\s*([0|1]?\.?\d+)/i.test(rgb))) {
    return rgb(0, 0, 0)
  }
  let arr = [], arrA = [];

  // 如果是rgb(a)色值
  arr = rgb.match(/^rgb\((\d+),\s*(\d+),\s*(\d+)/i);
  arrA = rgb.match(/^rgba\((\d+),\s*(\d+),\s*(\d+),\s*([0|1]?\.?\d+)/i);
  const hex = (x) => ('0' + parseInt(x, 10).toString(16)).slice(-2);

  if (arr && arr.length == 4) {
    return `#${hex(arr[1])}${hex(arr[2])}${hex(arr[3])}`;
  }

  if (arrA && arrA.length == 5) {
    return `#${hex(arrA[1])}${hex(arrA[2])}${hex(arrA[3])}${Math.round(arrA[4] * 255).toString(16).padStart(2, '0')}`;
  }
}

// 16进制颜色转换成rgb/rgba颜色表示
function cHexToRgb (hex) {
  hex = (hex || '').replace('#', '');
  if (!(hex.length == 3 || hex.length == 4 || hex.length == 6 || hex.length == 8)) {
    return `rgb(0, 0, 0)`
  }

  if (hex.length == 3 || hex.length == 4) {
    hex = hex.split('').map(function (char) {
        return char + char;
    }).join('');
  }

  const r = parseInt(hex.slice(0, 2), 16);
  const g = parseInt(hex.slice(2, 4), 16);
  const b = parseInt(hex.slice(4, 6), 16);

  if (hex == 8){
    const a = parseInt(hex.slice(6, 8), 16) / 255;
    if (a !== 1) {
      return `rgba(${r}, ${g}, ${b}, ${a})`
    }
  }
  return `rgb(${r}, ${g}, ${b})`
}
```
### RGB&HSL
> PS：原理可以参考概念里面的介绍
### HEX&HSL

```javascript
// 16进制颜色转换成hsl颜色表示
function funHexToHsl (hex) {
  hex = (hex || '').replace('#', '');

  if (hex.length == 3 || hex.length == 4) {
    hex = hex.split('').map(function (char) {
      return char + char;
    }).join('');
  }

  const r = parseInt(hex.slice(0, 2), 16) / 255;
  const g = parseInt(hex.slice(2, 4), 16) / 255;
  const b = parseInt(hex.slice(4, 6), 16) / 255;

  const max = Math.max(r, g, b);
  const min = Math.min(r, g, b);
  let h, s;
  const l = (max + min) / 2;

  if (max == min) {
  // 非彩色
    h = s = 0;
  } else {
    const d = max - min;
    s = l > 0.5 ? d / (2 - max - min) : d / (max + min);
    switch (max) {
      case r: h = (g - b) / d + (g < b ? 6 : 0); break;
      case g: h = (b - r) / d + 2; break;
      case b: h = (r - g) / d + 4; break;
    }
    h /= 6;
  }
  if (hex.length == 8) {
    const a = parseInt(hex.slice(6, 8), 16) / 255;
    return [h, s, l, a];
  }
  return [h, s, l];
}

// hsl颜色转换成十六进制颜色
function funHslToHex (h, s, l, a) {
  let r, g, b;

  if (s == 0) {
  // 非彩色的
    r = g = b = l;
  } else {
    const hue2rgb = function (p, q, t) {
      if (t < 0) t += 1;
      if (t > 1) t -= 1;
      if (t < 1 / 6) return p + (q - p) * 6 * t;
      if (t < 1 / 2) return q;
      if (t < 2 / 3) return p + (q - p) * (2 / 3 - t) * 6;

      return p;
    };

    const q = l < 0.5 ? l * (1 + s) : l + s - l * s;
    const p = 2 * l - q;
    r = hue2rgb(p, q, h + 1 / 3);
    g = hue2rgb(p, q, h);
    b = hue2rgb(p, q, h - 1 / 3);
  }

  const arrRgb = [Math.round(r * 255), Math.round(g * 255), Math.round(b * 255)];

  // Alpha值
  if (a) {
    arrRgb.push(Math.round(a * 255));
  }

  return arrRgb.map(rgb => { 

    if (rgb.length == 1) {
        return '0' + rgb;
    }

    return rgb;
  }).join('');
}
```
**🔗 参考链接**
- [三原色光模式 ](https://zh.wikipedia.org/wiki/%E4%B8%89%E5%8E%9F%E8%89%B2%E5%85%89%E6%A8%A1%E5%BC%8F)
- [HSL](https://zh.wikipedia.org/wiki/HSL%E5%92%8CHSV%E8%89%B2%E5%BD%A9%E7%A9%BA%E9%97%B4)