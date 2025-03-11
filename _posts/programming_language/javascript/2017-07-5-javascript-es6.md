---
layout: post
title: javascript es6
categories: web前端 编程语言 javascript
description: javascript 高级
keywords: Web前端, javascript
---

> 文档 <http://es6.ruanyifeng.com>
> 本文作为阅读读书笔记，用来加强记忆

## 块级作用域
### 为什么需要块级作用域
ES5只有全局和函数作用域，没有块级作用域，这样就会出现一些不合理的场景。
~~~ javascript
// 场景一， 变量提升导致意外结果
var tmp = new Date();
function f(){
    console.log(tmp);
    if(false){
        var tmp = 'tmp';
    }
}
f(); // output: undefined
~~~
上面代码本意是想输出日期，但结果却是undefined。这是因为tmp在函数作用域内被定义，屏蔽了全局作用域的tmp，但出现了变量提升，在函数开始定义成
`var tmp;`,所以输出为undefined。

~~~ javascript
// 场景二， 用来计数的循环变量泄露为全局变量
for(var i = 0; i <= 10; i++){
    console.log(i);
}
console.log(i); // 这时变量i依然有效，被泄露为全局变量。
~~~

### ES6的块级作用域
1. 允许块级作用域的任意嵌套
2. 块级作用域内可声明函数。考虑到不同环境的行为差异，推荐使用函数表达式，而不是函数声明语句

## let命令
用来声明变量，但声明的变量只在代码块内有效。
~~~ javascript
{
 let a = 1;
 var b = 2;
}
console.log(b); // output: 2
console.log(a); // output: Uncaught ReferenceError: a is not defined
~~~
### 不存在变量提升
变量提升，即变量在声明之前可以使用。let命令改变了语法行为，使变量只能在声明之后才可以使用。

### 暂时性死区
在块级作用域中如果存在`let`或`const`命令，在用`let`或`const`命令声明变量之前使用该变量就叫做该变量的暂时性死区。会报ReferenceError错。
~~~ javascript
var temp = 'temp';
if(true){
    temp = 'ttemp'; // temp的死区，ReferenceError 
    let temp;
}
~~~
### 不允许重复声明
`let`不允许在同一作用域内重复声明同一变量
~~~ javascript
function t_tunc(){
    var i;
    let i; // 报错 Uncaught SyntaxError: Identifier 'i' has already been declared
}
function t_func(){
    let i;
    let i; // 报错 Uncaught SyntaxError: Identifier 'i' has already been declared
}
~~~

## const 命令
声明并且初始化一个常量，不允许只声明不初始化。
1. 一旦声明常量的值就不可修改。
1. 与`let`相同，只在声明所在的块级作用域内有效。
1. 与`let`相同，声明的常量不提升，同样存在暂时性死区。
1. 与`let`相同，不允许重复声明

## 变量的解构赋值
ES6允许按照一定的模式，从数组和对象中提取值对变量进行赋值，被称为解构。
### 数组的解构赋值
只要某种数据解构具Iterator接口，都可以使用数组解构赋值。
数组的元素是按次序排列，变量的取值由它的位置决定
~~~ javascript
let [a, b, c] = [1, 2, 3]; // a=1, b=2, c=3
let [a, [[b], c]] = [1, [[2], 3]]; // a=1, b=2, c=3
let [{aa, bb}, b, c] = [{aa: 1, bb:2}, {a:1}, {c:3}]; //aa=1, bb=2, b={a:1}, c={c:3}
let [a, ...b] = [1, 2, 3, 4]; // a=1, b=[2, 3, 4]
let [a, b, c] = new Set([1, 2, 3]); // Set解构也可以用数组的解构赋值
let [a=1, b=2, c=3] = []; // 解构赋值时可以指定默认值
~~~

### 对象的解构赋值
与数组的解构赋值相比，对象的属性没有次序，变量必须与属性同名，才能取到正确的值
~~~ javascript
let {a, b} = {a: 1, b: 2}; // a=1, b=2
let {a, b} = {a: {a:1}, b:{b: 2}}; // a={a:1}, b={b:2}
let {a: {aa}, b} = {a: {aa:1}, b:{ba: 2}}; // aa=1, b={b:2}
let {a:[aa, bb]} = {a: [1, 2]}; // aa=1, bb=2
let {a: aa, b} = {a: 1, b: 2}; // aa=1, b=2 如果变量名和属性名不一致
let {x = 3} = {}; // 对象的结构赋值也是指定默认值
~~~
如果先已声明了变量，需使用`()`
~~~ javascript
let x;
// {x} = {x: 1} 会报错，因为引擎会{x}理解为代码块
({x} = {x: 1})
~~~
也可是使用对象的解构赋值来解构数组
~~~ javascript
let arr = [1, 2, 3];
let {0: first, [arr.length-1]: last} = arr; // first=1, last=3
~~~

### 字符串的解构赋值
~~~ javascript
let [h, o, l, ll, e] = 'hello'; // h='h', e='e', l='l', ll='l', e='e'
let {length: len} = 'hello'; // len=5
~~~

### 解构赋值使用
#### 交换变量的值
~~~ javascript
let x = 1;
let y = 2;
[x, y] = [y, x];
~~~

#### 从函数返回多个值
~~~ javascript
function t_func(){
    return [1, 2, 3];
}
let [a, b, c] = t_func();

