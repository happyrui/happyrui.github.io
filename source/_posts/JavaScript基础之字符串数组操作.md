---
title: JavaScript基础之字符串数组操作
date: 2019-10-10 19:04:31
tags: 
- JavaScript
categories: 
- web前端
- JS基础
---
## 数组

+ 截取相关
    + slice **截取**   不会影响原始数组
    
    ``` js 
        var arr = [1,2,3,4,5];
        // 截取  从 i 到 j的数组，不会改变原数组
        console.log(arr.slice(3));    // [4,5]
        console.log(arr);         // [1,2,3,4,5]
        console.log(arr.slice(1,3))   // [2,3]
        console.log(arr);      // [1,2,3,4,5]

    ```



    + splice  **删除，插入，替换**  改变原始数组
    
    ``` js
        var strArray = ['1', '2', 'd', 's', 'i'];
        //splice(i,j)  删除   以下标为i开始，截取j个元素
        strArray.splice(0,1);  //已删除的    ["1"]
        console.log(strArray);    //删除后的数组  ["2", "d", "s", "i"]
        //splice(i,0,j) 插入   以i开始，插入项,在 i 下标之前开始插入
        strArray.splice(1,0,'f','g','h');  
        console.log(strArray);     // ["2", "f", "g", "h", "d", "s", "i"]
        //splice(i,n,j) 替换  以i开始，删除n项，插入其他项
        strArray.splice(1,1,'s','z');  // 即删除i及其后面n-1项，在插入其他项。
        console.log(strArray);  //  ["2", "s", "z", "g", "h", "2", "d", "s", "i"]
    ```
<!-- more -->
+ 栈与队列操作（数组增加或删除元素）

``` js

var arr = [1,3,4,5,8,2,4,9];
console.log(arr.push(5));  //在数组末尾添加元素
console.log(arr);
console.log(arr.pop());     //弹出栈顶那项  删除并返回数组的最后一个元素
console.log(arr);
console.log(arr.shift());   //删除并返回数组的第一个元素
console.log(arr);
console.log(arr.unshift(9)); //在数组头部添加元素
console.log(arr);

```

+ 重排序方法
    + reverse()  反转数组项的顺序
    + sort()      默认按照字符串排序a-z
 
``` js
// 增加一个compare函数，实现对数值排序(升序或降序)
var arr=[1,6,5,7,2,8,3];
console.log(arr.reverse());
function compare(value1,value2){
    return value1-value2;  //升序
    return value2-value1;  //降序
}
console.log(arr.sort(compare));
```

+ 迭代方法
    + every()       如果该函数对每一项都返回true，则返回true
    + filter()         返回该函数会返回true 的项组成的数组
    + forEach()    这个方法没有返回值
    + map()         返回每次函数调用的结果组成的数组
    + some()       如果该函数对任意一项返回true，则返回true
  
迭代方法在数组的操作中是很常用的

1. arr.forEach()是和for循环一样，是代替for。arr.map()是修改数组其中的数据，并返回新的数据。
2. arr.forEach() 没有return  arr.map() 有return

  

+ 归并方法
    + reduce()       数组的逐个遍历，顺序
    + reduceRight()      逆序
 
``` js

var values = [1,2,3,4,5];
var sum = values.reduce(function(prev, cur){
    return prev + cur;
});
var sum1 = values.reduceRight(function(prev, cur){
    return prev * cur;
});
console.log(sum);   // 15
console.log(sum1);  // 120

```

+ 其他方法
    + from() 将伪数组变成数组，就是只要有length的就可以转成数组
    ```js
    // 有一个用法，数组去重
    Array.from(new Set(arr))
    ```
    + of() 将一组值转换成数组，类似于声明数组 
    + find(callback) 找到第一个符合条件的数组成员  返回数组
    + findIndex(callback) 找到第一个符合条件的数组成员的索引值  返回number
    + fill(target, start, end) 使用给定的值，填充一个数组,ps:填充完后会改变原数组

## 字符串

+ 字符方法
    + chartAt(num)  返回字符串 中下标为 num 的字符，如果参数超出该范围，返回空字符串，如果没有参数，返回位置为0的字符;
    + charCodeAt(num)  同上，返回 字符编码而不是字符

+ 字符串操作方法
    + substring(i, j)  返回从 i到 j 的数据，不包括j   不改变原数据
    + substr(i, l)  返回从i开始，长度为l的数据    不改变原数据
    ``` js
    const s = 'asasdadsadasd'
    s.substr(1,4)    // sasd
    s.substring(2,5) // asd
    
    ```

+ 字符串位置方法
    + indexOf()，参数为子字符串，从左至右查找，返回子字符串位置，如果没找到该子字符串，返回-1。
    + lastIndexOf()，参数为子字符串，从右至左查找，返回子字符串位置，如果没找到该子字符串，返回-1。

+ 大小写转换
    + toLowerCase() ，创建原字符串的小写副本
    + toUpperCase() ，创建原字符串的大写副本

+ 其他方法
    + trim()方法   该方法创建一个字符串的副本，删除前置和后缀的所有空格。
    + match() – 检查一个字符串是否匹配一个正则表达式。
    + replace() – 用来查找匹配一个正则表达式的字符串，然后使用新字符串代替匹配的字符串。
    + search() – 执行一个正则表达式匹配查找。如查找成功，返回字符串中匹配的索引值。否则返回 -1 
    + split()      通过将字符串划分成子串，将一个字符串做成一个字符串数组
    + join()   将分割的字符数组连接成字符串，参数为连接的分隔符，默认为逗号
    ``` js
    console.log(str1.split('').reverse().join(''));   //字符串反转
    ```


### 通用方法

+ concat(a1, a2)   不改变原数据的值
    + 连接两个数组 返回值为连接后的新数组
    + 连接多个字符串，返回值为连接后的新字符串
+ slice(i, j)  截取  从 i 到 j，不会改变原数据，不包括 j



![](https://image-static.segmentfault.com/629/456/629456743-57a401904d0d1)