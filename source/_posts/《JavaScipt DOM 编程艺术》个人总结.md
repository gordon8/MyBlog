---
title: 《JavaScipt DOM 编程艺术》个人总结
date: 2016-11-01 17:47:13
tags: 
- 前端
- JavaScript
- DOM

---

## 概述

### JavaScript定义
JavaScript ( JS ) 是一种轻量级、解释型、基于原型、多范式的动态脚本语言，并且支持面向对象、命令式和声明式（如：函数式编程）编程风格。其主要包含三部分：ECMAScript、BOM和DOM。
1. ECMAScript-(European Computer Manufacturers Association):描述了该语言的语法和基本对象。
2. BOM-(Browser Object Model)浏览器对象模型:描述与浏览器进行交互的方法和接口。
3. DOM-(Document Object Model)文档对象模型:描述处理网页内容的方法和接口。

### 本书精髓
本书只介绍最基本的JavaScript语法，把重心主要放在介绍DOM编程技术背后的思路和原则。本书的理念精髓为：平稳退化、渐进增强、以用户为中心。

<!-- more -->

## DOM 方法和属性

### DOM 常用方法
1. 创建节点
   * 创建元素节点 document.createElement(element)
   * 创建文本节点 document.createTextNode(text)
2. 复制节点
   node.cloneNode(deep)
   若需克隆所有后代，请把 deep 参数设置 true，否则设置为 false。
3. 插入节点
    * 插入子节点 element.appendChild(newChild) 
      给定子节点newchild 将成为给定元素节点element 的最后一个子节点。
    * 插入兄弟节点 element.insertBefore(newNode, targetNode)
      节点newNode将被插入元素节点element 并出现在节点targetNode的前面。节点targetNode必须是element元素的一个子节点。如果targetNode节点未给出，newNode节点就将被追加为element元素的最后一个子节点。
    * 插入内容 HTMLElementObject.innerHTML=text 
      用来设置或返回元素的innerHTML。
4. 删除节点
   element.removeChild(node) 该方法的返回值是一个指向已被删除的子节点的引用指针。当某个节点被removeChild()方法删除时，这个节点所包含的所有子节点将同时被删除。若想把某个节点从文档的一个部分移动到另一个部分，你不必使用removeChild()方法。appendChild()和insertBefore()方法都自动地先删除这个节点再把它重新插入到新位置去。
5. 替换节点
   element.replaceChild(newChild, oldChild)
   oldchild节点必须是element元素的一个子节点。它的返回值是一个指向已被替换的那个子节点的引用指针。replaceChild()方法也可以用文档树上的现有节点去替换另一个现有节点。如果newchild节点是文档树上的一个现有节点，replaceChild()方法将先删除它再用它去替换oldchild节点。
6. 设置属性节点
   element.setAttribute(attributeName, attributeValue)
   属性的名字和值必须以字符串的形式传递给此方法。如果这个属性已经存在，它的值将被刷新；如果不存在，setAttribute()方法将先创建它再为其赋值。setAttribute()方法只能用在属性节点上。
7. 查找节点
   * 查找属性节点 element.getAttribute(attributeName) 给定属性的名字必须以字符串的形式传递给这个方法。给定属性的值将以字符串的形式返回。如果给定属性不存在，getAttribute()方法将返回一个空字符串。
   * 查找元素节点
     1. document.getElementById(ID) 方法将返回一个有着给定id 属性值的元素节点。如果不存在这样的元素，它返回null。这个方法只能用于document对象。该方法返回的元素节点是一个对象，这个对象有着nodeName、nodeType、parentNode、childNodes 等属性。
     2. document.getElementsByName(name) 该方法与 getElementById()方法相似，但是它查询元素的 name 属性，而不是 id 属性。
     3. element.getElementsByTagName(tagname) 该方法将返回一个节点集合，这个集合可以当作一个数组来处理。这个集合的length 属性等于当前文档里有着给定标签名的所有元素的总个数。这个数组里的每个元素都是一个对象，它们都有着nodeName、nodeType、parentNode、childNodes 等属性。该方法不必非得用在整个文档上。它也可以用来在某个特定元素的子节点当中寻找有着给定标签名的元素。
     4. element.getElementsByClassName(names) 该方法与 getElementsByTagName()方法相似，但是它查询元素的 class 属性，而不是 tag 名。
   * 检查一个给定元素是否有子节点 element.hasChildNodes 该方法通常与if语句配合使用。如果这个元素有子节点，就把它们以一个数组保存到变量中。若hasChildNodes 方法返回false，给定元素的firstchild 和lastchild 属性也将为空。

### DOM 属性

