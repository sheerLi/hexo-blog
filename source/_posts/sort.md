---
title: 基本排序的方式
categories: JavaScript
date: 2019-05-01 19:47:13
tags: 
  - 排序
  - JavaScript
  - 前端
---
以下是 **js** 实现常用的基本排序，包括 **快速排序**、**冒泡排序**、**堆排序**、**桶排序**、**插入排序**、**归并排序**.

### 快速排序

>    排序过程只需要三步:  
> （1）在数据集之中，选择一个元素作为"基准"（pivot）。  
> （2）所有小于"基准"的元素，都移到"基准"的左边；所有大于"基准"的元素，都移到"基准"的右边。  
> （3）对"基准"左边和右边的两个子集，不断重复第一步和第二步，直到所有子集只剩下一个元素为止。  

``` js
function quickSort(array) {
  if( arr.length <= 1 ) {
    return arr;
  }
  // 找出数组中的基准值
  var pivotIndex = Math.floor(arr.length / 2);
  var pivot = arr.splice(pivotIndex, 1)[0];
  // 定义左右两个个空数组
  var left = [], right = [];
  for(var i=0, len = arr.length; i < len; i++){
    if(arr[i] < pivot) {
      left.push(arr[i])
    } else {
      right.push(arr[i])
    }
  }
  // 递归调用快排
  return quickSort(left).concat([pivot], quickSort(right));
}
```

### 冒泡排序

> 数组中有 n 个数，比较每相邻两个数，如果前者大于后者，就把两个数交换位置;  
> 这样一来，第一轮就可以选出一个最大的数放在最后面；  
> 那么经过 n-1（数组的 length - 1） 轮，就完成了所有数的排序。

``` js
function bubbleSort(arr){
  for(var i = 0; i < arr.length - 1; i++){
    // 声明一个变量，作为标志位
    var done = true;
    for(var j = 0; j < arr.length-1-i; j++){
      if(arr[j] > arr[j+1]){
        var smtp = arr[j];
        arr[j] = arr[j+1];
        arr[j+1] = smtp;
        // 若有交换过，标志位置为 false
        done = false;
      }
    }
    // 可以减少不必要的排序，提高性能
    if(done){
      break;
    }
  }   
  return arr;
}
```

### 插入排序

> 1.从第一个元素开始，该元素可以认为已经被排序；  
> 2.取出下一个元素，在已经排序的元素序列中从后向前扫描；  
> 3.如果该元素（已排序）大于新元素，将该元素移到下一位置；  
> 4.重复步骤3，直到找到已排序的元素小于或者等于新元素的位置；  
> 5.将新元素插入到该位置后； 
> 重复步骤2~5。

``` js
function insertionSort(array) {
  if (Object.prototype.toString.call(array).slice(8, -1) === 'Array') {
    for (var i = 1; i < array.length; i++) {
        var key = array[i];
        var j = i - 1;
        while (j >= 0 && array[j] > key) {
            array[j + 1] = array[j];
            j--;
        }
        array[j + 1] = key;
    }
    return array;
  } else {
      return 'array is not an Array!';
  }
}
```

### 选择排序

