## 插值语法闪烁的问题

1. 使用 `ng-cloak`
给 `ng-cloak` 添加样式
```css
[ng-cloak]{
    display:none;
}
```
2. 使用 `ng-bind` 绑定数据

## ng-src和src的区别

```html
<!--这种方式的在html解析的时候会把 {{url}} 当成一个图片地址去解析, 解析不到会报一个错误, 地址找不到-->
<img src='{{ url }}' />
<!--如果使用ng-src的话, html在解析的时候不会去加载ng-src, 而是在dom加载完的时候再由angular去解析的时候才会处理这个地址-->
<img ng-src='{{ url }}' />
```