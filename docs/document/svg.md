# SVG矢量图绘制

## 基本图形
- `<rect>` 矩形
    - `x`         矩形左上角x坐标
    - `y`         矩形左上角y坐标
    - `width`     矩形宽
    - `height`    矩形高
    - `rx`    y轴圆角半径
    - `ry`    y轴圆角半径
- `<circle>` 圆形
    - `cx` 圆心坐标 x
    - `cy` 圆心坐标 y
    - `r`  圆的半径
- `<ellipse>` 椭圆
    - `cx` 圆心坐标 x
    - `cy` 圆心坐标 y
    - `rx`  圆的半径 x
    - `ry`  圆的半径 y
- `<line>` 直线
    - `x1`  开始位置坐标 x
    - `y1`  开始位置坐标 y
    - `x2`  结束位置坐标 x
    - `y2`  结束位置坐标 y
- `<polyline>` 折线
    - `points` 存放每个点的坐标, 格式: `points = 'x1 y1 x2 y2 x3 y3 ...'`;
- `<polygon>` 多边形
    - `points` 存放每个点的坐标, 格式: `points = 'x1 y1 x2 y2 x3 y3 ...'`;
    - 与`polyline`的区别是最后一个点默认会连接到第一个点, 形成闭合的图形
## 基本属性
- fill 填充
- stroke    描边
- stroke-width  描边宽度
- transform 变换

## 基本图形示例

```svg
<svg xmlns="http://www.w3.org/2000/svg">
    <line x1='10' y1='10' x2='100' y2='100' stroke='red'>
    </line>
    <rect x='120' y='10' width='200' height='90' rx='5' stroke='red' fill='none'></rect>
    <circle cx='380' cy='50' r='30' stroke='blue' fill='none'></circle>
    <ellipse cx='50' cy='200' rx='50' ry='30' stroke='red' fill='none'></ellipse>
	<polyline points='250 200 300 200 260 300 200 250' stroke='red' fill='yellow'></polyline>
	<polygon points='250 200 300 200 260 300 200 250' transform='translate(100,0)' stroke='red' stroke-width='8' fill='#0094ff'></polygon>
</svg>
```

<svg xmlns="http://www.w3.org/2000/svg" height='320' width='700'>
    <line x1='10' y1='10' x2='100' y2='100' stroke='red'>
    </line>
    <rect x='120' y='10' width='200' height='90' rx='5' stroke='red' fill='none'></rect>
    <circle cx='380' cy='50' r='30' stroke='blue' fill='none'></circle>
    <ellipse cx='50' cy='200' rx='50' ry='30' stroke='red' fill='none'></ellipse>
	<polyline points='250 200 300 200 260 300 200 250' stroke='red' fill='yellow'></polyline>
	<polygon points='250 200 300 200 260 300 200 250' transform='translate(100,0)' stroke='red' stroke-width='8' fill='#0094ff'></polygon>
</svg>

## SVG 基本图形编辑器

