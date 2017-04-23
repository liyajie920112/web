# Canvas

## 什么是Canvas
- 说明: canvas是HTML5提供的一个新标签, `<canvas></canvas>`

## Canvas默认
- 默认宽高: 300 x 150

## 案例一 划线
```html
<canvas id='demo' width='200' height='200'></canvas>
```
```js
// 获取画布
var canvas = document.getElementById('demo');
// 获取画布2d上下文
var cgx = canvas.getContext('2d');
// 移动画笔到 20,20 的位置
cgx.moveTo(20,20);
// 开始划线, 结束位置为 100,20
cgx.lineTo(100,20);
// 接着上一条线继续画 终点为 20,50
cgx.lineTo(20,50);
// 开始渲染(描边)
cgx.stroke();
```
!> 注意点: 如果通过css设置`canvas`宽高, `canvas`中所画的东西会被放大或缩小.我们要通过属性的方式来给`canvas`设置宽高.

## 案例二 划线, 画新线`cgx.beginPath()`

```html
<canvas id='canvas'></canvas>
```

```js
var canvas = document.getElementById('canvas');
var cgx = canvas.getContext('2d');
cgx.moveTo(10,10);
cgx.lineTo(10,100);
cgx.lineWidth = 5;
cgx.strokeStyle = 'red';
cgx.stroke();

cgx.moveTo(20,20);
cgx.lineTo(20,100);
cgx.lineWidth = 2;
cgx.strokeStyle = 'blue';
cgx.stroke();
```

!> 上面这种情况在画第二条线的时候, 会先重复第一条线的路径. 要解决这种问题, 需要在画第二条线之前使用`cgx.beginPath()`

## 案例三 闭合的线 `cgx.closePath()`

```html
<canvas id='canvas'></canvas>
```
```js
var canvas = document.getElementById('canvas');
var cgx = canvas.getContext('2d');
cgx.moveTo(10,10);
cgx.lineTo(10,100);
cgx.lineTo(20,140);
cgx.strokeStyle='red';
// 闭合标签
cgx.closePath();
cgx.stroke();
```
## 案例四 画坐标系

```css
canvas { border:1px solid #0094ff; }
```
```html
<canvas id='canvas' width='600' height='400'></canvas>
```
```js
var canvas = document.getElementById('canvas');
var cgx = canvas.getContext('2d');
// 行高 5
var rowHeight = 8;
// 列宽 5
var colWidth = 8;
// canvas 宽度和高度
var canvasWidth = canvas.width;
var canvasHeight = canvas.height;

// 计算共多少行
var rows = parseInt(canvasHeight / rowHeight);

// 计算供多少列
var cols = parseInt(canvasWidth / colWidth);

// 实现线不模糊的效果
cgx.translate(0.5, 0.5);

// 绘制行
for (var i = 1, len = rows; i < len; i++) {
    cgx.beginPath();
    cgx.moveTo(0, i * rowHeight);
    cgx.lineTo(canvasWidth, i * rowHeight);
    cgx.lineWidth = 0.5;
    cgx.strokeStyle = '#ff6440';
    cgx.stroke();
}

// 绘制列
for (var i = 1, len = cols; i < len; i++) {
    cgx.beginPath();
    cgx.moveTo(i * colWidth, 0);
    cgx.lineTo(i * rowHeight, canvasHeight);
    cgx.lineWidth = 0.5;
    cgx.stroke();
}

// 绘制坐标轴

// 坐标轴原点
var ox = 40,
    oy = canvasHeight - ox;
//设置坐标系宽高
var ow = 400,
    oh = 300; 
// 设置y轴结束的位置 x,y
var yEndy = oy - oh,
    yEndx = ox;
// 设置x轴结束的位置 x,y
var xEndy = oy,
    xEndx = ox + ow;
// 划y轴
cgx.beginPath();
cgx.moveTo(ox, oy);
cgx.lineTo(yEndx, yEndy);
// 画箭头
cgx.lineTo(yEndx - 8, yEndy + 8);
cgx.lineTo(yEndx, yEndy);
cgx.lineTo(yEndx + 8, yEndy + 8);

cgx.lineWidth = 2;
cgx.strokeStyle = 'green';

cgx.stroke();

// 画x轴
cgx.beginPath();
cgx.moveTo(ox, oy);
cgx.lineTo(xEndx, xEndy);
// 画箭头
cgx.lineTo(xEndx - 8, xEndy - 8);
cgx.lineTo(xEndx, xEndy);
cgx.lineTo(xEndx - 8, xEndy + 8);
cgx.stroke();

// 画折线
// 设置点的数组
var points = [20, 30, 100, 80, 150,90,10,100];
// 计算x轴上每个点的间距
var marginWidth = parseInt(ow / points.length);
cgx.beginPath();
cgx.moveTo(ox, oy);
// 开始画线
for (var i = 0, len = points.length; i < len; i++) {
    cgx.lineTo(ox + ((i+1) * marginWidth), oy - points[i]);
}
cgx.stroke();
```
[点击此处查看Demo](https://codepen.io/liyajie/pen/EmyRqj)