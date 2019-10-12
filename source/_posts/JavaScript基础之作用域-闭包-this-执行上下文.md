---
title: 'JavaScript基础之作用域,闭包,this,执行上下文'
date: 2019-10-12 11:16:00
tags: 
- JavaScript
categories: 
- web前端
- JS基础
---

### 作用域

作用域： 变量与函数的可访问范围量

分为
+ 全局作用域： 在代码中任何地方都能访问到的对象拥有全局作用域
+ 局部作用域： 一般只在固定的代码片段内可访问到。最常见的是在函数体内定义的变量，只能在函数体内使用。

在函数体内，局部变量的优先级高于同名的全局变量。如果在函数内声明的一个局部变量或者函数参数中带有的变量和全局变量重名，那么全局变量就被局部变量所遮盖。
<!-- more -->  
声明提前：JavaScript 函数里声明的所有变量（但不涉及赋值）都被「提前」至函数体的顶部
由于 JavaScript 没有块级作用域，因此一些程序员特意将变量声明放在函数体顶部，而不是将声明靠近放在使用变量之处。这种做法使得他们的源代码非常清晰地反映了真实的变量作用域。

```js
var color = "blue";
function changeColor(){
    var anotherColor = "red";
    function swapColors(){
        var tempColor = anotherColor;
        anotherColor = color;
        color = tempColor;
        // 这里可以访问color、anotherColor和tempColor
    }
    // 这里可以访问color和anotherColor，但不能访问tempColor
    swapColors();
}
// 这里只能访问color
changeColor();
```

全局环境、changeColor() 的局部环境和 swapColors() 的局部环境。

全局环境中有一个变量 color 和一个函数 changeColor()。changeColor() 的局部环境中有一个名为 anotherColor 的变量和一个名为 swapColors() 的函数，但它也可以访问全局环境中的变量 color。swapColors() 的局部环境中有一个变量tempColor，该变量只能在这个环境中访问到。无论全局环境还是 changeColor() 的局部环境都无权访问 tempColor。然而，在 swapColors() 内部则可以访问其他两个环境中的所有变量，因为那两个环境是它的父执行环境。

内部环境可以通过作用域链访问所有的外部环境，但外部环境不能访问内部环境中的任何变量和函数。`每个环境都可以向上搜索作用域链`，以查询变量和函数名；但任何环境都不能通过向下搜索作用域链而进入另一个执行环境。

### JS执行机制



### 闭包

由于在 Javascript 语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成定义在一个函数内部的函数

用处有两个:

+ 一个是可以读取函数内部的变量（作用域链），
+ 另一个就是让这些变量的值始终保持在内存中。

```js
function fun() {　　　                          
    var n = 1;                                      
    add = function() {    
    //add 是一个全局变量，add 的值是一个匿名函数。而这个匿名函数本身也是一个闭包，和 fun2 处于同一作用域，所以 add 相当于是一个 setter，可以在函数外部对函数   内部的局部变量进行操作
        n += 1 
    }
    function fun2(){
        console.log(n);
    }
    return fun2;
}
var result = fun();　　
result(); // 1
add();
result(); // 2   
```

```js
function fun() {　　　
    var n = 1;
    var  add = function() {
        n += 1
    }
    function fun2(){
        console.log(n);
    }
    return fun2;
}
var result = fun();　　
result(); // 1
add();
result(); // 1

```
使用闭包解决一些问题：

```js
var add = function() {
    var counter = 0;
    var plus = function() {return counter += 1;}  //闭包 
    return plus;
}

var puls2 = add();
console.log(puls2());
console.log(puls2());
console.log(puls2());
// 计数器 counter 受 add() 函数的作用域保护，只能通过 puls2 方法修改。

```

使用闭包的注意事项:

+ 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在 IE 中可能导致内存泄露。**解决方法是，在退出函数之前，将不使用的局部变量全部删除或设置为 null，断开变量和内存的联系。**
+ 闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（public method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。	

### this

this 是 JavaScript 的关键字，指函数执行时的上下文，跟函数定义时的上下文无关。随着函数使用场合的不同，this 的值会发生变化。但是有一个总的原则，那就是 this 指代的是调用函数的那个对象。

**全局上下文**

在全局上下文中，也就是在任何函数体外部，this 指代全局对象。
```js
// 在浏览器中，this 指代全局对象 window
console.log(this === window);  // true
```

**函数上下文**

在函数上下文中，也就是在任何函数体内部，this 指代调用函数的那个对象。

**函数调用中的 this**

```js
function f1(){
    return this;
}
console.log(f1() === window); // true
```

如上代码所示，直接定义一个函数 f1()，相当于为 window 对象定义了一个属性。直接执行函数 f1()，相当于执行 window.f1()。所以函数 f1() 中的 this 指代调用函数的那个对象，也就是 window 对象。

