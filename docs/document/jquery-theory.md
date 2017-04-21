# jQuery原理

?> 基于jQuery 3.2.1版本分析

## 个版本之间的区别
- 1.x 版本 兼容IE6,7,8 做了很多兼容判断, 不利于核心代码
- 2.x 版本 不再兼容IE6,7,8
- 3.x 版本 新增了特性, 支持了模块化

## 为什么是闭包结构
- 避免和其他框架发生冲突, 如: 变量名
- 通过给window添加属性和方法让外界访问到背部的变量和方法

## window 以参数传入
- 提高查找效率
- 便于压缩

## undefined 以参数传入
- 防止外界修改`undefined`, IE9以下的undefined是可以修改的
- 便于压缩

## $ 是什么
- 内部实现: window.jQuery = window.$ = jQuery  10242行

## jQuery 变量是什么
- 提供给外界一个工厂方法, 便于创建jQuery对象.

```js
// 94行
jQuery = function(selector, context) {
        // The jQuery object is actually just the init constructor 'enhanced'
        // Need init if jQuery is called (just allow error to be thrown if not included)
        return new jQuery.fn.init(selector, context);
    },
```

## jQuery.fn 是什么
- jQuery.fn 就是jQuery.prototype

源码

```js
// 114行
// 替换掉jQuery原型为自定义的对象
jQuery.fn = jQuery.prototype = {...};
```

## 小结一
- jQuery其实是一个函数, 就是内部创建的init函数
- jQuery对象其实就是由内部 init函数 创建的对象.

```js
// 94行
jQuery = function(selector, context) {
    // The jQuery object is actually just the init constructor 'enhanced'
    // Need init if jQuery is called (just allow error to be thrown if not included)
    return new jQuery.fn.init(selector, context);
},
```

## jQuery为什么要用一个init方法来创建jQuery对象并返回, 而不是直接返回自己
1. 为了初始化一些参数.
2. 为了简单. 外部调用方便, 外部不用通过`new`来调用(new jQuery())

```js
function Dog(){
    return new Dog.prototype.init();
}
Dog.prototype = {
    constructor:Dog,
    init:function(){
        this.name = 'wangcai',
        this.age = 3
    },
    say:function(){
        console.log('我叫: '+this.name);
    }
}
// 因为Dog返回的对象是init创建的对象, 所以我们需要把init的原型重新指向Dog.prototype
// 这样创建出来的对象才能有Dog.prototype上的方法和属性
Dog.prototype.init.prototype = Dog.prototype;
// 调用say方法
var dog = new Dog();
dog.say();// 打印出: 我叫: wangcai
Dog().say();// 打印出: 我叫: wangcai  
```

!> 如果我们没有在Dog的构造函数中调用`new Dog.prototpye.init()` 那么我们在下面调用`say`方法之前需要先调用下`init`, 这样`this.name`才有值, 所以这样写: 1. 是为了初始化一些参数. 2. 是为了调用简单.(可以不用创建对象直接以方法的形式进行调用, 因为返回的是一个引用类型的对象, 所以会覆盖原来的对象.)

!> 如果没有`Dog.prototype.init.prototype = Dog.prototype;` 那么`say`方法只会在`Dog.prototype`上, 然而现在返回的是`init`的实例,所以`say`没有在这个实例上, 所以不会调用成功.

## jQuery可以接收的参数类型
- '',null,unefined,NaN,false,0
- 选择器
- html片段
- dom对象
- 数组
- 伪数组
- 对象
- 基本数据类型

## 传入null,undefined,NaN,0,false的时候返回的值

```js
console.log($(''));
console.log($(null));
console.log($(undefined));
console.log($(NaN));
console.log($(0));
console.log($(false));
// 上面返回的结果统一是一个空的jQuery对象

console.log(!'');
console.log(!null);
console.log(!undefined);
console.log(!NaN);
console.log(!0);
console.log(!false);
// 上面返回的结果统一是 true
```

## 如果传入的是普通的值

```js
$(1);
$('abc');
$(true)
// 上面打印的结果统一是 jQuery对象中只有一个元素就是传入的值
```

## 传入的是html片段

```js
$('<div><span>span1</span>div1</div><span>span2</span>');
// 打印出的结果是: jQuery对象中有两个对象, 第一个是div, 第二个是span
```
处理方式
- 先去掉手尾空格
- 判断是否以 < 开头, > 结尾, 并且长度 >3, 因为最小标签的长度就是3
- 处理完之后, 创建一个临时标签, 将这个html片段添加到临时标签中
- 然后获取到临时标签中的children, 将其遍历添加到jQuery对象上去.(借用数组的push方法 [].push.apply(this,children))

## 传入的是选择器
- 方式一: 使用querySelectorAll(selector);
- 方式二: 使用第三方框架: sizzle.js, jQuery内部就是使用的它.

处理方式:
- 将选择出来的元素进行遍历, 添加到jQuery对象上去.

## 传入的是对象(Object)
- 判断传入的是否是对象
- 并且这个对象不是数组, 也不是伪数组.
- 伪数组就是有`length`这个属性, 并且有`length`的值-1 这个属性的对象, 如: { 0: 'abc',1:'123',length:2 }

```js
this[0] = selector;
this.length = 1;
```

## 传入的是数组(Array)
- 通过`[object Array]`来判断是否是数组.
- 是数组的话就遍历数组的元素, 将每个元素的值按照顺序以属性值的方式添加到jQuery对象上.

## 传入的是伪数组
- 伪数组和真数组之间的区别是类型不一样
- 如果是伪数组的话需要通过for循环便利这个对象, 如果没有某个属性, 则该处的值为`undefined`

```js
// 创建一个伪数组
var obj = {
    1:'abc',
    length:2
}
// 将伪数组传入jQuery对象
$(obj);
// 内部实现
var i = 0;
var len = obj.length;
for(; i < len; i++){
    this[i] = obj[i];
}
```
!> 上面这种情况, 如果`obj[0]`没有这个值的话, 则返回`undefined`

## 封装静态方法
- 封装静态方法使用起来方便

## 扩展方法
- 将`extend`方法添加到jQuery构造函数上, 内部

```js
jQuery.extend = jQuery.fn.extend = function(){
    // 这里处理我们自己扩展和jQuery内部扩展的方法

};
```
内部实现原理
- 扩展静态方法:
    - 遍历传入的对象, 将对象的所有成员遍历添加到jQuery的构造函数上, 这样扩展的方法就是静态方法了, 这样直接使用jQuery来直接调用.
- 扩展示例方法:
    - 如果我们向扩展实例方法, 我们可以使用`jQuery.fn.test = function(){...}`, 这样实例上就有了test这个属性了.

- 有了扩展方法, 我们就能很容易的给jQuery编写我们需要的插件了.