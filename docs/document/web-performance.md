## eval

```js
var reference = {},props = 'p1'
// 使用eval
eval('reference.'+props+'=5')

// 不使用eval
reference[props] = 5
```

!> 性能比较： 使用eval比不使用eval的性能要慢100倍以上，因为JavaScript代码在执行前会有一个类似预编译的一个过程，首先会在当前上下文创建一个活动对象， 并将那些以var声明的变量设置为活动对象的属性，而这些属性的值都是`undefined`, 并将那些以function声明的函数也设置为活动对象的属性，属性值则是函数的定义。如果使用eval的话，eval中的代码是无法预知和识别其上下文的，无法被提前解析和优化，没有办法进行预编译，所以性能会大大降低。

## Function

```js
// 使用Function构造函数
var func1 = new Function(“return arguments[0] + arguments[1]”);
func1(10, 20); 
 
// 没有使用Function构造函数
var func2 = function(){ return arguments[0] + arguments[1] };
func2(10, 20);
```

!> 和eval类似，`func1`比`func2`的性能差很多， 推荐使用func2

## 缩小对象访问级别

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

## 避免隐式类型转换

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

## setTimeout和setInterval

!> setTimeout和setInterval中也可以传入字符串, 但是也会带来和eval一样的性能问题, 建议直接传入函数.