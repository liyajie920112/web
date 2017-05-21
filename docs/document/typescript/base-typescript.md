## 变量

> 语法
```ts
let 变量名 : 变量类型 = 变量值;
```

### 声明基本类型的变量
```ts
// 声明一个number类型的变量
let num : number = 1;

// 声明一个string类型的变量
let str : string = 'liyajie';

// 声明一个boolean类型的变量
let b : boolean = false;
let bb : boolean = Boolean(1);

// 声明undefined null类型的值
let a : undefined;
let aa : null;
```

!>  `let bbb : boolean = new Boolean(1);`这种方式是不允许的, 因为`new Boolean(1)`返回的是一个对象, 并不是boolean

### 声明任意值的变量

```ts
// 可以赋值任意类型的值
let anyVar : any = 1;
anyVar = 'liyajie';
anyVar = true;
anyVar = 1;
anyVar = undefined;
anyVar = null;

// 没有指定类型的变量默认解析成任意类型
let aa ; // 等价于: let aa : any;
```

### 类型推断

```ts
let str = 'liyajie';
str = 1;
// 声明代码会报错, 因为类型不一致

// 等价于

let str : string = 'liyajie';
str = 1;
```

### 联合类型

```ts
let a : string | number;
a = 'liyajie'; // 这里被推断成了 string
a = 1; // 这里被推断成了 number

a = false; // 报错, 因为a只能是string或者number类型
// 访问联合类型的属性
a.toString();// 没问题
console.log(a.length);// 报错: 因为number没有length属性
```

!> 访问联合类型的属性的时候必须是类型通有的属性才可以.

## 对象类型接口

> 对类的一部分行为进行抽象, 也常用于对[对象的形状]进行描述

?> 接口中的属性必须声明, 可选属性可以不用声明, 如果没有任意属性, 则不可以定义接口中没有声明的属性, 少定义接口中的属性也是不可以的.

```ts
// 声明一个接口
interface Person{
    readonly id: number; // 定义只读属性
    name: string; // 定义可读可写的属性
    age: number; // 定义number的属性
    sex?: boolean; // 定义可选的属性
    [propName:string] : any;// 定义任意属性
}

// 使用接口
let Student : Person = {
    id:1,
    name:'liyajie',
    age: 23,
    sex:true, // 可以声明 , 也可没有
    url:'http://www.liyajie.net/' // 任意属性
}

Studeng.id = 2;// 语法报错: 因为id是只读的.
```

## 数组的类型

### [类型 + 方括号]

定义一个number类型的数组

```ts
let numArr : number[] = [1,2,3];
// number数组中只可以有number类型的值,不可以有其他类型的值
let numberArr : number[] = [1,2,3,false]; // 报错
```

### 泛型表示法

```ts
let arr : Array<number> = [1,2,3];
```

### 接口表示法

只要index的类型是number, 那么值的类型必须是string

```ts
interface StringArr{
    [index:number] : string
}
let strArr : StringArr = ['1','2'];
```

### 任意类型的数组

允许数组中出现任意类型的值
```ts
let anyArr:any[] = [1,'str',false,undefined,null,{name:'liyajie'}];
```

### 类数组

```ts
function test() : void{
    let arr: number[] = arguments;
}
// 报错消息如下, 因为arguments是个类数组, 它的接口是IArguments, 我们应该这样定义

function test(): void {
    let arr: IArguments = arguments;
}
```

!> error TS2322: Type 'IArguments' is not assignable to type 'number[]'.
  Property 'push' is missing in type 'IArguments'.

## 函数的类型

### 函数声明

javascript中函数声明的常用方式有: 函数声明 和 函数表达式

```js
// 函数声明
function sum(a,b){
    return a + b;
}

// 函数表达式
var sum = function(a,b){
    return a + b;
}
```

typescript中声明函数的时候对函数的返回值, 形参的类型进行了约束

> 语法: `function 函数名(参数1: 类型, 参数2: 类型...) : 返回值类型 {...}`

```ts
function sum(a: number, b: number) : number{
    return a + b;
}
```

> 调用

```ts
sum(1,2); // return: 3
sum(1,2,3);// 多余的参数是不允许的
sum(1);// 少于声明的参数也是不允许的
```

如果我们需要多传或者少传参数, 我们可以在生命函数形参的时候使用可选参数

```ts
function sum(a:number,b?: number): number{
    return a + (b || 0); // 如果没有给 b 传值, 则给 0 
}
// 调用
console.log(sum(12)); // result: 12
```

!> 可选参数必须放到必选参数后面

### 表达式声明

```ts
let sum = function(a: number, b?: number) :number{
    return a + b;
}
```
这种写法sum的类型实际上是通过右边的表达式推测出来的, 如果我们需要手动个sum指定类型,可以这样写

