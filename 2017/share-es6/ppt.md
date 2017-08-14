title: ECMAScript 2016 新特性与实践
speaker: Shijinyu
url: https://github.com/shijinyu/shjy-passages/2017/share-es6/ppt.md
usemathjax : yes

[slide style="background-image:url('/resources/images/bg1.png')"]

# ECMAScript2015/2016 etc.
# 新特性、应用场景与实践

##### 2017.8

[slide]

## 分享内容

----

 * 新特性
    - 作用域变化与常量
    - 函数的扩展
    - 数值的扩展
    - `class`语法糖
    - 遍历器`Iterator`与`for...of`
 * 异步语法
    - `Promise` 对象
    - 生成器`Generator` 函数
    - `async / await` 命令

[slide]

## 分享内容

----

 * 新的原始类型方法
    - `Object`对象
    - `Array`对象
 * 新的内置对象与数据类型
    - `Map`、`Set`
    - `Reflect`、`Proxy`
    - `ArrayBuffer`

[slide]

## 分享内容

----

 * 更简洁的语法：
    - 模板字符串
    - 解构赋值与属性表示
 * JS的模块与包
    - 社区规范：`commonJS`
    - `ES6 Module`

[slide]

## 分享内容

----

  * 生产环境使用
    - 最优实践
    - 工具链一：`babel`
    - 工具链二：`webpack`
    - 工具链三：`gulp`
    - 工具链四：`browserify`
    - 语法检查：`eslint`

[slide]

## 新特性

### 作用域与常量

----

```javascript

// var foo = 0;
for(let foo = 0;foo < 100;foo++){
    // dosomething...
}

console.log(foo); // ?

```

[slide]

## 新特性

### 作用域与常量

----

变量提升、暂时性死区、不允许重复声明

```javascript

console.log(foo,bar); // ?
var foo = 100;
let bar = 100;

if(true){
    console.log(bar);
    let bar;
}

```

[slide]

## 新特性

### 作用域与常量

----

常量 ： 不可变的值。

```javascript

const foo = 1;
const bar = {
    'x' : 0,
    'y' : 0
}

```
[slide]

## 新特性

### 函数的扩展

----

参数的默认值:

```javascript

function add(x,y){
    x = x === undefined ? 0 : x;
    y = y === undefined ? 0 : y;
    return x + y;
}

//  => ES 6

function add(x = 0,y = 0){
    return x + y;
}

```

[slide]


## 新特性

### 函数的扩展

----

rest参数：

```javascript
// receive items as an array
function push(array, ...items) {
  items.forEach(function(item) {
    array.push(item);
    console.log(item);
  });
}

var a = [];
push(a, 1, 2, 3);

```

[slide]


## 新特性

### 函数的扩展

----

箭头函数

```javascript

const addOne = v => v + 1;

// => ES5 : 

var addOne = function (v){
    return v + 1;
}.bind(this);

```

[slide]


## 新特性

### 函数的扩展

----

 - 箭头函数中的`this`始终指向定义时的上下文。
 - 箭头函数不存在`arguments`对象。
 - 不可以以`new`命令调用——不能作为构造函数。

[slide]


## 新特性

### 数值的扩展

----

 - `Number.parseInt`,`Number.parseFloat`
 - `Number.isInteger`,`Number.isSafeIntege`
    * `Number.MIN_SAFE_INTEGER`,`Number.MAX_SAFE_INTEGER`
 - `Number.isFinite()`, `Number.isNaN()`
    * 与`isNaN`和`isFinite`的区别：前者只判断数值。
 - 

[slide]

## 新特性

### 数值的扩展

----

 - `Math.trunc`返回一个数字的整数部分
 - `Math.sign` 判断正负 : `-1,+1,0,NaN`
 - `Math.cbrt` 立方根 : 3√x
 - `Math.clz32` 将值转为32位二进制
 - `Math.imul` 以32位带符号整数形式相乘的结果
 - `Math.hypot`  = √(a^2 + b^2 + c^2 + ...)
 - `Math`的对数运算
 - ...

[slide]

## 新特性

### `class`语法糖

----

```javascript

// ES 5

function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

```

[slide]

## 新特性

### `class`语法糖

----

```javascript
// ES6

class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}

```

[slide]

## 新特性

### `class`语法糖

----

继承

```javascript

// ES 5
// 1
function Sub(){
    // dosomething ...
    Sup.apply(this,arguments);
}
Sub.prototype = new Sup();
Sub.prototype.constructor = Sub;
Sub.prototype.subMethod = ...
// 2
Sub.prototype = Object.create(Sup.prototype);
// => 《Javascript高级程序设计》
```

[slide]

## 新特性

### `class`语法糖

----

继承

```javascript

// ES 6
class Sub extends Sup{
    constructor(...args){
        super(...args);
    }
    // ...
}

```

> ES6中必须先初始化`super`才能使用`this`。

[slide]

## 新特性

### `class`语法糖

----

