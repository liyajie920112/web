## Vue知识点总结

### Vue实例

```js
new Vue({/*...*/});
```

### 模板语法

#### 插值
- 文本
    - {{ msg }}
    - v-once
    - v-html
    - v-text

- 属性
    - v-bind:id (简写可以省略v-bind, 直接使用:)

- 使用javascript表达式

```html
{{ number + 1}}
{{ Ok ? 'Yes' : 'No'}}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-'+id"></div>
```
下面例子不会生效
```html
<!--这是语句不是表达式-->
{{ var a = 1 }}
<!--流控制也不会生效, 请使用三元表达式-->
{{ if (ok) { return msg } }}
```

