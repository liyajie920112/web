# Canvas

## 什么是Canvas
- 说明: canvas是HTML5提供的一个新标签, `<canvas></canvas>`

## 角度和弧度的换算

- 1° = 2πr / 360 弧长
- 1° = 2π / 360  弧度

?> 弧度就是把圆的 周长(2πr) 平均分成r份, 每一份就是一弧度 `2πr / r = 2π`

## Canvas默认
- 默认宽高: 300 x 150

## 案例一 划线 `ctx.lintTo(x,y)`
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

<p data-height="265" data-theme-id="light" data-slug-hash="EmyRqj" data-default-tab="html" data-user="liyajie" data-embed-version="2" data-pen-title="canvas-coordinate" class="codepen">See the Pen <a href="https://codepen.io/liyajie/pen/EmyRqj/">canvas-coordinate</a> by liyajie (<a href="http://codepen.io/liyajie">@liyajie</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

[点击此处查看Demo](https://codepen.io/liyajie/pen/EmyRqj)

## 案例五 画矩形`ctx.rect(x,y,width,height)`

```js
var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d');

// 方式一
// ctx.rect(x,y,width,height);
ctx.rect(10,10,200,100);
ctx.lineWidth = 5;
ctx.stroke();

// 方式二
// ctx.fillRect(x,y, width, height);
ctx.fillRect(10,150,200,100); // 这种方式会默认填充颜色, 颜色为黑色

// 方式三
// ctx.strokeRect(x,y, width, height);
ctx.strokeRect(10,300,200,100);// 这种方式会默认描边
```

## 案例六 画圆弧 `ctx,arc(x,y,r,初始弧度,弧度,是否是逆时针)`

?> 角度转弧度 `弧度 = 角度 * Math.PI / 180`

```js
var canvas = document.getElementById('canvas');
var cgx = canvas.getContext('2d');
// cgx.arc(x,y,r,偏移弧度,弧度,是否是顺时针) false: 顺时针(默认), true: 逆时针
cgx.arc(200,200,100,10 * Math.PI / 180, 90 * Math.PI / 180,false)
cgx.stroke();
```

## 非零正交原则

非零正交原则示例图

![非零正交原则示例图](/document/images/canvas001.jpg)

> 非零环绕规则：

对于路径中指定范围区域，从该区域内部画一条足够长的线段，使此线段的完全落在路径范围之外。

> 非零环绕规则计数器：

然后，将计数器初始化为0，每当这个线段与路径上的直线或曲线相交时，就改变计数器的值，如果是与路径顺时针相交时，那么计数器就加1， 如果是与路径逆时针相交时，那么计数器就减1.
如果计数器始终不为0，那么此区域就在路径范围里面，在调用fill()方法时，浏览器就会对其进行填充。如果最终值是0，那么此区域就不在路径范围内，浏览器就不会对其进行填充。

> 从上图中看

第一条线段：根据非零环绕规则，这条直线只经过路径一次且路径是逆时针方向，那么计数器为-1;根据浏览器对计数器的计算原理得出，当调用fill()方法时浏览器会填充此区域。

第二条线段：根据非零环绕规则，这条直线经过路径二次且路径都是逆时针方向，那么计数器为-2;根据浏览器对计数器的计算原理得出，当调用fill()方法时浏览器会填充此区域。

第三条线段：根据非零环绕规则，这条直线经过路径二次;第一次经过的路径是逆时针方向，计数器则为-1; 第二次经过的路径是顺时针方向，计数器为1;原因计数器的最终值为0-1+1 = 0;所以根据浏览器对计数器的计算原理得出，当调用fill()方法时浏览器不会填充此区域

> 使用非零正交原则画一个圆环

```js
var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d');

ctx.arc(200,200,200,0, 360 * Math.PI / 180,false);// 顺时针
ctx.arc(200,200,100,0, 360 * Math.PI / 180,true);// 逆时针
ctx.fillStyle = '#0094ff';
ctx.fill();
```

## 绘制文本`ctx.fillText` `ctx.strokeText`

```js
var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d');

// 设置字体大小
ctx.font = '80px Arial';

// 设置阴影
ctx.shadowStyle = 'green';

// 设置阴影模糊度
ctx.shadowBlur = 10;

// 设置阴影偏移
ctx.shadowOffsetX = 5;
ctx.shadowOffsetY = 5;

// 设置垂直方向上的基线
ctx.textBaseline = 'top';

// 绘制一个填充文本
ctx.fillStyle = '#0094ff';
ctx.fillText('Hello World',20,20);

// 绘制一个空心文本
ctx.strokeStyle = '#ff6666';
ctx.strokeText('Hello World', 20,100);
```

>`ctx.textBaseline`

- alphabetic	默认。文本基线是普通的字母基线。
- top	文本基线是 em 方框的顶端。
- hanging	文本基线是悬挂基线。
- middle	文本基线是 em 方框的正中。
- ideographic	文本基线是表意基线。
- bottom	文本基线是 em 方框的底端。

> `ctx.textAlign`

- start	默认。文本在指定的位置开始。
- end	文本在指定的位置结束。
- center	文本的中心被放置在指定的位置。
- left	文本左对齐。
- right	文本右对齐。

## 绘制图片 `ctx.drawImage`

> 绘制图片

```js
var img = new Image();
img.src = 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1493554050&di=0f914f222af2cb5f44fd5880971c9a5c&imgtype=jpg&er=1&src=http%3A%2F%2Fimg.carvenus.com%2Fim%2Fnews%2F2014%2F04%2F140416_n01a.jpg';

var w = 300;
img.onload = function(){
    // 等比例计算宽高
    var h = this.height / this.width / w;
    ctx.drawImage(this,0,0,w,h);
}
```
> 裁剪图片

!> 截图是针对原图大小来截

```js
var img = new Image();
img.src = 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1493554050&di=0f914f222af2cb5f44fd5880971c9a5c&imgtype=jpg&er=1&src=http%3A%2F%2Fimg.carvenus.com%2Fim%2Fnews%2F2014%2F04%2F140416_n01a.jpg';

// ctx.drawImage(img对象, 开始截图的x位置,开始截图的y,截图的宽,截图的高, 截完之后的图的x,截完之后的图的y,截完之后的图的宽,截完之后的图的高);
// 第2个~第5个参数说的是截图的 位置和宽高
// 第6个~第9个参数说的是截完的图的 位置和宽高
img.onload = function(){
    ctx.drawImage(this,0,0,100,100,90,80,120,120);
}
```

