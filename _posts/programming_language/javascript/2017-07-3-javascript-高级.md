---
layout: post
title: javascript 高级
categories: web前端 编程语言 javascript
description: javascript 高级
keywords: Web前端, javascript
---

## 作为命名空间的函数
### 问题产生及分析
如果我定义了一个函数`t_func`，如下:
~~~ javascript
function t_func(){
    alert('This is t_func function);    #1
}
~~~
但这时我并不知道在此之前已经定义过了 `t_func`，也许是在其他文件中定义了这个函数，在多人协作时很容易出现这种
情况。如下：
~~~ javascript
// 之前定义过的 t_func 函数
function t_func(){
    alert("I'm the real t_func");   #2
}
~~~
这时我调用`t_func`，本意是想执行 `#2`，但实际上却执行的是`#1`。  这是因为`t_func`的两次定义其实都是在全局作用域内，
所有之前定义的`t_func`被重新定义了(污染到了全局作用域)。

### 解决办法
javascript只有函数作用域，函数之外是全局作用域。为了不再污染全局作用域，可以将函数作用域作为命名空间使用。如下:
~~~ javascript
(function(){
    this.t_func = function(){};
    windown.ff = this
}());
// 再使用t_func时可以使用下面方式
ff.t_func();
~~~

## 作用域链

## 闭包

## 原型链

## 方法链

## 面向对象