---

layout: post
title:  "前端面试知识总结2"
date:   2017-03-4 09:04:40
categories: 前端面试知识总结
---


* content
{:toc}


## CSS 
### 盒子模型 
盒子模型包含元素内容，内边距，边框，外边框几个要素。  
IE盒子模型的范围也包括这些，和标准W3C盒子模型不同的是：IE盒子模型的content部分包含了border+padding         
### box-sizing常用的属性有哪些，分别有什么作用
**box-sizing**:content-box|border-box|inherit;   
选择content-box:根据W3C盒模型布局    
选择border-box:根据IE盒模型来布局。  
### Doctype作用,标准模式和兼容模式各有什么区别 
(1)<!DOCTYPE>告知浏览器的解析器用什么文档标准来解析这个文档。DOCTYPE不存在或者格式不正确会导致文档以兼容模式出现。  
(2)标准模式的排版和JS运作模式都是以该浏览器的最高标准运行。在兼容模式中，页面会以宽松的向后兼容的方式来显示，模拟老式浏览器的行为以防止站点无法工作。     
### 介绍一下你对浏览器内核的理解    
主要分成两个部分：渲染引擎和JS引擎  
**渲染引擎**：负责获取网页的内容，整理讯息，以及计算网页的显示方式。浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不同。      
**JS引擎**：解析和执行JavaScript来实现网页的动态效果。  
目前内核倾向于只指渲染引擎。 
## HTML5 
### HTML5的新特性，以及如何解决HTML5新标签的浏览器兼容性问题。  
HTML5现在已经不是SGML的子集了，主要是关于图像。存储，多任务等功能的增加。    
* 绘画canvas   
* 用于媒介回放的video和audio元素   
* 本地离线存储长期存储数据，浏览器关闭后数据不丢失。   
* sessionStroage的数据在浏览器关闭后自动删除   
* 语义化更好的标签，比如artical,footer,header,nav,section;  
* 表单控件,calendar,data,time,email,url,search  
* 新的技术：webworker,websocket,Geolocation    s
IE8/IE7/IE6不支持通过document.createElement方法产生的标签，可以使用成熟的框架比如html5shiv;



