## 状态管理

[本文参考链接地址](https://vuex.vuejs.org/zh-cn)

![](https://vuex.vuejs.org/zh-cn/images/vuex.png)

### 简单使用

> 引入js

```html
<script src="./vue.js"></script>
<script src="./vuex2.3.0.js"></script>
```

> 编写代码

```js
const store = new Vuex.Store({
    state: {
        count: 0
    },
    mutations:{
        increment(state){
            state.count++
        }
    }
})
// 我们只能通过commit来改变mutations
store.commit('increment') 
console.log(store.state.count) // 1
```

### 结合Vue使用

```js
const store = new Vuex.Store({
    state:{
        count: 0
    },
    mutations:{
        increment(state){
            state.count++
        }
    }
})
const app = new Vue({
    el:'#app',
    store, // 在根组件中注册store选项, 那么store会被注册到所有的子组件下面,那么我们就可以在组件中这样使用:this.$store.state.属性名
    components:{

    }
})
```

### Getters

getters可以认为是store中的计算属性

```js
const store = new Vuex.Store({
    state:{
        todos:[
            {name:'liyajie',age: 12, done: true},
            {name:'lihaojie',age: 24, done: false}
        ]
    },
    // getters可以认为是store中的计算属性
    getters:{
        doneTodos: state => {
            // 返回已经完成的todo
            return state.todos.filter(todo => todo.done)
        }
    }
})
// 在组件中使用

new Vue({
    el:'',
    computed:{
        doneTodos(){
            return this.$store.getters.doneTodos;
        }
    }
})
```

### mapGetters 用于映射getters中的状态

mapGetters仅仅是将store中的getters映射到局部属性中

```js
new Vue({
    el:'',
    computed:{
        ...Vuex.mapGetters({
            doneCount: 'doneTodos'
        })
    }
})
```

!> 上述代码中`...`表示对象展开运算符, 但是还处在ECMAScript的stage-3的阶段

### Mutations (中文转换/突变)

!> Mutations中的方法必须是同步方法

更改Vuex的store中状态的唯一方法就是提交`mutations`

> 提交没有参数的

```js
const store = new Vuex.Store({
    state:{
        count: 0
    },
    mutations:{
        // 一个mutation
        increment(state){
            // 变更状态
            state.count++
        }
    }
})
// 提交mutations
store.commit('increment')
console.log(store.state.count) // 1
```

> 提交有参数的mutations, 爱在Vuex中这叫[提交载荷payload]

```js
store.commit('increment',10)
```

> 提交有参数的mutations, 但是参数是个对象

方式一

```js
mutations:{
    increment(state,payload){
        state.count += payload.n
    }
}
// 使用
const obj = { n : 10 }
store.commit('increment',obj)
console.log(store.state.count) // 10
```

方式二

```js
mutations:{
    increment(state,payload){
        state.count += payload.n
    }
}
// 使用
store.commit({
    type:'increment',
    n: 10
})
console.log(store.state.count) // 10
```

### Actions

- Actions中的方法可以是异步方法
- Actions提交的是mutations, 而不是直接去修改状态


简单使用
```js
const store = new Vuex.Store({
    state:{
        count: 0
    },
    mutations:{
        increment(state){
            state.count++
        }
    },
    actions:{
        // 这里的context相当于store
        increment(context){
            // 提交mutation
            context.commit('increment')
        }
    }
})
// 我们可以使用ES6中的参数解构来简化代码, 修改actions中的代码
actions:{
    // { commit } 
    // 等价于 const { commit } = context; 
    // 等价于 const commit = context.commit;
    increment({ commit }){
        commit('increment')
    }
}
```

> store.dispatch触发Actions

```js
// 这种方式的优点是可以解决mutation必须同步的问题, 因为actions可以是异步的.
store.dispatch('actionName')

// 如
actions:{
    incrementAsync({ commit }){
        setTimeout(() => {
            commit('increment')
        },2000)
    }
}
```

> Actions同样支持载荷和对象的方式进行派发

```js
// 载荷的方式
store.dispatch('increment',{
    n: 12
})

// 对象的方式
store.dispatch({
    type: 'increment',
    n: 12
})
```

### Modules 模块

> 模块的简单声明

```js
const module1 = {
    state:{},
    mutations:{},
    actions:{}
}
const module2 = {
    state:{},
    mutations:{},
    actions:{}
}

const store = new Vuex.Store({
    modules:{
        a:module1,
        b:module2
    }
})

// 使用
store.state.a // a的状态
store.state.b // b的状态
```

### 模块的局部状态

actions中的第一个参数就是局部状态, 第二个参数就是commit, 全局的状态存放在了第三个参数`rootState`中了

!> 当两个模块中存在名称相同的actions的时候, 那么我们使用`store.commit('name')`的时候, 两个模块的actions会被同时触发

[参考链接地址](https://vuex.vuejs.org/zh-cn/modules.html)