```ts
let sum: (a: number, b: number) = number => function(a: number, b: number): number{
    return a + b;
}
```

### 接口中函数的定义

```ts
// 定义函数的形状
interface sum{
    // sum类型的函数必须遵守下面这种格式(形状)
    (a: number, b: number) : number;// 返回值是 number 类型的
}
let mySum : sum = function(a:number,b: number) {
    return a + b;
}
```

### 可选参数

```ts
function sum(a : number,b? : number) : number{
    return a + (b || 0);
}
```

### 参数默认值

?> 参数默认值不受顺序影响

```ts
function sum(a : number = 2, b: number):number{
    return a + b;
}
```
编译成js

```js
function sum(a,b){
    if(a === void 0) { a = 2; }
    return a + b;
}
```

### 剩余参数

```ts
function sum(a: number,b:number,...rest):void{
    console.log(a,b,rest);// rest是个数组
}
// 调用
sum(1,3,4,5,6);// 1,3,[4,5,6]
```
编译成js

```js
function sum(a,b){
    var rest = [];
    for (var _i = 2; _i < arguments.length; _i++) {
        rest[_i - 2] = arguments[_i];
    }
    console.log(a, b, rest);
}
```

### 函数重载

> 需求: 当我们输入number类型的 123, 输出: 321, 输入string类型 'liyajie' 输出: 'eijayil'

采用联合类型的参数
```ts
function reverse(v: string | number) : string | number{
    if(typeof v === 'number'){
        return Number(v.toString().split('').reverse().join(''));
    } else if(typeof v === 'string') {
        return v.toString().split('').reverse().join('');
    }
}
// test
console.log(reverse(234)); // 432
console.log(reverse('liyajie')); // eijayil
```
!> 上面这种方式的缺点: 不能精确表达, 当输入为数字的时候,输出应该也是数字, 输入字符串的时候, 输出也应该为字符串

```ts
function reverse(x: number): number;// 定义重载
function reverse(x: string): string;// 定义重载
function reverse(x: number | string): number | string { // 定义实现
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.toString().split('').reverse().join('');
    }
}
```
前两个是定义重载, 最后一个是实现.typescript会优先匹配前两个.

## 类型断言

> 绕过编译器的类型推断, 我们自己指定类型

### 语法
    - `<类型> 值` or `值 as 类型`

```ts
// 将v断言成string类型
function strLength(v:number | string) : number{
    if((<string>v).length){
        return (<string>v).length;
    } else {
        return v.toString().length;
    }
}
```

```ts
function strLength(v:number | string) : number{
    return <boolean>v; // 报错: 因为boolean不在number 和 string中
}
```

!> 断言成联合类型中不存在的类型是不可以的

## 声明文件
- 声明文件的作用是在我们引用第三方库的时候帮我们检测语法和类型的.

> 语法: `declare var jQuery:(string) => any`

example
```ts
declare var jQuery:(string) => any;
jQuery('#box');
```

通常我们会将这种声明文件写到一个文件中, 使用`<reference path="./jquery.d.ts" />`

jquery的声明文件
[https://github.com/DefinitelyTyped/DefinitelyTyped/edit/master/types/jquery/index.d.ts](https://github.com/DefinitelyTyped/DefinitelyTyped/edit/master/types/jquery/index.d.ts)

## 内置对象
- typescript直接定义好的类型

### ECMAScript 的内置对象
`Boolean` `Date` `Error` `RegExp`等

typescript中定义这些变量

```ts
let a : Boolean  = new Boolean(1);
let b : Error = new Error('I am Error');
let c : Date = new Date();
let d : RegExp = /[a-z]/;
```

而他们的定义文件，则在 [TypeScript 核心库](https://github.com/Microsoft/TypeScript/tree/master/src/lib)的定义文件中。

### BOM和DOM的内置对象
`Document` `HTMLElement` `Event` `NodeList`

```ts
let body : HTMLElement = document.body;

let allDiv : NodeList = document.querySelector('div');

document.addEventListener('click',function(e: MouseEvent){
    // do something...
});
```

### TypeScript的核心定义文件
- 该文件定义了所有浏览器环境需要用到的类型

example

```ts
// 计算10的平方
Math.pow(10,'2'); // 报错, 因为Math.pow()的定义中定义了参数的类型是number

// 正确的写法
Math.pow(10,2);// 100
```

Math.pow();的定义

```ts
interface Math {
  /**
   * Returns the value of a base expression taken to a specified power.
   * @param x The base value of the expression.
   * @param y The exponent value of the expression.
   */
  pow(x: number, y: number): number;
}
```

### TypeScript写Nodejs
- 需要引用第三方的定义 

```bash
npm install @types/node --save-dev
```