```js
function f2(){
    "use strict"; // 这里是严格模式
    return this;
}
console.log(f2() === undefined); // true
```

如上代码所示，在「严格模式」下，禁止 this 关键字指向全局对象（在浏览器环境中也就是 window 对象），this 的值将维持 undefined 状态。

**对象方法中的 this**

```js
var o = {
    name: "stone",
    f: function() {
        return this.name;
    }
};
console.log(o.f()); // "stone"
```

如上代码所示，对象 o 中包含一个属性 name 和一个方法 f()。当我们执行 o.f() 时，方法 f() 中的 this 指代调用函数的那个对象，也就是对象 o，所以 this.name 也就是 o.name。


注意，在何处定义函数完全不会影响到 this 的行为，我们也可以首先定义函数，然后再将其附属到 o.f。这样做 this 的行为也一致。如下代码所示：
```js
var fun = function() {
    return this.name;
};
var o = { name: "stone" };
o.f = fun;
console.log(o.f()); // "stone"
```
类似的，this 的绑定只受最靠近的成员引用的影响。在下面的这个例子中，我们把一个方法 g() 当作对象 o.b 的函数调用。在这次执行期间，函数中的 this 将指向 o.b。事实上，这与对象本身的成员没有多大关系，最靠近的引用才是最重要的。

```js
o.b = {
    name: "sophie"
    g: fun,
};
console.log(o.b.g()); // "sophie"
```

**eval() 方法中的 this**

eval() 方法可以将字符串转换为 JavaScript 代码，使用 eval() 方法时，this 指向哪里呢？答案很简单，看谁在调用 eval() 方法，调用者的执行环境中的 this 就被 eval() 方法继承下来了。如下代码所示：

```js
// 全局上下文
function f1(){
    return eval("this");
}
console.log(f1() === window); // true

// 函数上下文
var o = {
    name: "stone",
    f: function() {
        return eval("this.name");
    }
};
console.log(o.f()); // "stone"
```


**call() 和 apply() 方法中的 this**

call() 和 apply() 是函数对象的方法，它的作用是改变函数的调用对象，它的第一个参数就表示改变后的调用这个函数的对象。因此，this 指代的就是这两个方法的第一个参数。

```js
var x = 0;　　
function f() {　　　　
    console.log(this.x);　　
}　　
var o = {};　　
o.x = 1;
o.m = f;　　
o.m.apply(); // 0
```

call() 和 apply() 的参数为空时，默认调用全局对象。因此，这时的运行结果为 0，证明 this 指的是全局对象。如果把最后一行代码修改为：

```js
o.m.apply(o); // 1
```

运行结果就变成了 1，证明了这时 this 指代的是对象 o。

**bind() 方法中的 this**

ECMAScript 5 引入了 Function.prototype.bind。调用 f.bind(someObject) 会创建一个与 f 具有相同函数体和作用域的函数，但是在这个新函数中，this 将永久地被绑定到了 bind 的第一个参数，无论这个函数是如何被调用的。如下代码所示：

```js
function f() {
    return this.a;
}

var g = f.bind({
    a: "stone"
});
console.log(g()); // stone

var o = {
    a: 28,
    f: f,
    g: g
};
console.log(o.f(), o.g()); // 28, stone
```


**DOM 事件处理函数中的 this**

一般来讲，当函数使用 addEventListener，被用作事件处理函数时，它的 this 指向触发事件的元素。如下代码所示：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <meta charset="UTF-8">
        <title>test</title>
    </head>
    <body>
        <button id="btn" type="button">click</button>
        <script>
            var btn = document.getElementById("btn");
            btn.addEventListener("click", function(){
                this.style.backgroundColor = "#A5D9F3";
            }, false);
        </script>
    </body>
</html>
```

但在 IE 浏览器中，当函数使用 attachEvent ，被用作事件处理函数时，它的 this 却指向 window。如下代码所示：

```html
<!DOCTYPE HTML>
<html>
    <head>
        <meta charset="UTF-8">
        <title>test</title>
    </head>
    <body>
        <button id="btn" type="button">click</button>
        <script>
            var btn = document.getElementById("btn");
            btn.attachEvent("onclick", function(){
                console.log(this === window);  // true
            });
        </script>
    </body>
</html>
```


**内联事件处理函数中的 this**

当代码被内联处理函数调用时，它的 this 指向监听器所在的 DOM 元素。如下代码所示：

```html
<button onclick="alert(this.tagName.toLowerCase());">
  Show this
</button>
```


上面的 alert 会显示 button，注意只有外层代码中的 this 是这样设置的。如果 this 被包含在匿名函数中，则又是另外一种情况了。如下代码所示：

```html
<button onclick="alert((function(){return this})());">
  Show inner this
</button>
```

在这种情况下，this 被包含在匿名函数中，相当于处于全局上下文中，所以它指向 window 对象。


### 执行上下文
