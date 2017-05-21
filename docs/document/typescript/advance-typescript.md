## 类型别名
> 语法: `type 类型别名 = 类型`

?> 使用场景 : 常用于联合类型

```ts
type Str = string;
type StrFun = () => Str;
type StrType = Str | StrFun;
function getStr(v : StrType) : Str{
    if(typeof v === 'string'){
        return v;
    } else {
        return v();
    }
}
// exec
console.log(getStr('123'));// 123
console.log(getStr(e => {
    return 'lihaojie';
}));// lihaojie
```

## 字符串字面量类型

```ts
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele:Element,event:EventNames){
    //  do something ...
}
handleEvent(document.getElementById('box'),'click');// 正确
handleEvent(document.getElementById('box'),'dblclick');// 错误: 因为dblclick不在EventNames中
```

## 元组
    - 类似ES6中的解构赋值

> 语法 : `let person:[string,number] = ['liyajie',23]`

```ts
let person : [string,number] = ['liyajie',25];
// 获取元素
console.log(person[0]);// liyajie
console.log(person[1]);// 25
// 添加 元素
person.push('www.baidu.com');
// 添加的元素的类型默认是元组中每个类型的联合属性 string | number
console.log(person[3]);// www.baidu.com

person.push(false); // 报错: 因为false不是元组中类型的联合属性
```

## 枚举

```typescript
enum Days {
    Sun,
    Mon,
    Tue,
    Wed,
    Thu,
    Fir,
    Sat
};
// 等价于
enum Days {
    Sun = 0,
    Mon = 1,
    Tue = 2,
    Wed = 3,
    Thu = 4,
    Fir = 5,
    Sat = 6
};
console.log(Days.Mon); // 1
```
