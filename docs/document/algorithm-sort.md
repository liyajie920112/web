## 选择排序

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