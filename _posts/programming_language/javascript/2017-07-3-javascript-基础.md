---
layout: post
title: javascript 基础
categories: web前端 编程语言 javascript
description: javascript 基础
keywords: Web前端, javascript
---

## 词法结构
编程语言的词法结构是一套基础规则，用来描述如何使用这门编程语言来编写程序。
1. javascript使用unicode字符集编写程序。
2. javascript的标识符区分大小写。
3. javascript支持 `//` 和 `/**/` 两种注释方式。
4. javascript标识符要与保留字区分。
5. javascript要注意可选的 `;`

## 数据类型 
数据类型表示一个数值集合及在该集合上可执行的操作。javascript的数据类型分为两类:原始类型和对象类型。
### 原始类型
原始类型中包括，数字、文本、布尔值、null和undefined
#### 数字
整型，浮点型
##### 二进制浮点数和四舍五入误差
**问题:** javascript使用二进制表示浮点数，并可以精确表示1/2,1/4,1/1024这些分数，但对于1/10,1/100却无法
精确表示，所以用舍入的方式使极其近似目标值，所以对于部分浮点型数是存在舍入误差的。如，由于舍入误差，0.2 - 0.1 不等于 0.3 - 0.2。

**解决:** 对于金融计算或科学计算方面，为避免舍入误差，可使用大整数来解决。如10.1， 用101来表示 或 
包装成Number对象。

#### 文本
"This is a 文本"
#### 布尔值
true false
#### null 和 undefined
null
: 表示空值，说明数字、字符串或对象是无值的。

undefined
: 表示变量未初始化。
: * 当查询对象属性或数组元素时返回undefined，表示属性或元素不存在。
: * 如果函数不返回任何值，则返回undefined
: * 当函数的形参没有提供实参，则只能得到undefined

虽然null和undefined是不同的，但都表示`值得空缺`。

### 对象类型
javascript中除了原来类型以为都是对象类型。对象，数组，函数。

### 类型转换
#### 自动转换
当javascript期望一种类型的数据时，你可以提供任意类型的数据，javascript将根据需要自行转换类型。
#### 显示转换
可以使用包装对象的方式来进行类型转换。如Number, Boolean, String或Object。
#### 对象转换为原始值
所有对象都继承了 `toString()` 和 `valueOf()`方法。`toString()` 的任务是返回一个反映这个对象的字符串，
`valueOf()` 的任务并未详细定义，只是简单返回对象本身。

**对象 到 string 的转换**
1. 如果对象有`toString()`方法则调用，并把结果转换成字符串。
2. 如果对象没有`toString()`方法，则调用`valueOf()`。
3. 如果两个方法都没有，则抛出异常。

**对象 到 number 的转换**
1. 如果对象有`valueOf()`方法则调用，并把结果转换成数字
2. 如果对象没有`valueOf()` 方法，则调用`toString()` 并把结果转换
3. 如果两个方法都没有，则抛出异常。

**对象 到 boolean 值的转换**   
所有对象都转换成true.
~~~ javascript
// 看这个
b_test = new Boolean(false);
alert(Boolean(b_test)); // 结果为true
~~~

## 变量
### 变量声明
javascript中使用var来声明一个变量。
~~~
var i;
var sum;
var i, sum;
var i = 1;
~~~
### 变量作用域
全局变量可不用`var`声明。
局部变量必须用`var`声明，否则引用全局变量。同名的局部变量遮盖全局变量。

#### 函数作用域与声明提前
**函数作用域**, 指在函数体内声明的所有变量在函数体内始终可见。
基于函数作用域，函数中声明的变量被提前到函数体顶部。如:
~~~ javascript
var a = 'a';
function t(){
    console.log(a); // output => aa 而不是 a
    var a = 'aa';   // 局部变量遮盖全局变量。
}
~~~

#### 作用域链

## 运算符和表达式
### 运算符
#### 算术运算符
~~~ javascript
+ - * / %
~~~
##### + 运算
可做数值加运行，也可连接字符串
##### 一元算术运算符
~~~ javascript
++ -- + -
~~~
##### 位运算符
~~~ javascript
& | ^ ~ << >> >>>
~~~
#### 关系运算符
~~~ javascript
< <= > >= == === != !==
~~~
##### == 和 ===
==
: 相等运算符

===
: 严格相等运算符或恒等运算符。只有在不经过类型转换就相等才返回true

##### in 运算符
~~~ javascript
var point = {x: 1, y: 2};
x in point; // true
z in point; // false

var data = [1,23];
1 in data; // true
3 in data; // false
~~~

