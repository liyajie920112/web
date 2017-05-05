<style>
.box{
	width:99px;
	height:99px;
	background-color:#000;
    border:5px solid #0094ff;
    box-sizing:content-box;
    float:left;
    margin-left:10px;
    overflow:hidden;
    display:flex;
}
.box .item{
	background-color:#fff;
	width:33px;
	height:33px;
    border:5px solid #ddd;
    box-sizing:border-box;
    text-align:center;
}
.clear{
    clear:both;
}
.box1{
    justify-content:flex-start;
}
.box2{
    justify-content:center;
}
.box3{
    justify-content:flex-end;
}

.box4{
    align-items:flex-start;
}
.box5{
    align-items:center;
}
.box6{
    align-items:flex-end;
}
.box7{
    -webkit-justify-content:space-between;
}
.box8{
    justify-content:space-around;
}
.box9{
    justify-content:space-between;
    align-items:center;
}
.box10{
    justify-content:space-between;
    align-items:flex-end;
}
.box11{
    flex-direction:column;
    justify-content:space-between;
    align-items:center;
}
.box12{
    position:relative;
    flex-direction:column;
    justify-content:space-around;
    align-items:center;
}
.box13{
}
.box13 .item:nth-child(2){
    align-self:center;
}
.box14{
    justify-content:space-between;
}
.box14 .item:nth-child(2){
    align-self:flex-end;
}
.box15{
    justify-content:space-between;
}
.box15 .item:nth-child(2){
    align-self:center;
}
.box15 .item:nth-child(3){
    align-self:flex-end;
}
.box16{
    flex-wrap:wrap;
    justify-content:flex-end;
    align-content:space-between;
}
.box17{
    flex-wrap:wrap;
    align-content:space-between;
}
.box17 .column{
    display:flex;
    flex-basis:100%;
    justify-content:space-between;
}
.box00{
    flex-wrap:wrap;
    align-items:flex-start;
}
.box01{
    flex-wrap:wrap;
    align-items:center;
}
.box02{
    flex-wrap:wrap;
    align-items:flex-end;
}
.box03{
    flex-wrap:wrap;
    align-content:flex-start;
}
.box04{
    display:flex;
    flex-wrap:wrap;
    align-content:center;
}
.box05{
    flex-wrap:wrap;
    align-content:flex-end;
}
.box06{
    flex-wrap:wrap;
    align-content:space-between;
}
.box07{
    flex-wrap:wrap;
    align-content:space-around;
}
.box08{
    flex-wrap:wrap;
    align-content:stretch;
}
.box08 .item{
    height:auto;
}
.order{
    flex-wrap:wrap;
    justify-content:flex-start;
}
.order .item:nth-child(7){
    order:-1;
}
.flexgrow{
    display:flex;
    flex-wrap:wrap;
}
.flexgrow .item{
    width:auto;
}
.flexgrow .item:nth-child(1){
    flex-grow:1;
}
.flexgrow .item:nth-child(2){
    flex-grow:2;
}
.flexgrow .item:nth-child(3){
    flex-grow:1;
}
.flexshrink{
    width:150px;
}
.flexshrink .item:nth-child(2){
    flex-shrink:0;
}
.flexbasis .item:nth-child(2){
    flex-basis:50px;
}
.alignself{
    align-items:flex-start;
}
.alignself .item:last-child{
    align-self:center;
}
.box18{
    display:flex;
    flex-wrap:wrap;
    align-content:space-between;
}
.box19{
    display:flex;
    flex-wrap:wrap;
    flex-direction:column;
    align-content:space-between;
}
.box20{
    display:flex;
    flex-wrap:wrap;
}
.box20 .row{
    display:flex;
    flex-basis:100%;
}
.box20 .row:nth-child(2){
    justify-content:center;
}
.box20 .row:nth-child(3){
    justify-content:space-between;
}
.box21{
    display:flex;
    flex-wrap:wrap;
}
.grid{
    width:80%;
    height:50px;
    display:flex;
    border:1px solid #0094ff;
}
.grid .grid-cell{
    display:flex;
    justify-content:center;
    align-items:center;
    border:5px solid #ddd;
}
.grid1 .grid-cell{
    flex:1;
}
.grid .grid-cell.u1of2{
    flex: 0 0 50%;
}
.grid .grid-cell.u1of3{
    flex: 0 0 33.333333%;
}
.grid .grid-cell.u1of4{
    flex: 0 0 25%;
}
.grid2 .grid-cell{
    flex:1;
}
.grid3{
    height:300px;
    display:flex;
    flex-direction:column;
}
.grid3 .header,
.grid3 .footer{
    height:50px;
    background-color:#0094ff;
}
.grid3 .body{
    display:flex;
    flex-grow:1;
}
.grid3 .body .left,
.grid3 .body .right{
    width:100px;
    background-color:#f4f4f4;
}
.grid3 .body .center{
    flex-grow:1;
}
.inputaddon{
    border:1px solid #0094ff;
    margin-top:5px;
    width:250px;
    display:flex;
}
.inputaddon span{
    background-color:#0094ff;
    padding:5px;
    color:#fff;
}
.inputaddon input{
    flex-grow:1;
    outline:none;
    border:none;
}
.media{
    display:flex;
    align-items:flex-start;
    border:1px solid #0094ff;
}
.media p{
    margin:0;
    flex:1;
}
.fixedbottom{
    display:flex;
	flex-direction:column;
	height:300px;
	border:1px solid;
}
.fixedbottom header,.fixedbottom footer{
    height:50px;
	background-color:#0094ff;
}
.fixedbottom .ctent{
	flex:1;
}
.lbox{
    width:400px;
    height:400px;
    display:flex;
    flex-flow:row wrap;
    align-content:flex-start;
    background-color:#000;
}
.lbox .item{
    height:100px;
    flex:0 1 25%;
    background-color:#fff;
    border:2px solid #0094ff;

}
</style>
## 语法

