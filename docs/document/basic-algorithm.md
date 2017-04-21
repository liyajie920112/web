## reverse a String
- 旋转一个字符串

```js
function reverseStr(str){
    return str.split('').reverse().join('');
}
```

## Factorialize a Number
- 计算某一个数的阶乘

```js
function factorialize(num){
    if(num > 1){
        return num * factorialize(num - 1);
    }
    return 1;
}
```

## Check for Palindromes
- 检查某个字是否是回文( 如: eye)
- 需要忽略空格, 标点符号, 大小写

```js
function palindrome(str) {
  var ascStr = str.toLowerCase().replace(/(\s+)|[,|.|_|(|)|//|\/|\-|:]/g,'');
  var descStr = ascStr.split('').reverse().join('');
  return ascStr == descStr;
}
```

## Find the Longest Word in a String
- 找出字符串中最长的单词, 并返回长度

```js
function findLongestWord(str){
    var arr = str.split(' ');
    return arr.reduce(function(v,o){
        if(v.length < o.length){
            v = o;
        }
        return v;
    }).length; 
}
```

## Title Case a Sentence
- 所有单词的首字母大写, 其余小写

```js
function titleCase(str) {
  var arr = str.split(' ');
  var r = '';
  for(var i = 0,len = arr.length; i < len ;i ++){
    var first = arr[i].charAt(0).toUpperCase();
    var last = arr[i].substr(1).toLowerCase();
    r += (first + last) + (i == len - 1 ? '':' ');
  }
  return r;
}
```

## Return Largest Numbers in Array
- 返回二维数组中的每个小数组中的最大的数

```js
function largestOfFour(arr) {
  // You can do this!
  for(var i = 0, len = arr.length; i < len ;i ++){
    arr[i] = arr[i].sort(function(a,b){
      return a < b;
    })[0];
  }
  return arr;
}

largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]);
```

## Confrim The Ending
- 判断是否是以某字符串结尾

```js
// str: 要判断的字符串
// target: 结尾字符串
function confirmEnding(str, target) {
  return str.substr(str.length - target.length,target.length) == target;
}

confirmEnding("He has to give me a new name", "me");
```

## Chunky Monkey
- 将数组按照规定的大小进行分割, 并将分割后的数组加入到新数组, 形成二维数组

```js
function chunk(arr, size) {
  // Break it up.
  var arr1 = [];
  var c = Math.ceil(arr.length / size);
  for(var i = 0; i < c; i++){
    arr1.push(arr.slice(i * size,size*(i+1)));
  }
  return arr1;
}

chunk(["a", "b", "c", "d",'e'], 2);// 打印出 [['a','b'],['c','d'],['e']]
```

## Caesars Cipher
- 凯撒密码, 又称: 移位密码
- 常见的案例就是ROT13, 字母会移动13个位置: A <-> N 交换, B <-> O 交换
示例

```js
rot13("SERR PBQR PNZC") 应该解码为 "FREE CODE CAMP"
```

解析:
```js
function rot13(str) { // LBH QVQ VG!
  var result = '';
  for(var i = 0, len = str.length; i< len; i++){
    var _str = str[i];
    var char = _str.charCodeAt();
    if(char >= 65 && char <= 90){
      // 78 说明是65 + 13, 如果小于78, 说明是0-13索引的字母, 只能+13才能取到后面的字母
      _str = String.fromCharCode(char < 78 ? char + 13 : char - 13);
    }
    result += _str;
  }
  return result;
}

// Change the inputs below to test
rot13("SERR PBQR PNZC");//FREE CODE CAMP
```