1. 节点的属性
   * 节点名 node.nodeName 
     只读属性，若给定节点是一个元素节点，nodeName属性将返回这个元素的名字（大写字母形式），这在效果上相当于tagName属性。若给定节点是一个属性节点，nodeName属性将返回这个属性的名字。若给定节点是一个文本节点，nodeName属性将返回一个内容为#text的字符串。
   * 节点类型 node.nodeType
     只读属性，共存在12种不同的节点类型，其中1~3非常重要：
     1. 元素 ELEMENT_NODE
     2. 属性 ATTRIBUTE_NODE
     3. 文本 TEXT_NODE
     通常与if语句配合使用,以确保不会在错误的节点类型上执行无效或非法的操作。
   * 节点值 node.nodeValue
     该属性将返回一个字符串。若给定节点是一个属性节点，nodeValue属性将返回这个属性的值。若给定节点是一个文本节点，nodeValue属性将返回这个文本节点的内容。若给定节点是一个元素节点，nodeValue属性将返回null。该属性是一个读/写属性。不过你不能为元素节点的nodeValue属性设置一个值。
2. 遍历节点树
   * 子节点 node.childNodes
     只读属性，将返回一个数组，这个数组由给定元素节点的子节点构成。
   * 第一个子节点 firstChild
     只读属性，返回一个节点对象的引用指针。
   * 最后一个子节点 lastChild
     只读属性，返回一个节点对象的引用指针。
   * 下一个子节点 nextSibling 
     只读属性，返回一个节点对象的引用指针。若给定节点的前面没有同属一个父节点的节点，它的nextSibling属性将返回null。
   * 父节点 parentNode
     只读属性，属性返回的节点永远是一个元素节点，因为只有元素节点才有可能包含子节点。唯一的例外是document节点，它的parentNode属性将返回null。
   * 前一个子节点 previousSibling
     只读属性，返回一个节点对象的引用指针。若给定节点的前面没有同属一个父节点的节点，它的previousSibling属性将返回null。

## 重点总结

### 最佳实践
1. 平稳退化（优雅降级）
   首先使用最新的技术面向高级浏览器构建最强的功能及用户体验，然后针对低级的浏览器进行限制，逐步衰减那些无法被支持的功能及体验。使用平稳退化技术时，你必须首先完整的实现了网站，其中包括所有的功能和特效。 然后再为那些无法支持所有功能的浏览器增加候选方案，使之在旧市的浏览器上可以以某种形式降级体验却不至于完全失效。
2. 渐进增强
   渐进增强的概念就是，指从最基本的可用性出发，在保证站点页面在低级浏览器中的可用性和可访问性的基础上，逐步增强功能及提高用户体验。渐进增强方案并不假定所有的用户都支持javascript，而总是提供一种候补方法，确保用户可以访问（主要的）内容。
3. 分离JavaScript
   尽量使网页结构、样式和JavaScript脚本的动作行为分离，使其各司其职。
4. 向后兼容性
   确保老版本的浏览器不会因为JavaScript脚本影响基本的正常工作。
5. 性能优化
   减少 HTTP请求数，减少DOM操作，减少作用域链查找等。

### Ajax 
1. 定义
   (Asynchronous JavaScript and XML)代表异步 JavaScript + XML, 本身不是一种技术, 是在2005年由 Jesse James Garrett 提出的一个术语, 描述了一种“新”的结合使用大量已经存在的技术的方式, 包括: HTML 或 XHTML, CSS, JavaScript, DOM, XML, XSLT, 还有最重要的 XMLHttpRequest 对象。
2. 主要优势
   * 对页面的请求以异步的方式发送到服务器，做到不用刷新网页，只更新页面中的一小部分。
   * 对XML文档的解析和处理。
3. 执行步骤 [详解](https://developer.mozilla.org/zh-CN/docs/AJAX/Getting_Started)
   1. 步骤 1 – "请!" --- 如何发送一个HTTP请求
   2. 步骤 2 – "收到!" --- 处理服务器的响应
   3. 步骤 3 – "万事俱备!" - 简单实例
   4. 步骤 4 – "X-档案" --- 处理XML响应

### 注意事项
1. JavaScript脚本只应该用来充实文档内容，应避免使用其来创建文档的核心内容。
2. 尽量采用更新元素className属性的方法来改变其样式，而不是直接更新其style对象的有关属性。
3. 实现动画效果的常用方法为将元素的position设为absolute,改变其top或left值。

## 实战总结 [demo](https://gordon8.github.io/demo/domsters/)

### 项目执行步骤
1. 规划站点结构
2. 规划网站页面整体结构
3. 设计样式
4. 用JavaScript增强网页体验

### 心得体会
1. 网站包含的所有页面风格一致，因此先创建HTML模板文档非常有必要
2. CSS文档要逻辑清晰
3. 创建JavaScript函数要尽量考虑将其抽象化，以增强其复用性。


   

