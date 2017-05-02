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
</style>
## 基本属性
### 设置到盒子上的属性
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

## align-items和align-content的区别

### align-items

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

### align-content

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

## 每个元素的属性

- `order` 默认 0
    - 数值越小排列越靠前

- `flex-grow` 默认 0
    - 定义元素的放大比例
    - 注意: 使用这个属性的时候, 宽度必须设置成auto或者不设置;

- `flex-shrink` 默认 1
    - 定义了元素缩小的比例, 当空间不足的时候, 将项目缩小. 如果设置成0 , 即时空间不足也不缩小该元素.

- `flex-basis`
    - 
- `flex`
- `align-self`


### order

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

### flex-grow

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

### flex-shrink

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

<div class='clear'></div>

## 一个元素的布局

### 盒子内元素水平居中

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

### 盒子内元素垂直居中

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

## 两个元素的布局

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

## 三个元素的布局

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

## 四个元素的布局

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