### 适用于父容器的属性属性
- flex-direction
- `flex-wrap` 定义元素在轴线上放不下的时候如何换行
    - `nowarp(default)` 不换行
    - `warp` 换行, 第一行在上方
    - `warp-reverse` 换行, 第一行在下方

- flex-flow
- `justify-content` 主轴上元素的对齐方式
    - `flex-start(default)` 左对齐
    - `center` 居中对齐
    - `flex-end` 右对齐
    - `space-between` 两端对齐, 元素之间间隔都相等
    - `space-around` 每个项目两侧的间隔相等

- `align-items` 交叉轴上元素的对齐方式
    - `flex-start` 交叉轴起点对齐
    - `center` 交叉轴中点对齐
    - `flex-end` 交叉轴终点对齐
    - `baseline` 项目的第一行文字的基线对齐
    - `stretch(default)` 如果元素没有设置高度或者高度为auto, 则默认高撑满整个容器

- `align-content` 多根轴线的对齐方式, 只有一个轴线的时候无效
    - `flex-start` 交叉轴的起点对齐
    - `center` 交叉轴的中点对齐
    - `flex-end` 交叉轴的终点对齐
    - `space-between` 与交叉轴两端对齐, 轴线之间间隔平均分布
    - `space-around` 每根轴线两侧的间隔都相等
    - `stretch(default)` 轴线占满整个交叉轴 

### align-items和align-content的区别

#### align-items

<div class='box box00'>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box00{
    display:flex;
    flex-wrap:wrap;
    align-items:flex-start;
}
```

<div class='box box01'>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box01{
    display:flex;
    flex-wrap:wrap;
    align-items:center;
}
```

<div class='box box02'>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box02{
    display:flex;
    flex-wrap:wrap;
    align-items:flex-end;
}
```

#### align-content

<div class='box box03'>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box03{
    display:flex;
    flex-wrap:wrap;
    align-content:flex-start;
}
```

<div class='box box04'>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box04{
    display:flex;
    flex-wrap:wrap;
    align-content:center;
}
```

<div class='box box05'>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box05{
    display:flex;
    flex-wrap:wrap;
    align-content:flex-end;
}
```