##### instanceof 运算符
判断对象是否某个类的实例
~~~ javascript
var d = new Date();
d instanceof Date; // true
~~~

#### 逻辑运算符
~~~ javascript
&& || !
~~~

#### 赋值运算符
~~~ javascript
= += -= *= /= %= >>= <<= >>>= &= |= ^|
~~~

#### 其他运算符
##### 条件运算符
~~~ javascript
?:
~~~

##### typeof 运算符
~~~ javascript
typeof x
typeof(x)
~~~

##### delete运算符
~~~ javascript
a = {a:'a', b:'b'}
b = [1,2,3,4]
delete a.a; // 把a的a属性删除
delelte b[0];   // 删除b数组的第一个元素，但数组长度没有改变,b[0]返回undefined
~~~

##### void 运算符
操作数会正常计算，但忽略计算结果并返回undefined，常用在 `javascript: URL` 中
~~~ javascript
<a href="javascript: void window.open()">打开一个新窗口</a>
~~~

##### 逗号运算符
~~~ javascript
i=1, j=2, k=3;
~~~

### 表达式
#### 对象和数组的初始化表达式
~~~ javascript
[1+2, 2+3];
{a: 1, b: 2};
~~~

#### 函数定义表达式
~~~ javascript
var t_func = function(args){...}
~~~

#### 属性访问表达式
~~~ javascript
expression.identifier
expression[expression]
~~~

#### 调用表达式
~~~ javascript
f(0)
obj.method()
~~~

#### 对象创建表达式
~~~ javascript
new Object()
当Object不需要参数时等价
new Object
~~~

### 表达式计算
~~~ javascript
eval()
~~~

## 程序控制
### if
~~~ javascript
if(expression){
...
}else{
...
}

if(expression){
...
}else if (expression){
...
}else{
...
}
~~~

### switch
~~~ javascript
switch(expression){
    case 常量:
        ...
    default:
        ...
}
~~~

### 循环
~~~ javascript
while(expression){
    ...
}

do{
    ...
}while(expression)

for(init; test; increment){
    ...
}

for(varible in object){
    ...
}
~~~

### 异常处理
~~~ javascript
throw expression
try{
    ...
}catch(e){
    ...
}finally{
    ...
}
~~~

### with
用于临时扩展作用域链
~~~ javascript
with(object){
    ...
}
//把object添加到作用域链的头部，然后执行语句块，最后再把作用域链恢复到原始状态。
~~~

## 对象
### 创建对象
~~~ javascript
var empty = {} // 创建一个空对象
var a = {a: 1, b:2} // 创建包含两个属性的对象
var b = {a: 1, b: {aa: 1, bb: 2}, c: [1, 2, 3]} //对象的属性值可是不同类型的值
var c = new Object() // 创建一个空对象，同var empty = {};
var d = Object.create(prototype) //通过制定原型创建对象，主要null表示新创建的对象没有原型
~~~

### 对象的增删改查
~~~ javascript
var obj = {a: 1, b: 2};
// 查询对象属性
var t_a = obj.a 
var t_a = obj['a'] // 这里方括号里也可以提供一个计算属性名 字符串 的表达式
// 给对象增加一个属性 
obj.c = 3; 
// 删除对象的一个属性
delete obj.c;   // 注意，不会删除原型的属性
delete obj['c'];
// 修改对象的属性
obj.a = 'a'; // 注意，如果a是继承属性且可赋值，则在本对象创建一个a属性，来屏蔽原型中a属性
~~~

### 检测对象属性
程序中经常判断某个属性是否存在于某个对象中。可以使用 `in操作符`、`hasOwnProperty`、
`propertyIsEnumerable` 或 `!== / ===`
~~~ javascript
var obj = {a: 1, b: 2};
// in 操作符
'a' in obj; // true
'c' in obj; // false

// !==
obj.a !== undefined; // true, 则obj存在a属性
obj.c !== undefined; // false, 则obj不存在c属性

// hasOwnProperty, 用于判断属性是否为对象的自有属性，即如果是继承的属性则返回false
obj.hasOwnProperty('a'); // true
obj.hasOwnProperty('c'); // false
obj.hasOwnProperty('toString'); // false

// propertyIsEnumerable, 是hasOwnProperty的增强，即只有是自有属性且可枚举属性为true时才返回真
obj_ = Object.create(obj);
obj_.a = 'a'
obj_.propertyIsEnumerable('a'); // true
obj_.propertyIsEnumerable('b'); // false, 因为继承自原型
Object.property.propertyIsEnumerable('toString') // false, 因为不可枚举
~~~

