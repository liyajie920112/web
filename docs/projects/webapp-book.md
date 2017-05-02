# 书城项目

## viewport布局

- viewport: 视口
- width=device-width 内容宽度设置成设备宽度
- minimum-scale: 最小缩放比
- maximum-scale: 最大缩放比
- user-scalable: 是否可以通过手指缩放

```html
<meta name="viewport" content="width=device-width,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no">
```

## html5 ajax跨域

- 需要在请求的服务器中设置响应头: header('Access-Control-Allow-Origin:*'); 这是php中设置头的方式, * 代表所有的域都可以访问这个域, 我们也可以设置 www.baidu.com  , 这样就只有百度可以进行跨域请求了.

## 移动端的性能陷阱和硬件加速
- 减少或避免: repaint(页面重绘), reflow(页面回流)
    - 当某个元素的位置没有变化, 颜色发生了变化的时候, 浏览器就会进行页面重绘
    - 当某个元素的位置发生变化的是, 就会产生页面回流, 回流消耗的性能更高.
    - 就是减少对dom元素的操作, 操作的时候我们可以先把元素在dom流中提取出来再操作.
- 尽量缓存所有可以缓存的数据
- 尽量使用CSS3的transform 代替dom操作, 因为css3中的transform会有硬件加速.

- 不要给非static的元素添加CSS3动画, 这样浏览器的性能会成倍开销.
- [适当]使用硬件加速, 用手机的GPU来渲染, 如: 使用canvas渲染图片, 一定要适当.
- 如果我们想让某个元素进行硬件加速, 我们可以给它设置`transform:translate3d(0,0,0);` 即可.

## 技术选型(移动端)
- 原则
    - 轻量化
    - 维护简单
    - 快速开发
    - 高性能