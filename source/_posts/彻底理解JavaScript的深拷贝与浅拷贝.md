---
title: 彻底理解JavaScript的深拷贝与浅拷贝
date: 2019-06-24 18:03:59
tags: 
- JavaScript
categories: 
- web前端
- JS基础
---

##### 是什么
js中有基础类型和引用类型。
基础类型是存储在栈内存中的，按值存储，按值访问。基本类型有Number，String，Boolean，Null，Undefined，Symbol
引用类型是存储在堆内存中的，值是可变的。在栈中保存对应的指针（一个指向堆的引用地址），指向堆中实际的值。比如数组，对象，正则等，除了基本数据类型，都是引用类型了。
<!-- more -->

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

ES6中的Object.assign方法，Object.assign是ES6的新函数。Object.assign() 方法可以把任意多个的源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象。但是 Object.assign() 进行的是浅拷贝，拷贝的是对象的属性的引用，而不是对象本身。

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