functions t_func(){
    return {
        a: 1, 
        b: 2
    }
}
let {a, b} = t_func();
~~~

#### 定义函数的参数
~~~ javascript
function add([x, y]){
    return x+y
}
add([1, 2]); // output: 3

function t_func({a, b, c}={a: 1, b:2, c:3}){
    console.log(a, b, c)
}
~~~

#### 遍历Map结构
~~~ javascript
for (let [key, value] of map) {
  console.log(key + " is " + value);
}
~~~

#### 输出模块的指定方法
~~~ javascript
const { SourceMapConsumer, SourceNode } = require("source-map");
~~~

## 字符串扩展
### 模板字符串
使用 \` 包围起来的字符。可以当做普通字符串使用，还可以定义多行字符串，或者嵌入变量
~~~ javascript
// 普通字符串
`In JavaScript '\n' is a line-feed.`

// 多行字符串
`In JavaScript this is
 not legal.`
 
 // 字符串中嵌入变量
 var name = "Bob", time = "today";
 `Hello ${name}, how are you ${time}?`
~~~

## 函数的扩展
### 函数参数的默认值
#### 用法
~~~ javascript
//基本用法
function t_func(x, y=1){ // 默认值的位置应从右向左
    console.log(y);
}
// 与解构赋值配合使用
function t_func({x,y}={x:0, y:1}){
    ...
}
~~~

#### 应用
~~~ javascript
// 指定某个参数不可省略，否则抛出错误
function throwIfMissing() {
  throw new Error('Missing parameter');
}

function foo(mustBeProvided = throwIfMissing()) {
  return mustBeProvided;
}

foo(); // Uncaught Error: Missing parameter
~~~

### rest 参数
形式为 `...变量名`， 将函数多于的参数放到变量名的数组里。

### 箭头函数
#### 用法
~~~ javascript
var f = v => v;
等同于
var f = function(v){
    return v;
};

var f = ()=>5;
等同于
var f = function(){
    return 5;
}

var f = (num1, num2) => num1+num2
等同于
var f = function(num1, num2){
    return num1+num2
}

// 如果箭头函数的代码块部分有多条语句，用大括号括起来
var f = (num1, num2) => {
    let result = num1+num2;
    return result;
}

// 由于大括号被解释为代码块，如箭头函数直接返回对象，需用圆括号括起来
var f = () => ({a: 1, b:2})


// 箭头函数可以和变量结构结合使用
var f = ({fist, last}) => first + ' ' + last;
~~~

#### 箭头函数的使用注意
1. 函数中的`this`对象就是定义时所在的对象而不是使用时所在的对象
1. 不可以作为构造函数，就不可以使用`new`
1. 不可使用`arguments`，该对象函数内不存在，如果要用可以用`rest`参数。
1. 不可使用yield

#### 箭头函数中的this
~~~ javascript
function Timer() {
  this.s1 = 0;
  this.s2 = 0;
  // 箭头函数
  setInterval(() => this.s1++, 1000);   // 这里的this 使用定义时的对象 Timer 实例
  // 普通函数
  setInterval(function () {
    this.s2++;  // 注意对应普通函数中this对象指向window
  }, 1000);
}

var timer = new Timer();

setTimeout(() => console.log('s1: ', timer.s1), 3100); // output: s1: 3
setTimeout(() => console.log('s2: ', timer.s2), 3100); // output: s2: 0
~~~

#### 嵌套的箭头函数
~~~ javascript
// 嵌套
let insert = (value) => ({
    into: (array) => ({
        after: (afterValue) => {
            array.splice(array.indexOf(afterValue) + 1, 0, value);
            return array;
        }})
    });
insert('value').into([1,2,3]).after(2)；

// 管道， 即前一个函数的输出是下一个函数的输入
const pipeline = (...funcs) => val => funcs.reduce((a, b) => b(a), val);
转换如下
function pipeline(...funcs) {
    return function (val){
        return funcs.reduce(function(a, b){return b(a)}, val)
    }

}
~~~

## 数组的扩展
### 扩展运算符
形如 `...`, 好比rest参数的逆运算，将一个数组转为逗号分隔的参数列表
~~~ javascript
console.log(1, ...[2, 3, 4], 5)
t_func(...[1,2,3])
~~~

## 对象的扩展
### 属性的简洁表示法
ES6允许直接写入变量和函数作为对象的属性和方法，使书写更加简洁。
~~~ javascript
var foo = 'foo';
var obj = {
    foo,
    method() {
        ...
    }
}
等同
var obj = {
    foo: foo,
    method: function(){
        ...
    }
}
~~~

### 属性名表达式
javascript 中定义对象属性有两种方式
~~~ javascript
// 方式一, 直接用标识符
obj.foo = 'foo'
// 方式二， 用表达式
obj['a'+'b'] = 123
~~~
~~~ javascript
let propKey = 'propKey'
var obj = {
    [propKey]: true,
    ['a'+'b']: 123
}
~~~

## Symbol
ES6引入的新的原始类型数据类型，表示**独一无二的值**。
~~~ javascript
let s = Symbol();
let s = Symbol('s'); //参数's'表示Symbol实例的描述，使Symbol变量易于区分
~~~


## 参考文档
1. <http://www.csdn.net/tag/探秘es6/news>



