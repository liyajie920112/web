## Vue知识点总结

### Vue实例

```js
new Vue({
    el:'挂在对象(选择器)',
    data:{
        /*...*/
    },
    beforeCreate(){

    },
    created(){
        console.log('created')
    },
    beforeMount(){
        console.log('beforeMount')
    },
    mounted(){
        console.log('mounted')
    },
    beforeUpdate(){
        console.log('beforeUpdate')
    },
    update(){
        console.log('update')
    },
    beforeDestroy(){
        console.log('beforeDestroy')
    },
    destroy(){
        console.log('destroy')
    }
})
```

### 注册全局组件

```js
Vue.component('my-component',{
    template:'<h1>我是组件</h1>'
})

// 使用
<my-component></my-component>
```

### 局部组件

```js
new Vue({
    el:'',
    components:{
        'my-component':{
            // 组件中的这个data必须是一个函数, 因为如果不是一个函数,那么里面的属性是被这个组件共享的.
            data(){
                return {

                }
            },
            template:'<h1>我是局部组件</h1>'
        }
    }
})
```

### 父子组件

```js
new Vue({
    el:'',
    components:{
        'father-component':{
            data(){
                return {

                }
            },
            // 子组件只能在父组件的模板中使用
            template:'<h1>father<child-component></child-component></h1>',
            components:{
                'child-component':{
                    data(){

                    },
                    template:'<h1>child</h1>'
                }
            }
        }
    }
})
```

### 父子组件通信

> 父组件传递数据到子组件, 子组件使用`props`接收

```js
new Vue({
    el:'',
    components:{
        'father-component':{
            data(){
                return{
                    name:'liyajie'
                }
            },
            template:'<h1>father <child-component :name="name"></child-component></h1>',
            components:{
                'child-component':{
                    props:{
                        name:{
                            type:String
                        }
                    },
                    template:'<h1>{{name}}</h1>'
                }
            }
        }
    }
})
```

> 子组件传递数据到父组件, 使用自定义事件

```js
new Vue({
    el:'',
    components:{
        fatherCom:{
            methods:{
                fatherEvent(arg){
                    console.log('父组件事件调用了--'+arg)
                }
            },
            template:'<div>father <child-com :test="fatherEvent"></child-com></div>',
            components:{
                methods:{
                    childEvent(){
                        console.log('子组件事件调用了')
                        this.$emit('test','参数')// 触发父组件的事件
                    }
                },
                template:'<div>child <button @click="childEvent">点击通信</button></div>'
            }
        }
    }
})
```