---
title: JavaScript基础之常见算法
date: 2019-10-10 19:10:57
tags: 
- JavaScript
categories: 
- web前端
- 算法
---
### 排序算法

|Algorithm| Average |Best| Worst| extra space| stable|
| ---- | ---- | ---- | ---- | ---- | ---- |
|冒泡排序 |O(N^2) |O(N) |O(N^2) |O(1) |稳定|
|直接插入排序|O(N^2)| O(N)| O(N^2) |O(1)| 稳定|
|折半插入排序 |O(NlogN) |O(NlogN) |O(N^2)| O(1) |稳定|
|简单选择排序 |O(N^2)| O(N^2)| O(N^2) |O(1)| 不稳定|
|快速排序| O(NlogN) |O(NlogN) |O(N^2) |O(logN)~O(N^2) |不稳定|
|归并排序 |O(NlogN) |O(NlogN)| O(NlogN)| O(N) |稳定|
|堆排序 |O(NlogN) |O(NlogN) |O(NlogN) |O(1) |不稳定|
|计数排序 |O(d*(N+K)) |O(d*(N+K)) |O(d*(N+K)) |O(N+K)| 稳定|

<!-- more -->

+ 冒泡:

依次比较相邻的两个数，如果不符合排序规则，则调换两个数的位置。这样一遍比较下来，能够保证最大（或最小）的数排在最后一位。再对最后一位以外的数组，重复前面的过程，直至全部排序完成。

```js
function bubbleSort(arr){
	var len = arr.length;
	for(var i = 0;i<len;i++){
		for(var j = 0;j<len-1-i;j++){
			if(arr[j]>arr[j+1]){  //相邻元素两两比较
				var temp = arr[j+1];  //元素交换
				arr[j+1] = arr[j];
				arr[j]=temp;
			}
		}
	}
	return arr;
}

```
改进：设置一标志性变量pos,用于记录每趟排序中最后一次进行交换的位置。由于pos位置之后的记录均已交换到位,故在进行下一趟排序时只要扫描到pos位置即可。

```js
function bubbleSort2(arr){
	var i=arr.length-1;  //初始时,最后位置保持不变
	while(i>0){
		var pos=0;//每趟开始时,无记录交换
		for(var j=0;j<i;j++){
			if(arr[j]>arr[j+1]){
				pos=j;//记录交换的位置
				var tmp=arr[j];
				arr[j]=arr[j+1];
				arr[j+1]=tmp;
			}
		}
		i=pos;//为下一趟排序作准备
	}
	return arr;
}

```

+ 选择:

选择排序（Selection Sort）与冒泡排序类似，也是依次对相邻的数进行两两比较。不同之处在于，它不是每比较一次就调换位置，而是一轮比较完毕，找到最大值（或最小值）之后，将其放在正确的位置，其他数的位置不变 

```js
function selectionSort(arr){
	var len=arr.length;
	var minIndex,temp;
	for(var i=0;i<len-1;i++){
		minIndex=i;
		for(var j=i+1;j<len;j++){
			if(arr[j]<arr[minIndex]){       //寻找最小的数
				minIndex=j;                //将最小数的索引保存
			}
		}
		temp=arr[i];
		arr[i]=arr[minIndex];
		arr[minIndex]=temp;
	}
	return arr;
}

```

+ 插入排序：

它将数组分成“已排序”和“未排序”两部分，一开始的时候，“已排序”的部分只有一个元素，然后将它后面一个元素从“未排序”部分插入“已排序”部分，从而“已排序”部分增加一个元素，“未排序”部分减少一个元素。以此类推，完成全部排序。

```js
function insertionSort(array) {
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
}

```
+ 希尔排序:

先取一个小于n的整数d1作为第一个增量，把文件的全部记录分组。所有距离为d1的倍数的记录放在同一个组中。先在各组内进行直接插入排序；`然后，取第二个增量d2<d1重复上述的分组和排序，直至所取的增量=1(<…<d2<d1)，即所有记录放在同一组中进行直接插入排序为止`

```js 
function shellSort(arr) {
	var len = arr.length,
		temp,
		gap = 1;
	while(gap < len/5) {          //动态定义间隔序列
		gap =gap*5+1;
	}
	for (gap; gap > 0; gap = Math.floor(gap/5)) {
		for (var i = gap; i < len; i++) {
			temp = arr[i];
			for (var j = i-gap; j >= 0 && arr[j] > temp; j-=gap) {
				arr[j+gap] = arr[j];
			}
			arr[j+gap] = temp;
		}
	}
	return arr;
}

```

+ 快速排序：

先确定一个“支点”（pivot），将所有小于“支点”的值都放在该点的左侧，大于“支点”的值都放在该点的右侧，然后对左右两侧不断重复这个过程，直到所有排序完成

