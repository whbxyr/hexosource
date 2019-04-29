---
title: 最大宽高限制下的自适应比例尺寸计算
date: 2019-04-29 22:47:45
categories: [技术类-算法]
tags: [JavaScript, 算法]
---
&emsp;&emsp;今天在做图片涂鸦的时候遇到了一个图片尺寸自适应的问题，即给一个有最大宽度跟最大高度限制的盒子，要将一张大小不确定的图片在保持宽高比的情况下完全放入盒子中，即既要图片能完全放入盒子中又要图片不变形。

&emsp;&emsp;其涉及到的主要是图片宽高的重新计算，抽象一下就是一个算法，即随意给定四个正数（x1, y1, x2, y2），求 x3 以及 y3，其需要满足的条件如下：

&emsp;&emsp;<font color="#f00">（1）当 x2 <= x1 && y2 <= y1 时，则 x3 = x2，y3 = y2。</font>

&emsp;&emsp;<font color="#f00">（2）当 x2 >= x1 || y2 >= y1 时，要求 x3 以及 y3 是满足 x3 <= x1 && y3 <= y1 && (x3 / y3 === x2 / y2) 的最大值。</font>

&emsp;&emsp;最终算法（es6 版本）如下。
```javascript
/**
 * 计算自适应图片的尺寸
 * @param {Number} x1 最大宽度，x1 > 0，必选
 * @param {Number} y1 最大高度，y1 > 0，必选
 * @param {Number} x2 原始图片宽度，x2 > 0，必选
 * @param {Number} y2 原始图片高度，y1 > 0，必选
 * @return {Object} 自适应后的新图片尺寸
 */
export const picBox = (x1, y1, x2, y2) => {
  let x3 = x2, y3 = y2
  const time1 = x1 / y1
  const time2 = x2 / y2
  if (time1 >= 1 && time2 >= 1 || time1 <= 1 && time2 <= 1) {
    if (time2 < time1) {
      if (y2 > y1) {
        x3 = (y1 * x2) / y2
        y3 = y1
      }
    } else {
      if (x2 > x1) {
        y3 = (x1 * y2) / x2
        x3 = x1
      }
    }
  } else if (time1 >= 1 && time2 <= 1) {
    if (y2 > y1) {
      x3 = (y1 * x2) / y2
      y3 = y1
    }
  } else if (time1 <= 1 && time2 >= 1) {
    if (x2 > x1) {
      y3 = (x1 * y2) / x2
      x3 = x1
    }
  }
  return {
    x3,
    y3
  }
}
```
&emsp;&emsp;现在假如图片盒子的最大宽度限制是 400，最大高度限制是 300，一张图片的尺寸是 500 * 600，那么放入盒子中的自适应图片尺寸分别是
```javascript
const newSize = picBox(400, 300, 500, 600)
newSize.x3 // 自适应后的宽度
newSize.y3 // 自适应后的高度
```