---
title: JavaScript基础之事件
date: 2019-10-10 19:08:48
tags: 
- JavaScript
categories: 
- web前端
- JS基础
---
### 事件模型

+ 事件冒泡：事件按照从最特定的事件目标到最不特定的事件目标(document对象)的顺序触发，**子级元素先触发**
+ 事件捕获：事件从最不精确的对象(document对象)开始触发，然后到最精确(也可以在窗口级别捕获事件，不过必须由开发人员特别指定)，**父级元素先触发**
+ 事件捕获阶段：事件从最上一级标签开始往下查找，直到捕获到事件目标(target)。
+ 事件冒泡阶段：事件从事件目标(target)开始，往上冒泡直到页面的最上一级标

事件模型的各个阶段  

    捕获阶段-目标阶段-冒泡阶段
<!-- more -->    
### 事件对象

DOM事件模型中的事件对象常用属性:

+ type用于获取事件类型
+ target获取事件目标
+ stopPropagation()阻止事件冒泡
+ preventDefault()阻止事件默认行为


IE事件模型中的事件对象常用属性:

+ type用于获取事件类型
+ srcElement获取事件目标
+ cancelBubble = true 阻止事件冒泡
+ returnValue = false 阻止事件默认行为

W3C模型是将两者进行中和，在W3C模型中，任何事件发生时，先从顶层开始进行事件捕获，直到事件触发到达了事件源元素。然后，再从事件源往上进行事件冒泡，直到到达document。
IE只支持事件冒泡，不支持事件捕获，它也不支持addEventListener函数，不会用第三个参数来表示是冒泡还是捕获，它提供了另一个函数attachEvent。
ele.attachEvent("onclick", doSomething2);

使用jquery,既停止冒泡又阻止默认行为
```js
$("#testC").on('click',function(){
    return false;
});
```

ele.addEventListener('click',doSomething2,true)
true=捕获   false=冒泡

### 事件委托

事件委托就是利用事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件

页面上有这么一个节点树，div>ul>li>a;比如给最里面的a加一个click点击事件，那么这个事件就会一层一层的往外执行，执行顺序a>li>ul>div，有这样一个机制，那么我们给最外面的div加点击事件，那么里面的ul，li，a做点击事件的时候，都会冒泡到最外层的div上，所以都会触发，这就是事件委托，委托它们父级代为执行事件

xs.on事件=function(){}  //相当于写一个函数。

处理兼容性问题
```js
var ev = ev || window.event;
var target = ev.target || ev.srcElement;
```

```js
window.onload = function(){
    var Oul = document.getElementById('oul');
    //var Oul = document.getElementsByTagName('ul');
    //想要使用addEventListener，必须是dom元素，获取的是id值
    Oul.addEventListener('click',function(ev){
        var ev = ev || window.event;
        var target = ev.target || ev.srcElement;
        //不能直接使用target,必须有所转换再去匹配
        if(target.nodeName.toLowerCase() == "li"){
            alert(target.innerHTML);
        }
    },false);
//  Oul.onclick= function(){
//      alert('aaa')
//  }
}
```