```js
function qSort(arr){
	if(arr.length==0){
		return [];
	}
	var left=[];
	var right=[];
	var pivot=arr[0];
	for(var i=1;i<arr.length;i++){
		if(arr[i]<pivot){
			left.push(arr[i]);
		}else{
			right.push(arr[i]);
		}
	}
	return qSort(left).concat(pivot,qSort(right));
} 

```

+ 堆排序

将待排序的序列构造成一个最大堆，此时序列的最大值为根节点;依次将根节点与待排序序列的最后一个元素交换,再维护从根节点到该元素的前一个节点为最大堆，如此往复，最终得到一个递增序列

```js
function heapSort(array){
        //建堆
        var heapSize=array.length,temp;
        for(var i=Math.floor(heapSize/2)-1;i>=0;i--){
            heapify(array,i,heapSize);
        }
        //堆排序
        for(var j=heapSize-1;j>=1;j--){
            temp=array[0];
            array[0]=array[j];
            array[j]=temp;
            heapify(array,0,--heapSize);
        }
        return array;
}
function heapify(arr,x,len){
        var l=2*x+1,r=2*x+2,largest=x,temp;
        if(l<len&&arr[l]>arr[largest]){
            largest=l;
        }
        if(r<len&&arr[r]>arr[largest]){
            largest=r;
        }
        if(largest!=x){
            temp=arr[x];
            arr[x]=arr[largest];
            arr[largest]=temp;
            heapify(arr,largest,len);
        }
}


```

+ 合并排序：

将两个已经排序的数组合并，要比从头开始排序所有元素来得快。因此，可以将数组拆开，分成n个只有一个元素的数组，然后不断地两两合并，直到全部排序完成

```js 
function mergeSort(arr){  //采用自上而下的递归方法
    var len=arr.length;
    if(len<2){
        return arr;
    }
    var middle=Math.floor(len/2),
        left=arr.slice(0,middle),
        right=arr.slice(middle);
    return merge(mergeSort(left),mergeSort(right));
}
function merge(left,right){
    var result=[];
    while(left.length&&right.length){
        if(left[0]<=right[0]){
            result.push(left.shift());
        }else{
            result.push(right.shift());
        }
    }
    while(left.length)
        result.push(left.shift());
    while(right.length)
        result.push(right.shift());
    return result;
}
```


### 数组操作

+ 数组去重

```js
// 1、ES6新增方法
Array.from(new Set(array)); 
// 2、indexOf
function unique(arr){
    var data = [];
    for(var i = 0; i<arr.length;ix==) {
        if(data.indexOf(arr[i]) == -1) {
            data.push(arr[i])
        } 
    }
    return data;
}
// 3、对象
function unique(arr) {
    var table = {};
    var data = [];
    for(var i=0;i<arr.length;i++) {
        if(!table[arr[i]]) {
            table[arr[i]] = true;
            data.push(arr[i]);
        }
    }
    return data;
}
// 判断某个值是否只出现一次
arr.indexOf(arr[i]) == arr.lastIndexOf(arr[i])
```

+ 数组中最大差值

最小与最大数的差值

```js
// 耗能大
function MaxMin(arr) {
    arr.sort(function(n1, n2) {return n1-n2});
    return arr[arr.length-1] - arr[0];
}
// 
function getMaxProfit(arr) {
   var min = arr[0];
   var max = 0;
   for(var i=0; i<arr.length;i++){
       var current = arr[i];
       min = Math.min(min, current);
       var p = current - min;
       max = Math.max(max, p);
   }
   return max;
}

```

### 字符串操作

+ 判断回文

```js
// 利用反转
if(str == str.split('').reverse().join('')){
    console.log('是回文')
}
// 利用字符下标
for(var i=0;i<str.length;i++){
    if(str.charAt(i)==str.charAt(len-1-i)){
        console.log('是回文')
    }
}
// for循环反转拼接，再比较  

```
+ 反转

```js
str.split('').reverse().join('')
```
+ 随机生成

```js
function randomString(n){
    var str = 'abcdefghijklmnopqrstuvwxyz0123456789';
    var tmp = '';
    for(var i=0;i<n;i++)
        tmp += str.charAt(Math.round(Math.random()*str.length));
    return tmp;
}
```
+ 统计字符串中次数最多的字母

```js


```
+ 


### 深拷贝，浅拷贝

##### 是什么
js中有基础类型和引用类型。
基础类型是存储在栈内存中的，按值存储，按值访问。基本类型有Number，String，Boolean，Null，Undefined，Symbol
引用类型是存储在堆内存中的，值是可变的。在栈中保存对应的指针（一个指向堆的引用地址），指向堆中实际的值。比如数组，对象，正则等，除了基本数据类型，都是引用类型了。