<div class='box box06'>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box06{
    display:flex;
    flex-wrap:wrap;
    align-content:space-between;
}
```

<div class='box box07'>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box07{
    display:flex;
    flex-wrap:wrap;
    align-content:space-around;
}
```

<div class='box box08'>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box08{
    display:flex;
    flex-wrap:wrap;
    align-content:stretch;
}
.box08 .item{
    height:auto;
}
```

<div class='clear'></div>

### 适用于子项目的属性

- `order` 默认 0
    - 数值越小排列越靠前

- `flex-grow` 默认 0
    - 定义元素的放大比例
    - 注意: 使用这个属性的时候, 宽度必须设置成auto或者不设置;

- `flex-shrink` 默认 1
    - 定义了元素缩小的比例, 当空间不足的时候, 将项目缩小. 如果设置成0 , 即时空间不足也不缩小该元素.

- `flex-basis` 默认 auto, auto就是项目原有的宽, 可以是像素, 也可是百分比.
    - 定义了子项目占据的主轴空间, 

- `flex` 
    - 是flex-grow,flex-shrink,flex-basis 的简写, 默认: 0 1 auto , 后两个属性可选.
- `align-self` 默认值 auto, 其他取值 center | flex-start | flex-end | baseline | stretch
    - 定义子项目与其他项目不一样的对齐方式. 可覆盖align-items,  auto表示继承父元素的align-items, 如果没有父元素, 则等同于stretch


#### order

<div class='box order'>
    <div class='item'>1</div>
    <div class='item'>2</div>
    <div class='item'>3</div>
    <div class='item'>4</div>
    <div class='item'>5</div>
    <div class='item'>6</div>
    <div class='item'>7</div>
</div>

```css
.box .item:nth-child(7){
    order:-1;
}
```

#### flex-grow

<div class='box flexgrow'>
    <div class='item'>1</div>
    <div class='item'>2</div>
    <div class='item'>3</div>
</div>

```css
.flexgrow{
    display:flex;
    flex-wrap:wrap;
}
.flexgrow .item{
    width:auto;
}
.flexgrow .item:nth-child(1){
    flex-grow:1;/*放大一倍*/
}
.flexgrow .item:nth-child(2){
    flex-grow:2; /*放大两倍*/
}
.flexgrow .item:nth-child(3){
    flex-grow:1;/*放大一倍*/
}
```

#### flex-shrink

<div class='box flexshrink'>
    <div class='item'>1</div>
    <div class='item'>2</div>
    <div class='item'>3</div>
    <div class='item'>4</div>
    <div class='item'>5</div>
    <div class='item'>6</div>
</div>

```css
/*当空间不足的时候让第二个元素缩小2倍, 如果还不够, 则其他元素等比缩小*/
.box .item:nth-child(2){
    flex-shrink:2; 
}
/*如果flex-shrink:0; 0 的情况说明即时空间不足也不缩放这个元素*/
```

#### flex-basis

<div class='box flexbasis'>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box .item:nth-child(2){
    flex-basis:50px;
}
```

#### align-self

<div class='box alignself'>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.alignself:nth-child(2){
    align-self:center;
}
```


<div class='clear'></div>

## 不同个数项目的布局

### 一个项目-水平居中

```css
.box{
    display:flex;
    justify-content:center;
}
```
`justify-content`
- center
- flex-start
- flex-end

<div class='box box1'>
    <div class='item'></div>
</div>

```css
flex-direction:row;
justify-content:flex-start;
```

<div class='box box2'>
    <div class='item'></div>
</div>

```css
flex-direction:row;
justify-content:center;
```

<div class='box box3'>
    <div class='item'></div>
</div>

```css
flex-direction:row;
justify-content:flex-end;
```

<div class='clear'></div>

### 一个项目-垂直居中

```css
.box{
    display:flex;
    align-items:center;
}
```

`align-items`
- center
- flex-end
- flex-start

<div class='box box4'>
    <div class='item'></div>
</div>

```css
flex-direction:row;
align-items:flex-start;
```

<div class='box box5'>
    <div class='item'></div>
</div>

```css
flex-direction:row;
align-items:center;
```

<div class='box box6'>
    <div class='item'></div>
</div>

```css
flex-direction:row;
align-items:flex-end;
```

<div class='clear'></div>

### 两个元素的布局

```css
.box{
    display:flex;
    justify-content:space-between;
}
```

`justify-content`
- space-between
- space-around

<div class='box box7'>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
justify-content:space-between;
align-items:flex-start;
```