私有方法：

```javascript

// ES6/7 尚没有相关提案，因此实现并没有区别

function privateAdd(){
    return ++this.x;
}

function Foo(){
    this.x = 0;
}
Foo.prototype.doAdd = function(y){
    return y + privateAdd();
}
// => ES6
class Foo{
    constructor(){
        this.x = 0;
    }
    doAdd(y){
        return y + privateAdd();
    }
}

```

[slide]

## 新特性

### `class`语法糖

----

私有属性：

```javascript

class Foo{
    #provateVal = 1;
}

```

[slide]

## 新特性

### `class`语法糖

----

>这是是ES Next的一个提案，babel有插件。

[slide]

## 异步语法糖

### Promise 对象

----

```javascript

const getJSON = function(url){
    return new Promise((success,fail)=>{
        const xhr = new XMLHttpRequest();
        xhr.onreadystatechange = function () { 
            if (request.readyState === 4) {
                if (request.status === 200) {
                    return success(request.responseText);
                } else {
                    return fail(request.status);
                }
            } 
        }
        xhr.open('GET', url);
        xhr.responseType = "json";
        xhr.setRequestHeader("Accept", "application/json");
        xhr.send();
    });
}

getJSON('url/to/server').then((data)=>{
    // dosomething while success ...
}).catch((err)=>{
    // dosomething while failed ...
})

```

[slide]

## 异步语法糖

### Promise 对象

----

 - 状态 : `Pending`（进行中）、`Fulfilled`（已成功）和 `Rejected`（已失败）
 - 一旦`Promise`执行就不可终止，并且可以在任何时候获得结果。
 - 执行后，`Promise`的状态总是固定的，不会被外界改变。
 - 嵌套 : 可以在`Promise`对象内嵌套`Promise`并传递给`Resolve`,如此可以通过`then`管道逐层调用。
 - 良好的错误处理。

[slide]

## 异步语法糖

### Promise 对象

----

方法：`Promise.prototype.then`

```javascript
/**
* @param resolve {Function} , 成功时
* @param reject {Function} , 失败时
*/

getJSON("/post/1.json").then(
  post => getJSON(post.commentURL)
).then(
  comments => console.log("Resolved: ", comments),
  err => console.log("Rejected: ", err)
);

```

[slide]

## 异步语法糖

### Promise 对象

----

<br>

方法：`Promise.prototype.catch`
方法：`Promise.prototype.finally`
方法：`Promise.prototype.done`

```javascript
getJSON('/posts.json').then(posts => {
  // ...
}).catch(error => {
  // 处理 getJSON 和 前一个回调函数运行时发生的错误
  console.log('发生错误！', error);
});
```

>`then(resolve).catch(reject)`与`then(resolve,reject)`有什么不同？

[slide]

## 异步语法糖

### Promise 对象

----

 - `Promise.all` : 将多个 Promise 实例，包装成一个新的 Promise 实例。
 - `Promise.race` : 同上，但会在一个实例改变状态后执行回调。
 - `Promise.resolve` : 将对象转化成`Promise`。
 - `Promise.reject` : 同上，但对象的状态为`reject`。
 - etc.

[slide]

## 异步语法糖

### 生成器：`Generator`

----

>Koa = Express + Generator + co

```javascript

function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
```
```javascript
hw.next();
// { value: 'hello', done: false }

hw.next();
// { value: 'world', done: false }

hw.next();
// { value: 'ending', done: true }

hw.next();
// { value: undefined, done: true }

```

[slide]

## 异步语法糖

### 生成器：`Generator`

----

>如何自动执行`Generator`？
>试试`co`库吧：

<br>

