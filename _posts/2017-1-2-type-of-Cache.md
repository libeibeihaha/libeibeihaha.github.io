---
layout: post
title:  "各类缓存详解"
date:   2017-01-2 00:06:05
categories: HTML
---


* content
{:toc}

## 1.Application Cache介绍
 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;html5引入了应用程序缓存技术,意味着web应用可进行缓存，并在没有网络的情况下使用，通过创建cache manifest文件，可以轻松的创建离线应用。

&nbsp;&nbsp;Application Cache带来的三个优势是：  
① 离线浏览  
② 提升页面载入速度  
③ 降低服务器压力，而且主要浏览器器都支持Application Cache.   
Application Cache简介:   
    Application Cache的使用要做两方面的工作：

    ① 服务器端需要维护一个manifest清单

    ② 浏览器上只需要一个简单的设置即可


manifest文件是简单的文本文件，他告知浏览器被缓存的内容*(以及不被缓存的内容).manifest文件可分为3个部分:  
**CACHE MANIFEST**-在此标题下列出的文件将在首次下载后进行缓存             
**NETWORK**-在此标题下列出的文件需要与服务器的连接，且不会被缓存      
**FALLBACK**-在此标题下列出的文件规定当页面无法访问时的回退页面(比如404页面)    
**更新缓存**  
一旦应用被缓存，他就会保持缓存直到发生下列情况:  
(1)用户清空缓存  
(2)manifest文件被修改  
(3)有程序来更新应用缓存  
目前为止实现了离线缓存，用在静态网页或者游戏比较好用。 
  
## 2.LocalStorage和SessionStorage操作

&nbsp;&nbsp;
&nbsp;&nbsp;
&nbsp;&nbsp;localStorage和sessionStorage都具有相同的操作方法，例如setItem、getItem和removeItem等   
**localStorage和sessionStorage的方法**：    
**setItem存储value**  
用途：将value存储到key字段  
用法：.setItem( key, value)  
代码示例：  
```js
sessionStorage.setItem("key", "value");  
localStorage.setItem("site", "js8.in");
```
**getItem获取value**   
用途：获取指定key本地存储的值  
用法：.getItem(key)  
代码示例：  
```js
var value = sessionStorage.getItem("key"); 
var site = localStorage.getItem("site");
```
**removeItem删除key**   
用途：删除指定key本地存储的值  
用法：.removeItem(key)  
代码示例：
```js
sessionStorage.removeItem("key"); 
localStorage.removeItem("site");
```
**clear清除所有的key/value**  
用途：清除所有的key/value  
用法：.clear()  
代码示例：  
```js
sessionStorage.clear(); 
localStorage.clear();  
```  
## 3.cookie详解

**(1)什么是cookie?**    
&nbsp;&nbsp;&nbsp;&nbsp;从web 角度 cookie是用于存储信息为下次访问 或者 与服务器之间传递信息的，它可以记住用户上次操作了什么，操作到哪里了，为智能的操作提供基础。它有自己的操作方式，有自己的大小，不过它的操作方式不太方便，可以依赖第三方库.  
**(2)cookie的基础知识点**:  
它有大小限制，一般不超过4kb,超出的将不再保存。     
cookie 是不安全的，能通过查看cookies 或者 与服务器传递的header   中看到，所以不能报存重要的私密信息，如 用户的密码。  
cookie 有自己的过期时间，可以用它来清除cookie。  
cookie 保存的是字符串，多个cookies 之间用 ‘; ’分割，注意分号后还有一个空格，cookie 的key和value之间是“=”相连。  
cookie 有其他高级存储方式如：path，domain，secure；   
cookie,sessionStorage,localStorage的区别
cookie是存储在浏览器端，并且随浏览器的请求一起发送到服务器端的，它有一定的过期时间，到了过期时间自动会消失。sessionStorage和localeStorage也是存储在客户端的，同属于web Storage，比cookie的存储大小要大有8m，cookie只有4kb，localeStorage是持久化的存储在客户端，如果用户不手动清除的话，不会自动消失，会一直存在，sessionStorage也是存储在客户端，但是它的存活时间是在一个回话期间，只要浏览器的回话关闭了就会自动消失。

## 4.sessionStorage 、localStorage 和 cookie 之间的区别

**共同点**：  
都是保存在浏览器端，且同源的。区别：cookie 数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递。而sessionStorage和 localStorage不会自动把数据发给服务器，仅在本地保存。cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径 下。存储大小限制也不同，cookie数据不能超过4k，同时因 为每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识。sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localStorage 在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的。Web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。Web Storage 的 api 接口使用更方便。

**sessionStorage与页面 js 数据对象的区别**  
&nbsp:页面中一般的 js 对象或数据的生存期是仅在当前页面有效，因此刷新页面或转到另一页面这样的重新加载页面的情况，数据就不存在了。而sessionStorage 只要同源的同窗口（或tab）中，刷新页面或进入同源的不同页面，数据始终存在。也就是说只要这个浏览器窗口没有关闭，加载新页面或重新加载，数据仍然存在。  
&nbsp:cookie，容量4kb，默认各种浏览器都支持，缺陷就是每次请求，浏览器都会把本机存的cookies发送到服务器，无形中浪费带宽。
userdata，只有ie支持，单个容量64kb，每个域名最多可存10个共计640k数据。默认保存在C:\Documents and Settings\Administrator\UserData\目录下，保存格式为xml。关于userdata更多资料参考http://msdn.microsoft.com/library/default.asp?url=/workshop/author/behaviors/reference/behaviors/userdata.asp  
**sessionStorage与localStorage**  
&nbspWeb Storage实际上由两部分组成：sessionStorage与localStorage。
sessionStorage用于本地存储一个会话（session）中的数据，这些数据只有在同一个会话中的页面才能访问并且当会话结束后数据也随之销毁。因此sessionStorage不是一种持久化的本地存储，仅仅是会话级别的存储。
localStorage用于持久化的本地存储，除非主动删除数据，否则数据是永远不会过期的。

**为什么选择Web Storage而不是Cookie？**    
与Cookie相比，Web Storage存在不少的优势，概括为以下几点：  
1. 存储空间更大：IE8下每个独立的存储空间为10M，其他浏览器实现略有不同，但都比Cookie要大很多。  
2. 存储内容不会发送到服务器：当设置了Cookie后，Cookie的内容会随着请求一并发送的服务器，这对于本地存储的数据是一种带宽浪费。而Web Storage中的数据则仅仅是存在本地，不会与服务器发生任何交互。  
3. 更多丰富易用的接口：Web Storage提供了一套更为丰富的接口，使得数据操作更为简便。  
4. 独立的存储空间：每个域（包括子域）有独立的存储空间，各个存储空间是完全独立的，因此不会造成数据混乱  
 

