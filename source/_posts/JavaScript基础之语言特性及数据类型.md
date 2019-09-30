---
title: JavaScript基础之语言特性及数据类型
date: 2019-09-30 13:07:21
tags:
---

### 零、是什么
 JavaScript是前端必学语言，和HTML,CSS并称为前端三剑客，是一门运行在浏览器端的脚本语言，功能是操作DOM，处理数据，渲染特效等
### 一、语言特性

#### 1、弱类型
说白了就是类型定义，对应的就是强类型，比如Java，C等都是强类型语言，在使用变量的时候必须声明是哪种类型的，一旦被定义了数据类型，除非强制类型转换，那么它到销毁的时候都是这个类型的，这样当然是比较安全的。而弱类型就是不需要定义是什么数据类型，它的值就表示了它是什么类型的。如下：
```js
var asd1 = 'have a nice day'   //string
var asd2 = 20                  //number
var asd3 = new Date()          //object
var asd4 = ['1','3','4']       //object
var asd5 = true                //boolean

```
    
#### 2、动态性
可以直接对用户的操作做出相应，不需要通过Web服务器。采用事件驱动的方式进行，比如你点击一个提交按钮就是一个事件，也就是说你执行某种操作的动作，非常常见。当相关事件在触发的时候就会自动执行需要响应的脚本或函数。
 
#### 3、运行在浏览器端
js脚本语言不允许访问本地硬盘，也不能存储在服务器上，所以它只能通过浏览器实现数据的展示和动态交互，正是因为这样，保证了数据的安全性。

#### 4、跨平台性
依赖于浏览器本身，与操作环境无关。只要能运行浏览器的计算机，并安装了支持javascript的浏览器就可以正确执行，从而实现了“编写一次，走遍天下”的梦想。

#### 5、脚本语言
解释性脚本语言，javascript不需要编译，只需要嵌入到html代码中，由浏览器逐行加载解释执行

### 二、基本数据类型
JavaScript的基本类型值是保存在栈内存中的简单数据段，按值存储，所以按值访问。基本数据类型有：

Number、String、Boolean、Null、Undefined、以及ES6的symbol(独一无二的值)。

用typeof 来检验基本类型，参考弱类型的举例，可以返回这些值：undefined、boolean、string、number、object、function

这里还有一些有意思的例子：
```js
typeof undefined      //undefined
typeof null           //object
typeof ['1','2','3']  //object
typeof {asd:'sssss'}  //object

```

所以不要使用typeof 来区分数组还是对象，因为都返回object。

有时需要根据数组或对象里有没有值来判断是否显示:
+ 如果是数组，arr.length>0。
+ 如果是对象，可以直接拿属性判断 obj.name。但如果不知道有什么属性，可以使用 Object.keys(obj).length > 0 来判断

**说说null和undefined的区别。**
+ 同：都表示 无
+ 不同： 如果转换为数值
    + undefined => NaN 有声明，但未赋值或者未初始化
    + null => 0 (原型链的终点) 没有，也没有定义，不存在

```js
typeof 未定义值     // undefined
typeof 未初始化值   // undefined

```

### 三、进阶
JavaScript 的基本知识就是上述，你可能发现好像很简单并不多，那只是基本类型，我们常用到的Object还没有介绍呢，接下来说一下进阶的知识。

#### 1、引用类型
和基本类型对应，引用类型是保存在堆内存中的对象，值是可变的，在栈中保存对应的指针（一个指向堆的引用地址），指向堆中实际的值。

类型值：Object（在JS中除了基本数据类型以外的都是对象，数据是对象，函数是对象，正则表达式是对象）

使用 instanceof 检测引用类型 。      需明确确定是哪种类型，返回 布尔值

```js
var a = [1,2]
var b = {'a':'asss'}
alert( a instanceof Array)   // true
alert( b instanceof Object)  // true 

```
除了使用instanceof ,还可以使用一个方法来返回复杂类型的类型值。

```js
var arr = [3,4,5,6,2,1]
var aa = Object.prototype.toString(arr)    // '[object Array]'
aa.substr(8,aa.length-9)     // Array

```

那么基本类型和引用类型有什么区别呢。

```js
var arr = [2,4,6];
var bcc = arr;//传址 ,对象中传给变量的数据是引用类型的，会存储在堆中；
var cxx = arr[0];//传值，把对象中的属性/数组中的数组项赋值给变量，这时变量C是基本数据类型，存储在栈内存中；改变栈中的数据不会影响堆中的数据
alert(bcc);//2,4,6
alert(cxx);//2
//改变数值 
bcc[1] = 6;
cxx = 7;
alert(arr[2]);//6
alert(arr[0]);//2

```
 从上面我们可以得知，当我改变bcc中的数据时，arr中数据也发生了变化；但是当我改变cxx的数据值时，arr却没有发生改变。
 
  这就是**传值**与**传址**的区别。因为arr是数组，属于引用类型，所以它赋予给bcc的时候传的是栈中的地址（相当于新建了一个不同名“指针”），而不是堆内存中的对象。而cxx仅仅是从arr堆内存中获取的一个数据值，并保存在栈中。所以bcc修改的时候，会根据地址回到arr堆中修改，cxx则直接在栈中修改，并且不能指向arr堆内存中。
  
  接下来就涉及到比较常用的深拷贝和浅拷贝，我们放在之后来说
  
#### 2、类型判断
在开发的过程中经常会判断值是否相等来进行下一步的操作，在js中有两个方式判断两个值是否相等。

** ==   等于操作符 **
js是弱类型语言，在使用 == 操作符的时候，会进行强制类型转换

```js
""           ==   "0"           // false
0            ==   ""            // true
0            ==   "0"           // true
false        ==   "false"       // false
false        ==   "0"           // true
false        ==   undefined     // false
false        ==   null          // false
null         ==   undefined     // true
" \t\r\n"    ==   0             // true

```
因为在强制类型转换的时候规则比较复杂，所以说使用 == 是一个不好的编程习惯，也会带来性能消耗。

**===  全等操作符**
不会进行强制类型转换，

```js
""           ===   "0"           // false
0            ===   ""            // false
0            ===   "0"           // false
false        ===   "false"       // false
false        ===   "0"           // false
false        ===   undefined     // false
false        ===   null          // false
null         ===   undefined     // false
" \t\r\n"    ===   0             // false

```

所以推荐使用 === 操作符。

#### 3、类型转换
 所以已经使用了 === 操作符，但是还是会产生很多问题，那么不然我们自己进行类型转换。

 转换为 字符类型：    将一个值加上空字符串可以轻松转换为字符串类型
 
 ```js
 '' + 10 === '10'; // true
 ```
 
 转换为 数字类型:      使用一元的加号操作符，可以把字符串转换为数字。
 
 ```js
 +'10' === 10; // true
 ```
 
 转换为布尔值：     通过使用 否 操作符两次，可以把一个值转换为布尔型
 
```js
!!'foo';   // true
!!'';      // false
!!'0';     // true
!!'1';     // true
!!'-1'     // true
!!{};      // true
!!true;    // true

```

是不是觉得很奇怪呢，这里我们就要说一下 **假值**

> ''、0、undefined、null、false、NaN 都是假值，返回 false。

其他的都将是真值，包括对象、数组、正则、函数等。注意 '0'、'null'、'false'、{}、[]也都是真值  。 

**结论**： false 0 '' 之间相互比较都是true,null和undefined相互比较是true。其余全是false