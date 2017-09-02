---
layout: post
title:  "css鼠标事件区别详解"
date:   2016-05-20 14:06:05
categories: css
tags: JavaScript 作用域 慕课网 ife
excerpt: JavaScript 作用域和作用域链学习笔记。
---

* content
{:toc}

## mouseenter和mouseover的区别 

使用mouseover/mouseout时，如果鼠标移动到子元素上，即便没有离开父元素，也会触发父元素的mouseout事件，
使用mouseenter/mouseleave时，如果鼠标没有离开父元素，在其子元素上任意移动，也不会触发mouseleave事件。

## 仿京东延迟和去抖动进行菜单栏优化
（1）切换子菜单时候，用setTimeout设置延迟 
（2）debounce去抖动技术：在事件被频繁触发时，只执行一次处理。方法：在计时器内部程序结束完以后清除计数器，在前面先判断有误计数器，如果有，表示程序没有执行，直接清除，这样可以保证总是最后一个计数器被执行。
### 引发的新问题，  
如果用户只切换一级菜单，那么会遇到延时。这样用户体验不是很好
#### 解决方案
这里可以给一个链接，之后解决


