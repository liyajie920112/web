## jQuery模块加载顺序
- window.onload 和 $(document).ready(fn) 的区别
    - `window.onload`是在DOM树加载完, 图片资源加载完, 外链的资源加载完之后才执行.
    - `$(document).ready(fn)` 只在dom树加载完之后才执行.

## 鼠标移入/移除事件区别
- `mouseenter`
    - 只有在鼠标指针穿过被选元素时，才会触发 mouseenter 事件。
- `mouseleave`
    - 只有移除被选元素的时候, 才会触发 mouseleave 事件.
- `mouseover`
    - 不论鼠标指针穿过被选元素或其子元素，都会触发 mouseover 事件。
- `mouseout`
    - 移除子元素和父元素的时候都会触发 mouseout 事件.

## 我没用过的方法

### `delay(duration,[queueName])` 
- 说明: 延迟动画执行
    - `duration`:(必填)延迟时间, 单位毫秒 ms
    - `queueName`:[可选] 动画队列名称
```js
<p id='foo'>测试</p>
$('#foo').delay(1000).fadeIn(1000);// 延迟1秒后执行fadeIn动画
```

### `stop([clearQueue],[jumpToEnd])` 
- 说明: 停止指定元素正在运行的动画
- 参数说明:
    - `clearQueue`;[可选], 默认false, false: 停止指定元素的正在执行的动画, true: 清空动画队列
    - `jumpToEnd`:[可选], 默认:false, false: 如果动画没有执行完, 不会跳到动画执行的属性值, true:直接跳过动画变成最后的属性值

### `addBack()`
- 说明: 

### `event.data`

```html
<div id='div'>我是div</div>
```
```js
$('#div').click({name:'liyajie'},function(e){
    console.log(e.data);// {name:'liyajie'}
})
```

### `event.result`

```html
<div id='div'>我是div event.result</div>
```

```js
$('#div').click(function(){
    return 123;
})
$('#div').click(function(event){
    console.log(event.result);// 123
})
```

### `trigger` 
- 说明: 手动触发自定义或者系统已经有的事件, 会有冒泡现象.

### `triggerHandler`
- 说明: 手动触发事件, 不会有冒泡现象.