[in->gayhub](https://github.com/tj/co)

<br>

```javascript

co(function* () {
  var result = yield Promise.resolve(true);
  return result;
}).then(function (value) {
  console.log(value);
}, function (err) {
  console.error(err.stack);
});

```

[slide]

## 异步语法糖

### 异步函数：`async / await`

----

```javascript
#!usr/etc/node
var fs = require('fs');

var readFile = function (fileName) {
  return new Promise(function (resolve, reject) {
    fs.readFile(fileName, function(error, data) {
      if (error) return reject(error);
      resolve(data);
    });
  });
};

```

[slide]

## 异步语法糖

### 异步函数：`async / await`

----

```javascript


// use Generator

var gen = function* () {
  var f1 = yield readFile('/etc/fstab');
  var f2 = yield readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};

// use async

var asyncReadFile = async function () {
  var f1 = await readFile('/etc/fstab');
  var f2 = await readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};

```

[slide]

## 异步语法糖

### 异步函数：`async / await`

----

```javascript

var p = (arg) => new Promise((resolve,reject)=>{
    // ...
    resolve(value);
    // ...
    reject(error);
});

p(arg).then((value)=>{
    console.log(value);
}).catch((err)=>{
    console.log(err);
});

// => async
async function wrapper(){
    try{
        var value = await p(arg);
    }catch(err){
        console.log(err)
    }
}
    

```

[slide]

## 异步语法糖

### 异步函数：`async / await`

 - `await`命令后面是一个 `Promise` 对象（如不是则立即调用`Promise.resolve`隐式转换）。
 - `async function`返回的也是 `Promise`对象。
 - `return` 语句返回的值，会成为`then`方法回调函数的参数。
 - 多个`await`会依次执行，不存在并行，如不存在依赖，则考虑性能问题。
 - `await`后的`Promise`对象状态为`rejected`时可能终止后续代码执行，因此最好放在`try...catch...`块中。

```javascript
// 并发写法：
// 写法一
let [foo, bar] = await Promise.all([getFoo(), getBar()]);

// 写法二
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;

```

[slide]

## 新的原始类型方法

### `Object`对象

----

  - `Object.defineProperty` - ES5 方法
  - `Object.is()`类似`===`
  - `Object.assign() `浅拷贝，类似`$.extend(false,...)`
  - 属性的遍历 : `Object.keys(obj)`与`Object.getOwnPropertyNames(obj)`
  - `Object.values()，Object.entries() `与`Object.keys`对应

[slide]

## 新的原始类型方法

### `Array`对象

----

 - 把类数组对象、值变成数组：`Array.from` `Array.of`。
 - `[].copyWithin(target, start = 0, end = this.length)`
 - `[].find()` 和 `[].findIndex()`
 - `[].includes()`
 - `[].fill()`

[slide]

## 更简洁的语法

### 模板字符串

----

 - 变量与表达式可以与字符串混合
 - 可写多行字符串

```javascript

var template = '<div>count:' + somevalue + '<\/div>';

// =>

var template = `<div>count:${somevalue}<\/div>`;

```

[slide]

## 更简洁的语法

### 解构赋值与属性表示

----

```javascript

let {assign,keys} = Object;

var div = document.querySelectorAll('div');
var div1 = div[0],
    div2 = div[1];

// =>

let [div1,div2] = [...document.querySelectorAll('div')];

const attr1 = 'attrName';

let obj = {
    [attr1] : 'value',
    'otherAttr' : 'value2',
}

console.log(obj.attrName) // `value`

```

[slide]

## JS的模块与包

### 社区规范

----

 - 上古时代：`umd` ： 
 
 ```javascript
(function(window,undefined){
    var jQuery = function(){
        // ...
    }
    // ... 
    window.jQuery = jQuery
})(window);

 ```

[slide]

## JS的模块与包

### 社区规范

----


 - `amd`与`requireJS` : [gayhub-wiki](https://github.com/amdjs/amdjs-api/wiki/AMD)

 - `cmd`与`sea.js` : [gayhub-wiki](https://github.com/seajs/seajs/issues/242)

```javascript

// CMD
define(function(require, exports, module) {
    var a = require('./a')
    a.doSomething()
    var b = require('./b')
    b.doSomething()   
    // ... 
})
// AMD
define(['./a', './b'], function(a, b) {  
    a.doSomething()
    b.doSomething()    
    // ...
})

```

[slide]

## JS的模块与包

### 社区规范：`commonJS`

----

```javascript

var a = require('./a')
a.doSomething()
var b = require('./b')
b.doSomething()  

module.exports = something;

```

[slide]

## JS的模块与包

### `ES6 Module`

----

```javascript

import { stat, exists, readFile } from 'fs';

var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};
export default React;
```

[slide]

## JS的模块与包

### `ES6 Module`

----

 - 模块基于编译时加载，而非运行时加载
 - 模块既可以整体加载也可以局部加载
 - 自动处于严格模式
 - 模块只会加载一次

[slide]

## 生产环境使用

### 最优实践

----

 - 浏览器支持如何？

[Can I use?](http://caniuse.com/)

 - `nodejs`或者说`v8`支持如何？

 - 性能如何？

[slide]

## 生产环境使用

### 工具链一：`babel`

----

原名`6to5`，支持`es2015/2016/Next`转成`ES5`语法。

[babel](http://babeljs.io)

[slide]

## 生产环境使用

### 工具链二：`webpack`

----

 - 打包工具，通过分析包依赖生成可在浏览器环境下直接执行的代码；
 - 通过插件机制在分析过程中转换代码，如`babel-loader`可转换`es6`。

[webpack](https://doc.webpack-china.org/)

[slide]

## 生产环境使用

### 工具链三：`gulp`

----

 - 灵活的任务分发器
 - 通过`Stream`管道进行处理
 - 强大的插件机制

[gulpjs](http://www.gulpjs.com.cn/)

[slide]

## 生产环境使用

### 工具链三：`browserify`

----

将`commonJS`代码转换成可在浏览器环境下执行的代码

[http://browserify.org/](http://browserify.org/)


[slide]

## 生产环境使用

### 语法检查：`eslint`

----

[http://eslint.org/](http://eslint.org/)

[slide]

## Q & A

## Thanks