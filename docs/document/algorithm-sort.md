## 选择排序 O(n^2)级别

> 思路: 1. 找到数组中最小的元素放到索引0的位置, 也就是将找到的元素和第一个元素进行交换, 2. 找到剩余元素中最小的元素, 放入索引1的位置, 将找到的元素与索引1的元素进行交换. 以此类推

```js
function swap(arr, index1, index2) {
  var temp = arr[index1]; // 第一个位置的值
  arr[index1] = arr[index2];
  arr[index2] = temp;
}
function selectionSort(arr) {
  for (var i = 0, n = arr.length; i < n; i++) {
    var minIndex = i;
    // 查找数组中剩余元素的最小值, 查找最小值还有一个方式: Math.min.apply(Math,arr)
    for (var j = i + 1; j < n; j++)
      if (arr[j] < arr[minIndex])
        minIndex = j;

    // 交换两个位置
    swap(arr, i, minIndex);
  }
}

function test() {
  var arr = [10, 9, 8, 6, 5, 4, 3, 2, 1, 7]
  selectionSort(arr)
  console.log(arr)
}
test()
```

## 插入排序 O(n^2)级别

> 思路: 1. 从第二个元素开始, 和上一个元素做比较, 如果小于上一个元素, 则进行位置交换, 否则说明该位置就是正确的位置

!> 和选择排序不同的是插入排序可以提前结束循环

?> 当是一个已经有序的数组进行排序的时候, 则插入排序会是一个O(n)级别的排序, 因为内层循环执行了一次, 当是一个趋于有序是数组的时候, 速度会越快.

```js
// 未优化
function insertionSort(arr) {
  for(var i = 1, n = arr.length; i < n; i ++) {
    for(var j = i; j > 0; j--) {
      if(arr[j] < arr[j - 1]) {
        swap(arr, j , j - 1)
      } else {
        break;
      }
    }
  }
}
```

优化后, 针对交换位置进行优化
```js
function insertionSort(arr) {
  for(var i = 1, n = arr.length; i < n; i ++) {
    var temp = arr[i]
    for(var j = i; j > 0; j--) {
      if(temp < arr[j - 1]) {
        arr[j] = arr[j - 1]
      } else {
        break;
      }
    }
    arr[j] = temp;
  }
}
```

## 冒泡排序

> 思路：拿第一个元素和后面相邻元素做比较，如果大于后面的元素则进行交换，否则不用交换，再拿第二个元素和后面的比较重复上一轮的操作，每一轮的比较都是找出最大元素

```js
function bubbleSort(arr) {
  for(var i = 0, len = arr.length - 1; i < len; i++) {
    for(var j = 0, len2 = arr.length - 1 - i; j < len2; j++) {
      if(arr[j] > arr[j + 1]) {
        // 交换两个元素的位置
        var temp = arr[j]
        arr[j] = arr[j + 1]
        arr[j + 1] = temp
      }
    }
  }
  console.log(arr) // 已经是有序数组了
}
```

?> 外层循环是每次比较的次数，内层循环是从哪个位置开始比较，比较几次

## 快速排序 O(NLogn)

```js
function quickSort(arr) {
  // 这是结束递归的条件，当数组中只有一个元素的时候返回
  if(arr.length <= 1) return arr;

  // 查找基准数索引
  var povitIndex = Math.floor(arr.length / 2);

  // 删除基准数并记录基准数
  var povit = arr.splice(povitIndex, 1)[0]

  // 定义两个空数组，将小于基准数的元素放到left数组中，大于基准数的元素放到right中
  var left = [];
  var right = [];

  for(var i = 0, len = arr.length; i < len; i++) {
    if(arr[i] > povit) {
      right.push(arr[i])
    } else {
      left.push(arr[i])
    }
  }
  // 递归执行该函数

  return quickSort(left).concat(povit, quickSort(right))
}

var arr = [1,4,6,3,2,8]
var newArr = quickSort(arr)
console.log(newArr)
```