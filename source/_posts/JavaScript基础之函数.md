---
title: JavaScript基础之函数
date: 2019-10-10 19:12:00
tags: 
- JavaScript
categories: 
- web前端
- JS基础
---
### 创建函数的方式

##### 函数声明和函数表达式

```js
// 函数表达式   匿名
var foo1 = function(...){}
// 函数表达式   命名
var foo2 = function acc(...){}
// 函数表达式 也就是立即执行函数
(function(){...})
// 函数表达式
setTimeout(funciton timer(){...},200)
// 函数声明
function(){...}
```
函数声明和函数表达式之间最重要的区别是它们的名称标识符将会绑定在何处。**函数声明会绑定在自身的作用域中，而函数表达式会绑定在表达式自身的函数中，而不是所在作用域中。**
<!-- more -->
##### 箭头函数

```js
// 没有return
acc = () => {...}
```
特点： 

+ **没有 this、super、arguments 和 new.target 绑定** 箭头函数内部的这些值直接取自定义时的外围非箭头函数
+ **不能通过 new 关键字调用** 箭头函数没有 [[Construct]] 方法，所以不能被用作构造函数，如果通过 new 关键字调用箭头函数，程序会抛出错误。
+ **没有原型** 由于不可以通过 new 关键字调用箭头函数，因而没有构建原型的需求，所以箭头函数不存在 prototype 这个属性。
+ **不可以改变 this 的绑定** 函数内部的 this 值不可被改变，在函数声明周期内始终保持一致。
+ **不支持 arguments 对象** 箭头函数没有 arguments 绑定，所以你必须通过命名参数和不定参数这两种形式访问函数的参数。
+ **不支持重复的命名参数** 无论是在严格还是非严格模式下，箭头函数都不支持重复的命名参数；而在传统函数的规定中，只有在严格模式下才不能有重复的命名参数。

由于没有 this 的绑定，箭头函数的 this 值不受 call()、apply()、bind() 方法的影响。

箭头函数完全修复了this的指向，this总是指向词法作用域，也就是外层调用者obj

##### 构造函数与类

在 ES6 Class 特性出现以前，我们经常使用 构造函数来模拟类的特性。思路基本都是：首先创建一个构造函数，然后定义另一个方法并复制给构造函数的原型。
 
```js
function Person(name) {
    this.name = name
}
Person.prototype.sayName = function() {
    console.log(this.name)
}
 
const person = new Person('Tumars')
person.sayName()  // "Tumars"
 
console.log(person instance Person)  // true
console.log(person instance Object) // true
```
上面代码中 Person 是一个构造函数，其执行后创建了一个名为 name 的属性；给 Person 的原型添加一个 sayName() 方法，所以 Person 对象的所有实例都会共享这个方法。由于存在原型继承的特性，person 对象是 Person 的实例，也是 Object 的实例。

通过构造函数创建的普通函数对象，拥有原型。

ES6 中的类是对构造函数写法的一种语法糖，它简化了构造函数的写法。

```js
class PersonCLass {
    constructor(name) {
        this.name = name
    }
 
    // 等价于 Person.prototype.sayName()
    sayName() {
        console.log(this.name)
    }
}
 
const person = new Person('Tumars')
person.sayName()  // "Tumars"
 
console.log(person instance PersonClass)  // true
console.log(person instance Object) // true
```
通过类声明语法定义 PersonClass 的行为与之前创建 Person 构造函数的过程相似，只是这里直接在类中通过特殊的 constructor 方法名来定义构造函数，且由于这种类使用简介语法拉定义方法，因而不需要添加 function 关键字。

### 实例方法

**apply()、call()**

使用 apply 与 call 调用函数被称为函数应用。这两个方法的用途都是在特定的作用域中调用函数，实际上等于设置函数体内 this 对象的值。

+ apply() 方法调用一个函数, 其具有一个指定的this值，以及作为一个数组（或类似数组的对象）提供的参数。
+ call() 方法调用一个函数, 其具有一个指定的this值和分别地提供的参数(参数的列表)。

**bind()**

bind()方法会创建一个新的函数, 当被调用时，将其this关键字设置为提供的值，在调用新函数时，在任何提供之前提供一个给定的参数序列。

### 函数去抖与函数节流




### 函数式编程

声明式的，不可变的，没有副作用的是函数式编程的三大护法

+ 纯函数

    对于相同的输入，永远会得到相同的输出，而且没有任何可观察的副作用，也不依赖外部环境的状态的函数，叫做纯函数。
    
+ 函数的柯里化

    传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。
    
+ 高阶函数

    高阶函数，就是把函数当参数，把传入的函数做一个封装，然后返回这个封装函数,达到更高程度的抽象。
    
```js
var checkage = min => (age => age > min);
var checkage18 = checkage(18); // 先将18作为参数，去调用此函数，返回一个函数age => age > 18;
checkage18(20);// 第二步，上面返回的函数去处理剩下的参数，即 20 => 20 > 18; return true;
```
　事实上柯里化是一种“预加载”函数的方法，通过传递较少的参数，得到一个已经记住了这些参数的新函数，某种意义上讲，这是一种对参数的“缓存”，是一种非常高效的编写函数的方法。

**导图**

![image](http://www.ferecord.com/wp-content/uploads/2017/12/Function.png)