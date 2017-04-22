## Ajax原生写法

```js
var xhr;
if(window.XMLHttpRequest){
    xhr = new XMLHttpRequest();
} else {
    xhr = new ActiveXObject('Microsoft.XMLHTTP');// 为了兼容IE5,6
}
// method: get/post
// url: 请求地址
// true: 是否是异步的, true: 异步, false: 同步
xhr.open('method','url',true);
xhr.send(null);// get请求的时候参数拼接到url后面, post请求的时候将参数放到send方法中
xhr.onreadystatechange = function(){
    if(xhr.readyState == 4){
        if(xhr.status >= 200 && xhr.status < 300 || xhr.status == 304){
            // 这里说明请求成功了
            // 这里执行成功回调
            console.log(xhr.responseText);// 打印服务器的响应信息.
        }
    }
}
```

## Ajax封装