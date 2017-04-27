---
layout: post
title:  "垃圾回收机制和内存泄漏"
date:   2017-04-26 03:06:05
categories: javaScript
---


* content
{:toc}      



## 垃圾收集 
javascript具有自动垃圾收集机制，也就是说，执行环境会负责管理代码执行过程中的使用的内存。而在C和C++之类的语言中，开发人员的一项基本任务就是手动跟踪内存的使用情况，这是造成许多问题的一个根源。在编写javascript程序时候，开发人员不用再关心内存使用的问题，所需内存的分配以及无用的回收完全实现了自动管理。这种垃圾收集机制的原理其实很简单：找出那些不再继续使用的变量，然后释放其中占用的内存。为此，垃圾收集器会按照固定的时间间隔（或代码执行中预设的收集时间），周期性的执行这一操作。  

### 标记清除      
javascript中最常用的垃圾收集方式是标记清除(mark-and-sweep)。当变量进入环境（例如，在函数中声明一个变量）时，就将这个变量标记为“进入环境”。从逻辑上讲，永远不能释放进入环境的变量所占的内存，因为只要执行流进入相应的环境，就可能用到它们。而当变量离开环境时，这将其标记为“离开环境”      

### 引用计数    
引用计数的含义是跟踪记录每个值被引用的次数。当声明一个变量并将引用类型的值赋给该变量时，则这个值的引用次数就是1。如果同一个值又被赋给另一个变量，则该值的引用次数加1.相反，如果包含对这个值引用的变量又取得另外一个值，则这个值的引用次数减1.当这个值的引用次数变成0时，则说明没有办法访问这个值了，因此就可以将其占用的内存空间回收回来。这样当垃圾收集器下次再运行时，它就会释放那些引用次数为零的值所占用的内存。  
我们知道，IE中有一部分对象并不是原生javascript对象。例如，其中BOM和DOM中的对象就是使用C++以COM (Component Object Model，组件对象模型)对象的形式实现的，而COM对象的垃圾收集机制采用的就是引用计数策略。因此，即使IE的javascript引擎是使用标记清除策略来实现的，但javascript访问的COM对象依然是基于引用计数策略的。换句话说，只要IE中设计COM对象，就会存在循环引用的问题。下面这个简单的例子，展示了使用COM对象导致的循环引用问题：   
```js
  var element = document.getElementById("some_element");
  var myObject = new Object();
  myObject.element = element;
  element.somObject = myObject; 
```    
这里例子在一个DOM元素(element)与一个原生的javascript对象(myObject)之间创建了循环引用。其中，变量myObject有一个名为element的属性指向element对象；而变量element也有一个属性名叫someObject回指myObject。由于存在这个循环引用，即使将例子中的DOM从页面中移除，它也永远不会被回收。

为了避免类似这样的循环引用问题，最好是不使用他们的时候手工断开原生javascript对象与DOM元素之间的连接。   
### 常见的内存泄漏的原因   
#### 全局变量引起的内存泄漏
```js
    function leaks(){  
    leak = 'xxxxxx';//leak 成为一个全局变量，不会被回收
}
```  
#### 闭包引起的内存泄漏
```js
    var leaks = (function(){  
    var leak = 'xxxxxx';// 被闭包所引用，不会被回收
    return function(){
        console.log(leak);
    }
   })()
```
#### dom清空或删除时，事件未清除导致的内存泄漏    
```js
<div id="container">
</div>
 $('#container').bind('click', function(){
    console.log('click');
}).remove();  
// zepto 和原生 js下，#container dom 元素，还在内存里jquery 的 empty和 remove会帮助开发者避免这个问题  
<div id="container">  
</div>
$('#container').bind('click', function(){
    console.log('click');
}).off('click').remove();    
```                              
把事件清除了，即可从内存中移除




