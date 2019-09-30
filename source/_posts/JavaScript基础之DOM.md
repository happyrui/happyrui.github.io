---
title: JavaScript基础之DOM
date: 2019-09-30 13:09:17
tags: 
- JavaScript
categories: 
- web前端
- JS基础
---

![](https://img-blog.csdnimg.cn/2018111317293160.png)

## 基础DOM
### DOM概述
**Document Object Model ，就是 文档对象模型**

大家应该已经对HTML标签了解了，其实DOM和标签之间的关系密不可分，html标签通过浏览器解析成DOM节点。html标签包裹的内容展示在页面上，行为操作是需要DOM来完成的。一个页面有很多的html标签，那么就对应有很多DOM节点。这么多的DOM节点根据父子级的关系构成DOM树。
<!-- more -->
### 节点类型

![](https://img-blog.csdn.net/20170821121034602)

在这里我们就只关注最常用的三种：元素、属性、文本。

+ 元素节点（Element）: html的标签 <p></p>
+ 属性节点（Attribute）:元素属性 比如设置的id
+ 文本节点（Text）: 页面上需要展示的内容

这些节点的属性也都是通用的。有

+ nodeType(返回节点的属性值)，
+ nodeName(返回节点的名称)，
+ nodeValue(返回或设置当前节点的值)。

``` js
<div id='main'><p>这是节点1</p><!-- 注释 -->hello</div>  <!-- 第一级child节点 -->
<script type="text/javascript">
	var divCon = document.getElementById('main');
	var con1 = document.getElementsByTagName('p');
	//空格和换行符会被解释成节点
	for (var i = 0; i < divCon.childNodes.length; i++) { 
		console.log('-------------')   
        console.log("nodeType:"+divCon.childNodes[i].nodeType+"");  
        console.log("nodeName:"+divCon.childNodes[i].nodeName+""); 
        console.log("nodeValue:"+divCon.childNodes[i].nodeValue+"");                   
    } 
</script>

```

如果只想要返回元素节点的长度可以用： 

ele.childElementCount 方法或者 ele.children.length方法

**注意： IE浏览器只能使用nodeType 是否等于某个数值 来判断节点类型**

### 属性

可以进行获取或设置某个节点的属性值。

+ innerHTML 节点（元素）的文本值，获取/设置节点内容
+ parentNode 节点（元素）的父节点
+ childNodes 节点（元素）的子节点
+ attributes 节点（元素）的属性节点
+ style 修改节点样式

当然，还有上述的nodeType，nodeName，nodeValue 属性

**属性操作：（也算是方法）**

| 方法 | 表述 |
| ---- | ---- |
| getAttribute(name) | 返回指定的属性值 |
| setAttribute(name,value) | 把指定属性设置或修改为指定的值。 |
| removeAttribute(name) | 删除属性 |

``` js
<div id='main'><p>这是节点1</p><!-- 注释 -->hello</div>  <!-- 第一级child节点 -->
<script type="text/javascript">
   var divCon = document.getElementById('main');
   var con1 = document.getElementsByTagName('p');
   console.log(divCon.innerHTML)   // <p id="con1">这是节点1</p><!-- 注释 -->hello
   divCon.innerHTML+=' world'
   console.log(con1[0])  // <p>这是节点1</p>
   console.log(con1[0].parentNode)  // 不能直接使用 con1 需要指定是哪一个元素的父元素。
   console.log(divCon.childNodes) // 数组  共有三个值，空格和注释都是节点
   console.log(divCon.attributes)  // id
   console.log(con1.attributes)  //  undefined  没有属性
   con1[0].style.color = 'red';   // 通过 . 运算符操作
</script>

```

### 方法

| 方法 | 表述 | 返回类型 |
| ---- | ---- | ---- |
| getElementById() | 返回指定的属性值 | object |    
| getElementsByTagName() | 把指定属性设置或修改为指定的值。 | 数组 |
| getElementsByClassName() | 返回包含带有指定类名的所有元素的节点列表。 | 数组 |
| appendChild(node) | 把新的子节点添加到指定节点 |  |    
| removeChild(node) | 删除子节点 |  |    
| replaceChild(node1,node2) | 替换子节点。node1替换node2 |  |    
| insertBefore(node1,node2) | 在指定的子节点前面插入新的子节点。n1插在n2前 |  |    
| createAttribute(node) | 创建属性节点。 |  |    
| createElement(node) | 创建元素节点。 |  |  
| createTextNode(node) | 创建文本节点。 |  |  


``` js
var con2 = document.createElement('h1');
var con2Attr = document.createAttribute('class');
con2Attr.value = 'con2'
con2.setAttributeNode(con2Attr)
var con2Content = document.createTextNode('这是一级标题');
con2.appendChild(con2Content);

con1[0].appendChild(con2);
console.log(con2);   // <h1 class='con2'>这是一级标题</h1>
// con1[0].removeChild(con2);  //  去掉刚添加的节点

var con3 = document.createElement('a');
var con3Attr = document.createAttribute('href');
var con3Name = document.createTextNode('Baidu')
con3Attr.value='http://baidu.com'
con3.setAttributeNode(con3Attr)
con3.appendChild(con3Name);
console.log(con3);
con1[0].replaceChild(con3,con2);  // con3 替换了con2

con1[0].insertBefore(con2,con3);   // 在con3前插入con2


```

**ps：实现一个insertAfter方法**

``` js
function insertAfter(a1,a2){
	 var parent = a2.parentNode;
     if(parent.lastChild == a2) {
           ​parent.appendChild(a1);
     }​
     else {
          parent.insertBefore(a1, a2.nextSibling);​
    }​
}


```