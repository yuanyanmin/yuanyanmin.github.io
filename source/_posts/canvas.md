---
toc: true
title: canvas
date: 2018-09-20 17:05:26
tags:
 - canvas
categories:
---
**天气**： :sunny:
**心情**： :laughing:

HTML5 canvas 元素用于图形的绘制，通过脚本 (通常是JavaScript)来完成

<!--more-->
## 绘制图形

### canvas 元素

```
<canvas id="tutorial" width="150" height="150"></canvas>

// 当浏览器不支持 canvas 时，显示替换内容
<canvas id="tutorial" width="150" height="150">
    current stock price: $3.15 +0.15
</canvas>
```

### 渲染上下文

getContext() 方法： 用来获得渲染上下文和它的绘画功能。
只有一个参数：上下文的格式。

```
var canvas = documeng.getElementById('tutorial');
var ctx = canvas.getContext('2d');

// 检查支持性
var canvas = documeng.getElementById('tutorial');
if(canvas.getContext) {
    var ctx = canvas.getContext('2d');
}else {
    // canvas-unsupported code here
}
```

### 绘制矩形

```
// 绘制一个填充的矩形
fillRect(x, y, width, height);

// 绘制一个矩形的边框
strokeRect(x, y, width, height);

// 清除指定矩形区域，让清除部分完全透明
clearRect(x, y, width, height);
```

### 绘制路径

图形的基本元素是路径。路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。一个路径，甚至一个子路径，都是闭合的。

```
/*
*  1. 首先，你需要创建路径起始点
*  2. 然后，使用画图命令画出路径
*  3. 之后，把路径封闭
*  4. 最后，通过描边或者填充路径区域来渲染图形
*/

// 生成路径
beginPath();

// 闭合路径
closePath();

// 通过线条来绘制图形轮廓
stroke();

// 通过填充路径的内容区域生成实心的图形
fill();

// 第一条路径构造命令通常被视为是moveTo（）
// 要在设置路径之后专门指定你的起始位置。

// 当你调用fill()函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用closePath()函数。但是调用stroke()时不会自动闭合。

```

### 绘制一个三角形

```
function draw() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext) {
    var ctx = canvas.getContext('2d');

    ctx.beginPath();
    ctx.moveTo(75, 50);
    ctx.lineTo(100, 75);
    ctx.lineTo(100, 25);
    ctx.fill();
  }
}
```

### 线

```
// 绘制直线，需要用到 lineTo() 方法
// 路径使用填充（fill）时，路径自动闭合，使用描边（stroke）则不会闭合路径。
```

### 圆弧

```
/*
 * arc() 方法
 * arc(x, y, radius, startAngle, endAngle, anticlockwise)
 * 以（x, y）为圆心的，以 radius 为半径的圆弧，从 startAngle 开始到 endAngle 结束，按照 anticlockwise 给定的方向（默认为顺时针）生成。
 * startAngle 、endAngle 开始、结束的弧度。 弧度=(Math.PI/180) * 角度
 * anticlockwise : 布尔值，true 时，逆时针；否则顺时针。
 */
```

### 矩形

```
// 绘制一个左上角坐标为(x,y),宽高为width以及height的矩形
rect(x, y, width, height)
```

## 使用样式和颜色

### 色彩

```
// 设置图形的填充颜色
fillStyle = color

// 设置图形轮廓的颜色
strokeStyle = color

// 透明度   0~1
globalAlpha = transparencyValue

```

### 线型 Line styles

```
// 设置线条宽度
lineWidth = value

// 设置线条末端样式
lineCap = type

// 设定线条与线条间接合处的样式
lineJoin = type

// 限制当两条相交时交接处的最大长度
miterLimit = value

// 返回一个包含当前虚线样式，长度为非负偶数的数组
getLineDash()

// 设置当前虚线样式
setLineDash(segments)

// 设置虚线样式的起始偏移量
lineDashOffset = value
```

### 阴影 Shadows

```
// 阴影在 x 轴的延伸距离
shadowOffsetX = float

// 阴影在 y 轴的延伸距离
shadowOffsetY = float

// 阴影的模糊程度
shadowBlur = float

// 阴影的模糊程度
shadowColor = color
```

## 绘制文本

### 渲染文本

```
// 在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的
fillText(text, x, y, [,maxWidth])

// 在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的
strokeText(text, x, y, [,maxWidth])
```

### 有样式文本

```
// 文本样式 (默认为： 10px sans-serif)
font = value

// 文本对齐选项 start(默认),end,left,right,center
textAlign = value

// 基线对齐选项 top, hanging, middle, alphabetic(默认), ideographic, bottom
textBaseline = value

// 文本方向 ltr,rtl,inherit(默认)
direction = value

```

## 使用图像

可以用于动态的图像合成或者作为图形的背景，以及游戏界面（Sprites）等等

### 获取图片

```
// Image()函数构造出来的，或者任何的<img>元素
HTMLImageElement

var img = new Image();
img.src = "myImage.png"

// 用一个HTML的 <video>元素作为你的图片源，可以从视频中抓取当前帧作为一个图像
HTMLVideoElement

// 可以使用另一个 <canvas> 元素作为你的图片源
HTMLCanvasElement

// 这是一个高性能的位图，可以低延迟地绘制
ImageBitmap
```

### 绘制图片

一旦获得了源图对象，我们就可以使用 drawImage 方法将它渲染到 canvas 里

```
// image 是 image 或者 canvas 对象，x 和 y 是其在目标 canvas 里的起始坐标
drawImage(image, x, y)

// 缩放  width 和 height，用来控制当向canvas画入时应该缩放的大小
drawImage(image, x, y, width, height)

// 切片
// 第一个参数和其它的是相同的,都是一个图像或者另一个 canvas 的引用
// 前4个是定义图像源的切片位置和大小，后4个则是定义切片的目标显示位置和大小。
drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)

```