<div class='box box9'>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
justify-content:space-between;
align-items:center;
```

<div class='box box10'>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
justify-content:space-between;
align-items:flex-end;
```

<div class='box box8'>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
justify-content:space-around;
align-items:flex-start;
```

<div class='box box11'>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
flex-direction:column;
justify-content:space-between;
align-items:center;
```
<div class='box box12'>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
flex-direction:column;
justify-content:space-around;
align-items:center;
```

<div class='box box13'>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box13{
    display:flex;
}
.box13 .item:nth-child(2){
    align-self:center;
}
```

<div class='box box14'>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box14{
    display:flex;
    justify-content:space-between;
}
.box14 .item:nth-child(2){
    align-self:flex-end;
}
```
<div class='clear'></div>

### 三个项目

<div class='box box15'>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box15{
    display:flex;
    justify-content:space-between;
}
.box15 .item:nth-child(2){
    align-self:center;
}
.box15 .item:nth-child(3){
    align-self:flex-end;
}
```

### 四个项目

<div class='box box16'>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box16{
    display:flex;
    /*设置换行*/
    flex-wrap:wrap;
    /*设置主轴排版方式, 居右*/
    justify-content:flex-end;
    /*设置交叉轴排版方式*/
    align-content:space-between;
}
```

<div class='box box17'>
    <div class='column'>
        <div class='item'></div>
        <div class='item'></div>
    </div>
    <div class='column'>
        <div class='item'></div>
        <div class='item'></div>
    </div>
</div>

```css
/*方式一*/
.box{
    display:flex;
    /*设置主轴方向, 垂直方向*/
    flex-direction:column;
    /*设置主轴的对齐方式, 两端对齐*/
    justify-content:space-between;
}
.box .column{
    display:flex;
    /*设置column下的子项目的对齐方式, 两端对齐*/
    justify-content:space-between;
}
/*方式二*/
.box{
    display:flex;
    /*设置box的子项目换行*/
    flex-wrap:wrap;
    /*设置多个轴之间的布局方式, 两端对齐*/
    align-content:space-between;
}
.box .column{
    display:flex;
    /*让column占满主轴空间*/
    flex-basis:100%;
    /*让column中的项目两端对齐*/
    justify-content:space-between;
}
```

### 五个项目

<div class='box box20'>
    <div class='row'>
        <div class='item'></div>
        <div class='item'></div>
        <div class='item'></div>
    </div>
    <div class='row'>
        <div class='item'></div>
    </div>
    <div class='row'>
        <div class='item'></div>
        <div class='item'></div>
    </div>
</div>

```css
.box20{
    display:flex;
    flex-wrap:wrap;
}
.box20 .row{
    display:flex;
    flex-basis:100%;
}
.box20 .row:nth-child(2){
    justify-content:center;
}
.box20 .row:nth-child(3){
    justify-content:space-between;
}
```

<div class='clear'></div>

### 六个项目

<div class='box box18'>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box18{
    display:flex;
    flex-wrap:wrap;
    align-content:space-between;
}
```

<div class='box box19'>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box19{
    display:flex;
    flex-wrap:wrap;
    flex-direction:column;
    align-content:space-between;
}
```

<div class='clear'></div>

### 九个项目

<div class='box box21'>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
    <div class='item'></div>
</div>

```css
.box21{
    display:flex;
    flex-wrap:wrap;
}
```

<div class='clear'></div>

## 网格布局

### 基本的网格布局
- 就是平均分布, 容器里面的元素平均分配空间, 但是需要设置项目自动缩放(width:auto / height:auto);

<div class='grid grid1'>
    <div class='grid-cell'>1/4</div>
    <div class='grid-cell'>1/4</div>
    <div class='grid-cell'>1/4</div>
    <div class='grid-cell'>1/4</div>
</div>

```html
<div class='grid grid1'>
    <div class='grid-cell'>1/4</div>
    <div class='grid-cell'>1/4</div>
    <div class='grid-cell'>1/4</div>
    <div class='grid-cell'>1/4</div>