### 枚举对象的属性
程序中还经常需要枚举一个对象的属性，通常我们使用 `for/in`循环、`Object.keys()`、
`Object.getOwnPropertyNames()`

#### for / in 枚举对象属性
遍历对象中所有可枚举属性，包括继承的属性.
~~~ javascript
var baseObj = {a: 1, b: 2};
var sunObj = Object.create(baseObj);
sunObj.c = 3;
for( var p in sunObj){
    console.log(p); 
}
// output => a b c
// 其中a,b是继承属性，c是自有属性
~~~
对象继承的内置方法不可枚举，代码中给对象添加的属性是可枚举的除非修改它的属性特性
~~~ javascript
sunObj.toString = function(){}; // 屏蔽原型中的toString方法
for(var p in sunObj){
    console.log(p);
}
// output => a b c toString
~~~
所以在枚举对象的属性时，需要添加一个过滤条件, 比如只枚举对象的自有属性。
~~~ javascript
for(var p in sunObj){
    if(sunObj.hasOwnProperty(p)){
        console.log(p);
    }
}
~~~
#### 其他方式枚举对象属性
~~~ javascript
Object.keys(sunObj); // 返回对象可枚举的自有属性名称的数组， output: ['c', 'toString']
Object.getOnwPropertyNames(Object.prototype) // 返回所有自有属性，包括不可枚举的属性
~~~

### 存取器属性
~~~ javascript
var obj = {
    a: 1, 
    b: 2,
    // 这里存储器最好还是写成相同的名字
    set setter(params){
        this.a = params.a;
        this.b = params.b;
    },
    get getter(){   // 无参数
        return this.a + this.b;
    }
}
obj.getter;
obj.setter = {a: 2, b: 3};
obj.getter;
// output: 3, 7

~~~
### 属性的特性
对象的属性可是认为包含一个名字和四个特性。这四个特性分别是: 值特性(value)、可写性(writable)、
可枚举性(enumerable)和可配置性(configurable)
~~~ javascript
// 查询属性的特性
Object.getOwnPropertyDescriptor(obj, 'a')   
// output: {value: 1, writable: true, enumerable: true, configurable: true}

// 设置属性的特性
Object.defineProperty(obj, 'x', {
    value: 7,
    writable: true,
    enumerable: true,
    configurable: true
})
console.log(obj) // output: {a: 1, b: 2, x: 7} 可以看出obj增加了属性 x: 7

Object.defineProperties(obj, {
y: {value: 8, writable: true, enumerable: true, configurable: true},
z: {value: 9, writable: true, enumerable: true, configurable: true}
})
~~~

### 对象的三个属性
原型属性
: 用来继承属性

类属性
: 字符串，用来表示对象的类型

可扩展性
: 表示是否可以给对象添加新属性


## 数组
### 创建数组
~~~ javascript
var empty = []; // 创建一个空数组
var a = [,,]; // 创建含有两个undefined元素的数组
var b = [1,2,3];
var c = [1, {a: 1, b: 2}, [1, 2, 3]]; // 数组的元素可以是不同类型的值
var d = new Array(); // 创建一个空数组，同var empty = []
var e = new Array(10); // 创建指定长度的数组，注意此时并没有存储任何值，只是用来预分配一个存储空间
var f = new Array(1,2,3,4) // 指定数组元素创建数组
~~~

### 数组元素的增删改查
~~~ javascript
// 访问数组元素
var t_array = [1, 2, 3, 4];
console.log(t_array[0]);
// 修改属性元素
t_array[0] = 0;
// 添加数组元素
t_array[4] = 5;
t_array.push(6); // 在数组末尾添加一个元素
t_array.push(7, 8); // 在数组末尾添加两个元素
t_array.unshift(9, 10); //在数组头部添加两个元素
// 删除数组元素
delete t_array[3];  // delete删除数组元素并不会改变数组length属性值。
t_array.pop();  // 从数组末尾删除一个元素并返回这个元素，会改变数组length属性
t_array.shift(); // 从数组头部删除一个元素并返回这个元素，会改变数组length属性
~~~

### 数组遍历
~~~ javascript
for(var i; i < t_array.length; i++){
    ...
}
for(var index in t_array){
    console.log(t_array[index]);
}
t_array.forEach(function(ele){
    ...
});
~~~

### 数组方法
#### join
将数组所有元素转换为字符串并连接在一起，可指定一个可选的字符串来分隔数组元素
~~~ javascript
var t_array = ['a', 'b', 'c']
t_array.join(':'); // output: a:b:c
~~~

#### reverse
逆序数组，会修改原数组元素顺序
~~~ javascript
var t_array = [3, 4, 1, 5, 7, 9];
console.log(t_array.reverse());
~~~

