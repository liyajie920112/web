# JS兼容性相关

## Array.isArray() 方法的使用
> ES5的新特性, 需要做兼容性处理

```js
// 检查是否是数组对象
function checkIsArray(obj){
    if(Array.isArray){// 如果有这个属性
        return Array.isArray(obj);
    } else {
        return Object.prototype.toString.call(obj) == '[object Array]';
    }
}
```

## document.getElementsByClassName 的问题

对于 IE 我们使用兼容性处理

```js
// dom 就是我们要查找那个元素下的 class
// classN : 类名
function getElementByCN(dom, classN) {
// 如果有 dom.getElementsByClassName 则使用它
    if (dom.getElementsByClassName) {
        return dom.getElementsByClassName(classN);
    }
    var arr = [];
    var all = document.getElementsByTagName('*');
    for (var i = 0, len = all.length; i < len; i++) {
        var classArr = all[i].className.split(' ');
        var index = classArr.indexOf(classN);
        if(index != -1){
            arr.push(all[i]);
        continue;
        }
    }
    return arr;
}
```

## 获取浏览器宽度和高度

```js
// 获取浏览器宽度
var screenW = document.body.clientWidth || document.documentElement.clientWidth || window.innerWidth;
// 获取浏览器高度
var screenH = document.body.clientHeight || document.documentElement.clientHeight || window.innerHeight;

// 封装成方法
function client(){
    if(window.innerWidth) { // 如果是 IE浏览器
        return {
            width:window.innerWidth,
            height:window.innerHeight
        }
    } else if(document.compatMode == 'CSS1Compat'){ // 火狐和符合 w3c 的浏览器
        return {
            width:document.documentElement.clientWidth,
            height:document.documentElement.clientHeight
        }
    } else {
        return {
            width:document.body.clientWidth,
            height:document.body.clientHeight
        }
    }
}
```

## 获取浏览器的滚动

  2.1 `Chrome` 下获取滚动的距离使用`document.body.scrollTop`
  2.2 `IE9+`获取滚动距离使用`window.pageYOffset`
  2.3 火狐以及符合w3c标准的 使用`document.documentElement.scrollTop`

兼容性写法
```js
if(window.pageYOffset != null) {
  return {
    top:window.pageYOffset,
    left:window.pageXOffset
  }
} else if(document.compatMode == CSS1Compat) { // 说明是火狐或者符合w3c
  return {
      top:document.documentElement.scrollTop,
      left: document.documentElement.scrollLeft
    }
} else {
    return {
      top: document.body.scrollTop,
      left: document.body.scrollLeft
    }
}
```

## 事件中的 target 的问题

```js
btn.onclick = function(event){
    // 为了兼容 IE要使用下面的写法
    var event = event || window.event;
    if(event.target) {
        return event.target.id;
    } else { // ie下使用 srcElement
        return event.srcElement.id
    }
}
```

## 获取页内样式的兼容写法
```js
// obj: dom对象
// attr: 要获取的css属性
function getAttr(obj, attr) {
    if (obj.currentStyle) {
        return obj.currentStyle[attr];
    } else {
        return window.getComputedStyle(obj, null)[attr];
    }
}
```