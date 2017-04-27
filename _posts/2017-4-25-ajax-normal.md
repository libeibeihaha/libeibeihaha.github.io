---
layout: post
title:  "ajax基础详解"
date:   2017-04-25 03:06:05
categories: Ajax
---


* content
{:toc}



## Ajax简介
&nbsp;&nbsp;ajax不是一项具体的技术，而是javascript.XHTML和CSS.DOM.XML和XMLHttpRequest等多门技术的综合应用。
&nbsp;&nbsp;作用:可以再用户浏览网页的同事与服务器进行异步交互和实现网页内容的局部更新。
&nbsp;&nbsp;核心:程序不通过浏览器发送请求，而是调用javascript中的类XMLHttpRequest对象来发送请求，再由这个Jacascript对象接受响应，并将响应结果用DOM编程方式挂到原来的网页上。    
**同步与异步**  
(1) 同步:是指发送方发出数据后，等接收方回响应以后才发下一个数据包的通讯方式。  
&nbsp;&nbsp;&nbsp;流程:提交请求->等待服务器处理->处理完毕(这期间服务器不能干任何事)  
(2) 异步:是指发送方发出数据后，不等接收方发回响应，接着发送下个数据包的通讯方式。
&nbsp;&nbsp;&nbsp;流程:请求数据事件触发->服务器处理(这是浏览器仍然可以做其他事情)->处理完毕    
## Ajax框架的基本流程:  
### 初始化XMLHttpRequest对象  
 XMLHttpRequest最早是在IE5以ActiveX组件的形式实现的，非W3C标准,所以XMLHttpRequest在不同浏览器上的实现方法不统一,但在不同浏览器上的实现事兼容的。
#### XMLHttpRequest对象属性:
(1)onreadystatechange:通信状态的事件触发器,每次readyState属性的改变都会触发readystatechange事件。  
(2)readyState:通信状态,值为数值。readyState值得变化会因浏览器的不同而有所差异。但是，当请求结束的时候，每个浏览器都会把readyState的值设为4。  
**readyState的值:**    
0 ---未初始化(open方法还未调用)  
1 ---正在加载(send方法还没有被调用)  
2 ---以加载完毕(请求已经开始)  
3 ---交互中(服务器正在发送相应)  
4 ---完成(响应发送完毕)    
(3)status:服务器返回的状态码。
常用status状态码及其含义:    
404 ---没找到页面(not found)  
403 ---禁止访问(forbidden)  
500 ---内部服务器出错(internal service error)  
200 ---一切正常(ok)  
304 ---请求的资源没有被修改，可以直接使用浏览器中缓存的版本。  
(4)responseText:从服务器发送的响应数据  
**注意：只有当readyState属性值变成4时，responseText属性才可用，表明Ajax请求已经结束。**  
(5)responseXML：如果服务器返回的XML，那么数据将存储在responseXML属性中。  
**注意： 只用服务器发送了带有正确首部信息的数据时， responseXML 属性才是可用的。所以，修改 MIME 类型为 text/xml  (即contentType="text/xml")**  
(6)statusText:服务器返回的状态文本信息   
#### XMLHttpRequest对象方法   
(1)abort();停止当前请求  
(2)getAllResponseHeaders(); -- 把http请求的所有响应首部作为键/值对返回  
(3)getResponseHeader('headerLabel'); -- 返回制定首部的串值     
(4)open("method","url",asynch) -- 建立对服务器的调用    
**注意:**  
a. 如只是想要从服务器检索一个文件，而不需要发送任何数据，使用GET（可以再GET请求里通过附加在URL上的查询字符串发送数据，不过数据限制在2000个字符）。若需要向服务器发送数据，用POST。    
b. 如果以POST方式请求，必须先调用setRequestHeader方法，修改MIME类别。  **eg:****xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");**   
(5)send(data); -- 向服务器发送数据。若采用的GET请求方式,send(null)(即使send非空值，服务器也接受不到)。   
#### XMLHttpRequest对象初始化代码：
```js  
 function crateXHR()
{   
   if(typeof XMLHttpRequest!="undefined")
   {
      return new XMLHttpRequest();
   }
   else if(typeof ActiveXObject!="unfined")
   {
      if(typeof arguments.callee.activeXString!="string")
      {
         var versions=["MSXML2.XMLHttp.6.0","MSXML2.XMLHttp3.0","MSXML2.XMLHttp"],i,len;
         for(i=0,len=versions.length;i<len;i++)
         {
              try{
                    new ActiveXObject(version(i));
                    arguments.callee.activeXString=version[i];
                     break;
                 }catch(e){}               
         }
      }
     return new ActiveXObject(arguments.callee.activeXString);
   }
   else
   {
     throw new Error("NO XHR object available");
   }
}  
```    
### 指定响应处理函数
(1) 首先，它要检查XMLHttpRequest对象的readyState值，判断目前的通信状。  
```js
     if (http_request.readyState == 4)
   {                                
       // 信息已经返回，可以开始处理                          
    } else {
       // 信息还没有返回，等待
            }
```  
(2)  其次，还需要判断返回的HTTP状态码，确定返回的页面没有错误。
```js
    if (http_request.status == 200) 
   {
        // 页面正常，可以开始处理信息
    } else {
            // 页面有问题
           }
```   






