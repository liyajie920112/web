## Ajax原生写法

```js
function sendAjaxRequest(method,url,params){
    var xhr;
    if(window.XMLHttpRequest){
        xhr = new XMLHttpRequest();
    } else {
        xhr = new ActiveXObject('Microsoft.XMLHTTP');// 为了兼容IE5,6
    }
    // method: get/post
    // url: 请求地址
    // true: 是否是异步的, true: 异步, false: 同步
    var param = parseParams(params);
    method = method.toLowerCase();
    xhr.open(method,url + (method == 'get' ?'?'+param:''),true);
    // 如果是post请求需要设置请求头
    if(method == 'post'){
        xhr.setRequestHeader('Content-Type','application/x-www.form-urlencoded');
    }
    xhr.send(method == 'get'?null:param);// get请求的时候参数拼接到url后面, post请求的时候将参数放到send方法中
    xhr.onreadystatechange = function(){
        if(xhr.readyState == 4){
            if(xhr.status >= 200 && xhr.status < 300 || xhr.status == 304){
                // 这里说明请求成功了
                // 这里执行成功回调
                console.log(xhr.responseText);// 打印服务器的响应信息.
            }
        }
    }
}

function parseParams(params){
    var result = '';
    for(var key in params){
        result += key+'='+params[key]+'&';
    }
    return result + 'v='+randomNum();// 参数中加随机数是为了防止ie的缓存
}

// 生成随机数
function randomNum(){
    return Math.random() + (new Date()).getTime();
}
```

!> 注意: 如果是post请求, 需要设置请求头的`Content-Type:application/x-www-form-urlencoded`, 并且要设置在open之后 , send之前

## JSONP跨域请求