#### sort
排序数组，并返回排序后的数组, 会修改原数组， 默认按字母顺序排序，还可以提供比较函数来排序。
比较函数的返回值分别为负数、0、正数
~~~ javascript
var t_array = [3, 4, 1, 5, 7, 9];
console.log(t_array.sort());
console.log(t_array.sort(function(a, b){return b-a}));
~~~

#### concat
连接concat参数中提供的元素，创建并返回一个新数组
~~~ javascript
var t_array = [1, 2, 3]
t_array = t_array.concat(1, 2);
t_array = t_array.concat([3, 4], [5, 6, 7]);
t_array = t_array.concat(5, [6, [7, 8]]);
~~~

#### slice
~~~ javascript
t_array.slice(0, 3);
t_array.slice(-3, -1);
~~~

#### splice
~~~ javascript
var t_array = [1, 2, 3, 4, 5]
t_array.splice(1) // t_array: [1]
t_array.splice(1, 2) // t_array: [1, 4, 5]
t_array.splice(1, 0, 'a', 'b') // t_array: [1, 'a', 'b', 2, 3, 4, 5]
t_array.splice(1, 2, 'a', 'b') // t_array: [1, 'a', 'b', 4, 5]
~~~

## 函数
### 定义函数
~~~ javascript
function t_func(a, b, c){
    ...
}

var t_func = function(a, b, c){
    ...
}
// 定义函数后立即调用
var r_func = (function(a, b, c){ ... }('a', 'b', 'c'));
// 嵌套函数
function t_tunc(){
    function inner_func(){
        ...
    }
    ...
}
~~~

### 函数的实参和形参
函数的实参和形参，个数可以不匹配，且都不做类型检查。
#### 可选形参
当允许传入的实参比形参个数少时，未接收实参的形参可理解为可选形参。
~~~ javascript
function t_func(o, /*optional*/ a){ // 加上/*optional*/注释更清楚的说明形参a为可选的，还可以给予默认值。
    a = a || [];  // 可选形参的处理，
    ...
}
~~~

#### 实参对象: 可变长的实参列表
当传入的实参个数多于形参时，函数 **无法直接获取** 那些形参个数以外的实参值(未命名值的引用)，未解决这个
问题需要使用实参对象arguments。arguments是一个指向实参数组引用。  
arguments的callee和caller属性。callee指代当前正在执行的函数，caller指代调用当前正在执行的函数的函数。

### 函数的属性、方法
#### 属性
length
: 函数定义时，形参的个数

prototype
: 函数的原型对象

#### 方法
call / apply
: 另一种调用函数的方式，可以改变原函数的运行时上下文，即改变函数内部this的指向。

~~~ javascript
function t_func(){
    console.log(this);  
}
t_func();// output: Window {stop: function, open: function, alert: function, confirm: function, prompt: function…}

var obj = {a: 1, b: 2, c: 3}
t_func.call(obj); //Object {a: 1, b: 2, c: 3}
t_func.apply(obj);

// call、apply主要的区别在为原函数提供参数的方式
function t_func(a, b, c){};
var obj = {a: 1, b: 2, c: 3}
t_func.call(obj, 1, 2, 3)
t_func.apply(obj, [1, 2, 3])
~~~

bind
: 将函数绑定至某个对象。bind将返回一个函数,并改变函数体内this的指向。bind与call、apply的主要区别，是调用bind后函数不会立即执行。

## 类
### 定义类
~~~ javascript
// 构造方法
function Range(from, to){
    // 定义实例属性
    this.from = from;
    this.to = to;
}
// 实例方法
Range.prototype.instance_method = function(){
    ...
};
// 类属性
Range.CLASS_PROPERTY = 'class_property'
// 类方法
Range.class_method = function(){
    ...
};
~~~

### 类和类型
用typeof虽然可以知道原始类型、对象和函数的类型，但当typeof应用于上面定义的类是，发现任何类的实例对象都返回Object。
那如何才能知道一个实例对象属于哪个类呢？
#### instanceof 运算符
~~~ javascript
range = new Range(1, 2);
range instanceof Range; // output: true
~~~

#### constructor 属性
~~~ javascript
range = new Range(1, 2);
range.constructor == Range; // output: true
~~~

#### 构造函数的名称
此外还可使用对象的`name`属性返回构造函数的函数名来确定类实例所属的类，如果`name`属性未定义，则可以自定义。
~~~ javascript
Range.prototype.name = (function () {
    return this.name || this.constructor.toString().match(/function\s*([^(]*)\(/)[1];
}.call(Range));
~~~

