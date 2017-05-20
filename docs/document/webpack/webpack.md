## webpack 

```bash
# 将hello.js 打包并输出到hello.bundle.js
webpack hello.js hello.bundle.js
```

## webpack的选项

### --module-bind
- 绑定loader, 用于加载模块

### --watch
- 时时监听文件改动, 时时编译

### --progress
- 显示编译打包的时候的进度条

### --display-modules
- 显示打包的模块

### --display-reasons
- 显示为什么打包这个模块的原因

### webpack 的配置config

```js
module.exports = {
    entry: [name], // 入口文件, 可以是对象, 使用多个入口文件
    output:{
        path:__dirname+'/dist/js',
        filename:'[name]-[chunkhash].bundle.js',
        publicPath:'http://www.liyajie.net/' // 上线地址
    },//插件
    plugins:[
        new htmlWebpackPlugin({
            filename:'index-[name].js',// 插入到html中的脚本的名称
            template:'index.html', // 模板文件, 上下文是当前项目根目录 /
            inject:'head', // 将生成的js插入到head, 默认插入到body底部
            title:'hello webpack'
        })
    ]
}
```
#### html页面中如果需要使用htmlWebpackPlugin中的选项参数

```html
<title><%= htmlWebpackPlugin.options.title %></title>
```

### 将js/css使用页内式嵌入到html中

```html
<script type="text/javascript">

    <% htmlWebpackPlugin.files.js.forEach(function(jsFile){ %>
      <%= compilation.assets[jsFile.substr(htmlWebpackPlugin.files.publicPath.length)].source() %>
    <% }) %>
  
</script>
```