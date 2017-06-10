## js性能调优

### eval

```js
var reference = {},props = 'p1'
// 使用eval
eval('reference.'+props+'=5')

// 不使用eval
reference[props] = 5
```

!> 性能比较： 使用eval比不使用eval的性能要慢100倍以上，因为JavaScript代码在执行前会有一个类似预编译的一个过程，首先会在当前上下文创建一个活动对象， 并将那些以var声明的变量设置为活动对象的属性，而这些属性的值都是`undefined`, 并将那些以function声明的函数也设置为活动对象的属性，属性值则是函数的定义。如果使用eval的话，eval中的代码是无法预知和识别其上下文的，无法被提前解析和优化，没有办法进行预编译，所以性能会大大降低。

### Function

```js
// 使用Function构造函数
var func1 = new Function(“return arguments[0] + arguments[1]”);
func1(10, 20); 
 
// 没有使用Function构造函数
var func2 = function(){ return arguments[0] + arguments[1] };
func2(10, 20);
```

!> 和eval类似，`func1`比`func2`的性能差很多， 推荐使用func2

### 缩小对象访问级别

通俗的讲就是尽量使用局部变量， 因为局部变量层级最浅所以最先找到，全局变量层级较深查找效率会有所降低

```js
str += 'str1' + 'str2';
```
!> 上面这种方式会有一些临时变量的创建与销毁所以影响性能

```js
var strarr = [];
strarr.push('str1')
strarr.push('str2')
str = strarr.join('')
```

利用数组的push和join进行字符串拼接, 性能有所优化

!> 最新的浏览器(FF3+,IE8+)对`+=`做了性能优化, 我们也可以这样写,优化后性能比数组拼接的方式要快

```js
str += 'str1'
str += 'str2'
```

### 避免隐式类型转换

```js
var str = '12345',arr=[]
for(var i = 0; i < str.length; i++){
    arr.push(str.chatAt(i))
}
```
!> 上面代码存在的问题是有隐式类型转换, 因为str是一个字符串常量, 并不是一个字符串对象, 所以在调用`str.charAt(i)`的时候会去创建一个值为`12345`的临时的字符串对象, 然后再调用`chatAt`方法, 最后在释放这个临时对象,这个创建和销毁的过程是很耗性能的.

> 改进后

```js
var str = new String('12345'),arr = []
for(var i = 0, len = str.length; i < len; i++){
    arr.push(str.charAt(i))
}
```

?> 我们在开始的时候就先把字符串对象创建好, 这样就没有隐式类型转换的, 性能就会有所提升.

### setTimeout和setInterval

!> setTimeout和setInterval中也可以传入字符串, 但是也会带来和eval一样的性能问题, 建议直接传入函数.

## dom性能调优

### Repaint和Reflow
- Repaint也就Redraw, 重绘是不影响页面布局和dom结构的一种重绘操作, 如下动作会产生Repaint操作

1. 不可见到可见(visibility属性)
2. 颜色或者图片改变(background,color,border-color样式属性)
3. 不改变元素外观大小,形状和位置, 但改变其外观的变化

- Reflow和Repaint比起来会有更显著的变化, 主要发生在dom树被操作的时候,任何改变dom结构和布局都会产生Reflow, 但一个元素的Reflow发生的时候, 他所有的父元素和子元素都会放生Reflow, 最后Reflow必然会导致Redraw的发生

1. 浏览器窗口大小发生变化
2. Dom节点的添加删除操作
3. 一些页面上元素的大小,位置,形状发生变化的操作

### 减少Reflow(重排)

```js
var pDiv = document.createElement('div')
document.body.appendChild(pDiv) // -----reflow
var div1 = document.createElement('div')
var div2 = document.createElement('div')
pDiv.appendChild(div1) // -----reflow
pDiv.appendChild(div2) // -----reflow
```

优化方式一

```js
var pDiv = document.createElement('div')
var div1 = document.createElement('div')
var div2 = document.createElement('div')
pDiv.appendChild(div1)
pDiv.appendChild(div2)
document.body.appendChild(pDiv) // -----reflow
```

优化方式二

```js
var divp = document.getElementById('one')
divp.style.display = 'none'
divp.appendChild(div1)
divp.appendChild(div2)
divp.appendChild(div3)
divp.appendChild(div4)
divp.appendChild(div5)
divp.style.width = '100px'
divp.style.height = '100px'
divp.style.display = 'block'
```

?> 先隐藏在显示,在隐藏的时候做的操作是不会引起`reflow`的, 提高了效率

### 特殊测量和方法

Dom中有些特殊的测量属性和方法的调用也会触发`reflow`, 比较典型的就是`offsetWidth`和`getComputedStyle`

```js
var dom = document.getElementById('test')
var ow = dom.offsetWidth; // ----- reflow
var sl = dom.scrollLeft; // ----- reflow
var display = window.getComputedStyle(div,'').getPropertyValue('display') // ----- reflow
```

> 测量属性和方法大致有

- offsetLeft/Top/Width/Height
- scrollTop/Left/Width/Height
- clientTop/Left/Width/Height
- getComputedStyle
- currentStyle()

?> 我们应该减少对这些属性和方法的访问和调用

### 优化测量属性和方法
- 我们应该把测量属性和方法的结果尽量使用变量缓存起来

```js
var dom = document.getElementById('div')
var ow = dom.offsetWidth;
div1.style.width = ow + 'px';
div2.style.width = ow + 'px';
div3.style.width = ow + 'px';
```

### 样式相关

```js
var dom = document.getElementById('div')
dom.style.borderColor = '1px solid red'
dom.style.backgroundColor = 'yellow'
dom.style.padding = '10px'
dom.style.margin = '10px'
```
!> 上面的每个操作都会引起`reflow`减少这种情况我们可以这样做

> 使用className优化到只重排/重绘一次

```js
.style {
    border-color:1px solid red;
    background-color:yellow;
    padding:10px;
    margin:10px;
}
var dom = document.getElementById('div')
dom.style.className = 'style' // -----reflow
```

> 使用cssText优化, 一次性设置所有样式, 只重排/重绘一次

```js
var dom = document.getElementById('div')
var style = 'border-color:1px solid red;'+'color:red;'+'background-color:yellow;'+'padding:10px;'+'margin:10px;';
dom.style.cssText += style;
```

### 动态创建script标签

加载并执行一段 JavaScript 脚本是需要一定时间的，在我们的程序中，有时候有些 JavaScript 脚本被加载后基本没有被使用过 （比如：脚本里的函数从来没有被调用等等）。加载这些脚本只会占用 CPU 时间和增加内存消耗，降低 Web 应用的性能。所以推荐动态的加载 JavaScript 脚本文件，尤其是那些内容较多，消耗资源较大的脚本文件。

- 向技术人员问
    - 我们公司的技术路线
    - 项目组中有多少人
- 人事
    - 工资的薪水是税前还是税后
    