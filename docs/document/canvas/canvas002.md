## 线性渐变 context.createLinearGradient(x0,y0,x1,y1)

```js
// 从(0,0) ~ (100,100)位置进行渐变
var gradient = ctx.createLinearGradient(0,0,100,100);
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

```js

```

## 变换(transform)

## 裁剪区域

## 将图片转换成base64字符串

