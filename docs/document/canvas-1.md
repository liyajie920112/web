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