</div>
```

```css
.grid1{
    display:flex;
}
.grid1 .grid-cell{
    flex:1;
}
```

### 百分比布局
- 将网格宽度固定为百分比, 其他网格平均分配空间

<div class='grid grid2'>
    <div class='grid-cell u1of2'>1/2</div>
    <div class='grid-cell'>auto</div>
    <div class='grid-cell'>auto</div>
</div>

```html
<div class='grid grid2'>
    <div class='grid-cell u1of2'>1/2</div>
    <div class='grid-cell'>auto</div>
    <div class='grid-cell'>auto</div>
</div>
```
```css
.grid-cell{
    flex:1;
}
.grid-cell.u1of2{
    flex: 0 0 50%;
}
```

### 圣杯布局
- 这是一种很常见的网站布局, 分为上中下三个部分, 中间部分又分为左中右三个部分.

<div class='grid grid3'>
    <header class='header'>header</header>
    <section class='body'>
        <nav class='left'>left</nav>
        <section class='center'>center</section>
        <section class='right'>right</section>
    </section>
    <footer class='footer'>footer</footer>
</div>

```html
<div class='grid'>
    <header class='header'></header>
    <section class='body'>
        <nav class='left'></nav>
        <section class='center'></section>
        <section class='right'></section>
    </section>
    <footer class='footer'></footer>
</div>
```

```css
.grid{
    display:flex;
    flex-direction:column;
}
.grid .header,
.grid .footer{
    height:50px;
}
.grid .body{
    display:flex;
    flex-grow:1;
}
.grid .body .left,
.grid .body .right{
    width:100px;
}
.grid .body .center{
    flex-grow:1;
}
```

### 输入框布局
- 类似bootstrap的输入框组

<div class='inputaddon'>
    <span>用户名</span>
    <input type='text' />
</div>

<div class='inputaddon'>
    <span>邮箱</span>
    <input type='text' />
    <span>发送</span>
</div>

```css
.inputaddon{
    display:flex;
}
.inputaddon input{
    flex-grow:1;
}
```

### 悬挂式布局
- 在主栏的左侧或者右侧添加图片

<div class='media'>
    <img width='50' height='50' src='http://daohang.liyajie.net/baby.jpg' />
    <p>我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情我是详情</p>
</div>

```css
.media{
    display:flex;
    align-items:flex-start;
    border:1px solid;
}
.media p{
    margin:0;
    flex:1;
}
```

### 固定在底栏
- 当内容不足的时候, 底栏仍然在下面, 足的时候撑下去, 

<div class='fixedbottom'>
    <header>header</header>
    <div class='ctent'>
        <p>测试内容测试内容测试内容测试内容测试内容测试内容测试内容测试内容测试内容测试内容</p>
        <p>测试内容测试内容测试内容测试内容测试内容测试内容测试内容测试内容测试内容测试内容</p>
    </div>
    <footer>footer</footer>
</div>

```css
.fixedbottom{
    display:flex;
	flex-direction:column;
	height:300px;
	border:1px solid;
}
.fixedbottom header,.fixedbottom footer{
    height:50px;
	background-color:#0094ff;
}
.fixedbottom .ctent{
	flex:1;
}
```

### 流式布局
- 类似九宫格的布局

<div class='lbox'>
    <div class="item"></div>
	<div class="item"></div>
	<div class="item"></div>
	<div class="item"></div>
	<div class="item"></div>
	<div class="item"></div>
	<div class="item"></div>
	<div class="item"></div>
	<div class="item"></div>
</div>

```css
.lbox{
    width:400px;
    height:400px;
    display:flex;
    flex-flow:row wrap;
    align-content:flex-start;
    background-color:#000;
}
.lbox .item{
    height:100px;
    flex:0 1 25%;
    background-color:#fff;
    border:2px solid #0094ff;

}
```