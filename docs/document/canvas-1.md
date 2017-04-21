# Canvas

## 什么是Canvas
- 说明: canvas是HTML5提供的一个新标签, `<canvas></canvas>`

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
// 开始渲染
cgx.stroke();
```
!> 注意点: 如果通过css设置`canvas`宽高, `canvas`中所画的东西会被放大或缩小.我们要通过属性的方式来给`canvas`设置宽高.