[点击这里查看-SVG基本图形编辑器](https://codepen.io/liyajie/pen/OpdYLw)

## SVG中世界,视野,视窗

- `width, height` : 控制视窗
- `SVG代码` - 定义世界
- `viewBox, preserveAspectRatio` - 控制视野
- `preserveAspectRatio`取值
    - `align`值
        - `xMinYMin`：viewBox的<min-x>对齐viewport的最小X值，min-y对齐viewport的最小Y值。相当于backrgound-position: 0% 0%;。
        - `xMinYMid`：viewBox的<min-x>对齐viewport的最小X值，viewBox的Y轴中点对齐viewport的Y轴中点。相当于backrgound-position: 0% 50%;。
        - `xMinYMax`：viewBox的<min-x>对齐viewport的最小X值，min-y+<height>对齐viewport的最大Y值。相当于backrgound-position: 0% 100%;。
        - `xMidYMin`：viewBox的X轴中点对齐viewport的X轴中点，min-y对齐viewport的最小Y值。相当于backrgound-position: 50% 0%;。
        - `xMidYMid`（默认值）：viewBox的X轴中点对齐viewport的X轴中点，viewBox的Y轴中点对齐viewport的Y轴中点。相当于backrgound-position: 50% 50%;。
        - `xMidYMax`：viewBox的X轴中点对齐viewport的X轴中点，min-y+<height>对齐viewport的最大Y值。相当于backrgound-position: 50% 100%;。
        - `xMaxYMin`：viewBox的<min-x>+<width>对齐viewportX轴的最大值，min-y对齐viewport的最小Y值。相当于backrgound-position: 100% 0%;。
        - `xMaxYMid`：viewBox的<min-x>+<width>对齐viewportX轴的最大值，viewBox的Y轴中点对齐viewport的Y轴中点。相当于backrgound-position: 100% 50%;。
        - `xMaxYMax`：viewBox的<min-x>+<width>对齐viewportX轴的最大值，min-y+<height>对齐viewport的最大Y值。相当于backrgound-position: 100% 100%;
    - `meet`	保持宽高比并将viewBox缩放为适合viewport的大小
    - `slice`	保持宽高比并将所有不在viewport中的viewBox剪裁掉
    - `none`	不保存宽高比。缩放图像适合整个viewbox，可能会发生图像变形

```svg
<svg xmlns='http://www.w3.org/2000/svg'
width='500'
height='300'
viewBox='0 0 250 150'
preserveAspectRatio='xMinYMid meet'>
<!--svg content-->
</svg>
```

## 分组
- 使用`<g>`标签可以用来分组, 组内的元素会继承`g`标签上的属性, 并且`g`标签可以嵌套.

## 线性渐变`linearGradient`
- 属性
    - `x1`,`y1` 开始坐标位置 , 默认 0 0
    - `x2`,`y2` 结束坐标位置 , 默认 1 0
    - `gradientUnits`
        - `objectBoundingBox`(默认值) 定义x1,y1,x2,y2的坐标, 1,1 就是最终右下角的坐标位置.
        - `userSpaceOnUse` : x1,y1,x2,y2的坐标将会转换成使用世界坐标系的坐标.

```svg
<svg xmlns='http://www.w3.org/2000/svg' width='500' height='200'>
    <defs>
        <linearGradient id='grad' x1='0' y1='0' x2='1' y2='1'>
        	<stop offset='0%' stop-color='#0094ff'/>
        	<stop offset='50%' stop-color='green'/>
        	<stop offset='100%' stop-color='#ff0000'/>
        </linearGradient>
    </defs>
    <rect fill='url(#grad)' x='50' y='50' width='200' height='100'></rect>
</svg>
```
<svg xmlns='http://www.w3.org/2000/svg' width='500' height='200'>
    <defs>
        <linearGradient id='grad' x1='0' y1='0' x2='1' y2='1'>
        	<stop offset='0%' stop-color='#0094ff'/>
        	<stop offset='50%' stop-color='green'/>
        	<stop offset='100%' stop-color='#ff0000'/>
        </linearGradient>
    </defs>
    <rect fill='url(#grad)' x='50' y='50' width='200' height='100'></rect>
</svg>

```svg
<svg xmlns='http://www.w3.org/2000/svg' width='500' height='200'>
    <defs>
        <linearGradient id='grad1' gradientUnits='userSpaceOnUse' x1='0' y1='0' x2='200' y2='100'>
        	<stop offset='0%' stop-color='#0094ff'/>
        	<stop offset='50%' stop-color='green'/>
        	<stop offset='100%' stop-color='#ff0000'/>
        </linearGradient>
    </defs>
    <rect fill='url(#grad1)' x='50' y='50' width='200' height='100'></rect>
</svg>
```

<svg xmlns='http://www.w3.org/2000/svg' width='500' height='200'>
    <defs>
        <linearGradient id='grad1' gradientUnits='userSpaceOnUse' x1='0' y1='0' x2='200' y2='100'>
        	<stop offset='0%' stop-color='#0094ff'/>
        	<stop offset='50%' stop-color='green'/>
        	<stop offset='100%' stop-color='#ff0000'/>
        </linearGradient>
    </defs>
    <rect fill='url(#grad1)' x='50' y='50' width='200' height='100'></rect>
</svg>