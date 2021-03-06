## 线性渐变 context.createLinearGradient(x0,y0,x1,y1)

```js
// 从(0,0) ~ (100,100)位置进行渐变
var gradient = ctx.createLinearGradient(0,0,100,100);

// 添加渐变梯度
gradient.addColorStop(0,'red');
gradient.addColorStop(1,'blue');
// 将渐变
ctx.fillStyle = gradient;
ctx.fill(); 
```
下图是个线性渐变

![](/document/images/canvas/linearGradient.png)

!> 注意渐变的坐标位置(x,y)是相对于画布的的原点来设置的.


## 径向渐变 context.createRadialGradient(x0,y0,r0,x1,y1,r1);

!> x0,y0,r0,x1,y1,r1 是相对于画布左上角原点的

```js
var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d');

// 设置径向渐变, 从(200,200)开始, 到(200,200)结束, 渐变长度为r1-r0
var radial = ctx.createRadialGradient(200,200,0,200,200,100);
radial.addColorStop(0,'red');
radial.addColorStop(0.5,'blue');
radial.addColorStop(1,'green');
ctx.arc(200,200,100,0,2 * Math.PI);
ctx.fillStyle = radial;
ctx.fill();
```

下图是径向渐变

![](/document/images/canvas/radialgradient.png)

## 变换(transform)

<iframe height='377' scrolling='no' title='canvas-transform' src='//codepen.io/liyajie/embed/EmgZEy/?height=377&theme-id=light&default-tab=result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/liyajie/pen/EmgZEy/'>canvas-transform</a> by liyajie (<a href='http://codepen.io/liyajie'>@liyajie</a>) on <a href='http://codepen.io'>CodePen</a>.
</iframe>

## 裁剪区域(context.clip)

```js
var canvas = document.getElementById('cavnas');
var ctx = canvas.getContext('2d');

// 绘制一个圆
ctx.arc(100,100,100,0,2 * Math.PI);

// 开始裁剪, 这行代码之后, 只有在上面绘制的图形之内的图形才能看到
ctx.clip();

var img = new Image();
img.src = 'baby.jpg';
img.onload = function(){
    // 绘制图片
    ctx.drawImage(this,0,0,100,100);
}
```


## 将图片转换成base64字符串

```js
var canvas = document.getElementById('canvas');
// 参数一: 图片格式
// 参数二: 图片质量
var base64 = canvas.toDataURL('image/jpeg',1);

console.log(base64);
```

## Canvas性能优化
- 将小画布画到大画布上.

## 设置画布透明度 globalAlpha

- `context.globalAlpha(0~1)`

## 合成属性 `context.globalCompositeOperation`

- source-over (default)

这是默认设置，新图形会覆盖在原有内容之上。

![](https://mdn.mozillademos.org/files/220/Canvas_composite_srcovr.png)

- destination-over

会在原有内容之下绘制新图形。

![](https://mdn.mozillademos.org/files/215/Canvas_composite_destovr.png)

- source-in

新图形会仅仅出现与原有内容重叠的部分。其它区域都变成透明的。

![](https://mdn.mozillademos.org/files/218/Canvas_composite_srcin.png)

- destination-in

原有内容中与新图形重叠的部分会被保留，其它区域都变成透明的。

![](https://developer.mozilla.org/@api/deki/files/66/=Canvas_composite_destin.png)

- source-out

结果是只有新图形中与原有内容不重叠的部分会被绘制出来。

![](https://developer.mozilla.org/@api/deki/files/72/=Canvas_composite_srcout.png)

- destination-out

原有内容中与新图形不重叠的部分会被保留。

![](https://developer.mozilla.org/@api/deki/files/67/=Canvas_composite_destout.png)

- source-atop

新图形中与原有内容重叠的部分会被绘制，并覆盖于原有内容之上。

![](https://developer.mozilla.org/@api/deki/files/70/=Canvas_composite_srcatop.png)

- destination-atop

原有内容中与新内容重叠的部分会被保留，并会在原有内容之下绘制新图形

![](https://developer.mozilla.org/@api/deki/files/65/=Canvas_composite_destatop.png)

- lighter

两图形中重叠部分作加色处理。

![](https://developer.mozilla.org/@api/deki/files/69/=Canvas_composite_lighten.png)

- darken

两图形中重叠的部分作减色处理。

![](https://developer.mozilla.org/@api/deki/files/64/=Canvas_composite_darken.png)

- xor

重叠的部分会变成透明。

![](https://developer.mozilla.org/@api/deki/files/74/=Canvas_composite_xor.png)

- copy

只有新图形会被保留，其它都被清除掉。

![](https://developer.mozilla.org/@api/deki/files/63/=Canvas_composite_copy.png)

## 刮刮卡案例

- 该案例主要用到的是合成属性`globalCompositeOperation`

<iframe height='202' scrolling='no' title='canvas-刮奖' src='//codepen.io/liyajie/embed/OmbVyy/?height=202&theme-id=light&default-tab=result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/liyajie/pen/OmbVyy/'>canvas-刮奖</a> by liyajie (<a href='http://codepen.io/liyajie'>@liyajie</a>) on <a href='http://codepen.io'>CodePen</a>.
</iframe>

## 时钟效果 纯Canvas

?> 重点是时间的换算, 角度的换算

<iframe height='525' scrolling='no' title='canvas-Clock' src='//codepen.io/liyajie/embed/dWOovB/?height=525&theme-id=light&default-tab=result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/liyajie/pen/dWOovB/'>canvas-Clock</a> by liyajie (<a href='http://codepen.io/liyajie'>@liyajie</a>) on <a href='http://codepen.io'>CodePen</a>.
</iframe>


## Canvas 画一个五角星

<p data-height="371" data-theme-id="light" data-slug-hash="wdmNBw" data-default-tab="js,result" data-user="liyajie" data-embed-version="2" data-pen-title="Canvas Star" class="codepen">See the Pen <a href="https://codepen.io/liyajie/pen/wdmNBw/">Canvas Star</a> by liyajie (<a href="http://codepen.io/liyajie">@liyajie</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>