> 初始状态：无序区为R[1..n]，有序区为空；  
> 第i趟排序(i=1,2,3…n-1)开始时，当前有序区和无序区分别为R[1..i-1]和R(i..n）。该趟排序从当前无序区中选出关键字最 小的记录 R[k]，将它与无序区的第1个记录R交换，使R[1..i]和R[i+1..n)分别变为记录个数增加1个的新有序区和记录个数减少1个的新无序区；  
> n-1趟结束，数组有序化了。

``` js
function selectionSort(array) {
  if (Object.prototype.toString.call(array).slice(8, -1) === 'Array') {
    var len = array.length, temp;
    for (var i = 0; i < len - 1; i++) {
      var min = array[i];
      for (var j = i + 1; j < len; j++) {
        if (array[j] < min) {
          temp = min;
          min = array[j];
          array[j] = temp;
        }
      }
      array[i] = min;
    }
    return array;
  } else {
      return 'array is not an Array!';
  }
}
```

### 桶排序

> 设置一个定量的数组当作空桶；  
> 遍历输入数据，并且把数据一个一个放到对应的桶里去；  
> 对每个不是空的桶进行排序；  
> 从不是空的桶里把排好序的数据拼接起来。

```js
/*方法说明：桶排序
@param  array 数组
@param  num   桶的数量*/
function bucketSort(array, num) {
  if (array.length <= 1) {
    return array;
  }
  var len = array.length, buckets = [], result = [], min = max = array[0], regex = '/^[1-9]+[0-9]*$/', space, n = 0;
  num = num || ((num > 1 && regex.test(num)) ? num : 10);
  for (var i = 1; i < len; i++) {
    min = min <= array[i] ? min : array[i];
    max = max >= array[i] ? max : array[i];
  }
  space = (max - min + 1) / num;
  for (var j = 0; j < len; j++) {
    var index = Math.floor((array[j] - min) / space);
    if (buckets[index]) {   //  非空桶，插入排序
      var k = buckets[index].length - 1;
      while (k >= 0 && buckets[index][k] > array[j]) {
          buckets[index][k + 1] = buckets[index][k];
          k--;
      }
      buckets[index][k + 1] = array[j];
    } else {    //空桶，初始化
        buckets[index] = [];
        buckets[index].push(array[j]);
    }
  }
  while (n < num) {
    result = result.concat(buckets[n]);
    n++;
  }
  return result;
}
```

### 归并排序

> 把长度为n的输入序列分成两个长度为n/2的子序列；  
> 对这两个子序列分别采用归并排序；  
> 将两个排序好的子序列合并成一个最终的排序序列。

```js
function mergeSort(array, p, r) {
  if (p < r) {
    var q = Math.floor((p + r) / 2);
    mergeSort(array, p, q);
    mergeSort(array, q + 1, r);
    merge(array, p, q, r);
  }
}
function merge(array, p, q, r) {
  var n1 = q - p + 1, n2 = r - q, left = [], right = [], m = n = 0;
  for (var i = 0; i < n1; i++) {
    left[i] = array[p + i];
  }
  for (var j = 0; j < n2; j++) {
    right[j] = array[q + 1 + j];
  }
  left[n1] = right[n2] = Number.MAX_VALUE;
  for (var k = p; k <= r; k++) {
    if (left[m] <= right[n]) {
      array[k] = left[m];
      m++;
    } else {
        array[k] = right[n];
        n++;
    }
  }
}
```

### 堆排序

> 将初始待排序关键字序列(R1,R2....Rn)构建成大顶堆，此堆为初始的无序区；  
> 将堆顶元素R[1]与最后一个元素R[n]交换，此时得到新的无序区(R1,R2,......Rn-1)和新的有序区(Rn),且满足R[1,2...n-1]<=R[n]；  
> 由于交换后新的堆顶R[1]可能违反堆的性质，因此需要对当前无序区(R1,R2,......Rn-1)调整为新堆，然后再次将R[1]与无序区最后一个元素交换，得到新的无序区(R1,R2....Rn-2)和新的有序区(Rn-1,Rn)。不断重复此过程直到有序区的元素个数为n-1，则整个排序过程完成。

```js
/*方法说明：堆排序
@param  array 待排序数组*/           
function heapSort(array) {
  if (Object.prototype.toString.call(array).slice(8, -1) === 'Array') {
    //建堆
    var heapSize = array.length, temp;
    for (var i = Math.floor(heapSize / 2); i >= 0; i--) {
      heapify(array, i, heapSize);
    }

    //堆排序
    for (var j = heapSize - 1; j >= 1; j--) {
      temp = array[0];
      array[0] = array[j];
      array[j] = temp;
      heapify(array, 0, --heapSize);
    }
  } else {
      return 'array is not an Array!';
  }
}
/*方法说明：维护堆的性质
@param  arr 数组
@param  x   数组下标
@param  len 堆大小*/
function heapify(arr, x, len) {
  if (Object.prototype.toString.call(arr).slice(8, -1) === 'Array' && typeof x === 'number') {
    var l = 2 * x, r = 2 * x + 1, largest = x, temp;
    if (l < len && arr[l] > arr[largest]) {
      largest = l;
    }
    if (r < len && arr[r] > arr[largest]) {
      largest = r;
    }
    if (largest != x) {
      temp = arr[x];
      arr[x] = arr[largest];
      arr[largest] = temp;
      heapify(arr, largest, len);
    }
  } else {
      return 'arr is not an Array or x is not a number!';
  }
}
```