基本类型的复制，是不会相互影响的。因为直接改变的就是栈中的值。而引用类型的复制，在修改其中一个的时候，另一个也会跟着发生变化。是因为复制的是栈中的指针，当改变值时，指针会仍然指向堆中实际的值，所以也就会跟着变化。

##### 举一些例子吧：
```js
// 基础类型
var a = 2;
b = a;
console.log(b)  //2
b = 3;
console.log(a, b) //2,3

// 引用类型
var arr = [2,4,6];
var bcc = arr;//传址 ,对象中传给变量的数据是引用类型的，会存储在堆中；
var cxx = arr[0];//传值，把对象中的属性/数组中的数组项赋值给变量，这时变量cxx是基本数据类型，存储在栈内存中；改变栈中的数据不会影响堆中的数据
console.log(bcc);//2,4,6
console.log(cxx);//2
//改变数值 
bcc[1] = 6;
cxx = 7;
console.log(arr[1]);//6
console.log(arr[0]);//2

```
从上面我们可以得知，当我改变bcc中的数据时，arr中数据也发生了变化；但是当我改变cxx的数据值时，arr却没有发生改变。

这就是传值与传址的区别。因为arr是数组，属于引用类型，所以它赋予给bcc的时候传的是栈中的地址（相当于新建了一个不同名“指针”），而不是堆内存中的对象。而cxx仅仅是从arr堆内存中获取的一个数据值，并保存在栈中。所以bcc修改的时候，会根据地址回到arr堆中修改，cxx则直接在栈中修改，并且不能指向arr堆内存中。  

##### 浅拷贝

简单来说，引用类型的直接复制，在修改其中一个的时候，另一个就会跟着变化。这种直接复制的方式  就是浅拷贝。浅拷贝是拷贝一层，深层次的对象级别的就拷贝引用

还有一种浅拷贝的方式：

ES6中的Object.assign方法，Object.assign是ES6的新函数。Object.assign() 方法可以把任意多个的源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象。但是 Object.assign() 进行的是浅拷贝，拷贝的是对象的属性的引用，而不是对象本身。

为了解决这个问题，引入深拷贝的方式，其实就是拷贝多层，每一级别的数据都会拷贝出来；

##### 深拷贝的实现方式

+ 手动复制

    把一个对象的属性复制给另一个对象的属性
    
+ 对象只有一层的话可以使用上面的：Object.assign()函数

+ JSON的方式，先转为字符串，再转为对象

    JSON.parse(JSON.stringify(obj))
    
    缺点：会抛弃对象的constructor。也就是深拷贝之后，不管这个对象原来的构造函数是什么，在深拷贝之后都会变成Object。而且只有可以转为json格式对象的才能这样使用。
    
+ 递归拷贝

```js
function deepClone(initalObj, finalObj) {    
  var obj = finalObj || {};    
  for (var i in initalObj) {        
    var prop = initalObj[i];        // 避免相互引用对象导致死循环，如initalObj.a = initalObj的情况
    if(prop === obj) {            
      continue;
    }        
    if (typeof prop === 'object') {
      obj[i] = (prop.constructor === Array) ? [] : {};            
      arguments.callee(prop, obj[i]);
    } else {
      obj[i] = prop;
    }
  }    
  return obj;
}
var str = {};
var obj = { a: {a: "hello", b: 21} };
deepClone(obj, str);
console.log(str.a);
```
+ Object.create()方法

```js
function deepClone(initalObj, finalObj) {    
  var obj = finalObj || {};    
  for (var i in initalObj) {        
    var prop = initalObj[i];        // 避免相互引用对象导致死循环，如initalObj.a = initalObj的情况
    if(prop === obj) {            
      continue;
    }        
    if (typeof prop === 'object') {
      obj[i] = (prop.constructor === Array) ? [] : Object.create(prop);
    } else {
      obj[i] = prop;
    }
  }    
  return obj;
}

```

### 其他常见

+ 十六进制颜色值的随机生成

```js
function randomColor(){
 var arrHex=["0","2","3","4","5","6","7","8","9","a","b","c","d"],
     strHex="#",
     index;
     for(var i=0;i < 6; i++){
      index=Math.round(Math.random()*15);
      strHex+=arrHex[index];
     }
 return strHex;
}
```
+ 阶乘
+ 二分查找


[JavaScript 面试中常见算法问题详解](https://zhuanlan.zhihu.com/p/25308541)

[一些常见算法的JavaScript实现](http://www.nowamagic.net/librarys/veda/detail/167)

[常见的js算法面试题收集，es6实现](https://juejin.im/post/5a7aaf745188257a5a4c9a39)