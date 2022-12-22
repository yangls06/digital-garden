---
title: "Hands on JavaScript"
tags:
- JavaScript
weight: -5
---



# JavaScript基础

最近工作中会涉及到前端工作，因此要用到JavaScript，故而学习一波。参考：

* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Language_Overview

## 环境准备：Jupyter Notebook + IJavaScript

打算在Jupyter Notebook中使用JavaScript，便于交互式的学习和探查。于是找到了IJavascript这个Jupyter Notebook的JavaScript Kernel. 
其安装和使用参考主页：http://n-riesco.github.io/ijavascript/

> 注意📢：在安装的过程中（brew install pkg-config node zeromq），可能会出现“Error: No such file or directory @ rb_sysopen”的错误，可以参考：https://blog.csdn.net/weixin_43770545/article/details/127715990, 做相应的问题排查和修复。

安装好IJavaScript之后，可以直接通过jupyter-lab（或者jupyter notebook）命令启动，然后选择JavaScript内核的Notebook。

![image-20221222100114102](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221222100114102.png)

然后就能在其中测试和探索JavaScript的各种功能。

下面是跟随https://developer.mozilla.org/en-US/docs/Web/JavaScript/Language_Overview 教程一路run下来的结果。

## Data types：数据类型
JavaScript中有7中原生类型：
* Number：除了非常非常大的整数，Number可以表达任意数字（整数、浮点数均可）。
* Bigint：非常非常大的整数。
* String：字符串。
* Boolean：布尔型，true/false。
* Symbol：类似uuid，creating unique identifiers that won't collide。
* Undefined：没有被赋值的变量。
* Null：non-value

其他的数据都属于`Object`，包括：
* Function：函数不是一种特殊的数据结构，它只是可以被调用的一种特殊的object。
* Array
* Date
* RegExp
* Error

> 疑问❓：没有dict或者map吗？

### Numbers

JavaScript有两种数字类型：Number和Bigint。

Number既能表示整数（-(2^53-1)~2^53-1）,又能表示浮点数（最大值：1.79\*10^308）


```javascript
console.log(3/2); // 1.5, not 1
```

    1.5



```javascript
// 浮点数可以存在不精确的情况
console.log(0.1+0.2)
```

    0.30000000000000004



```javascript
// Number literals can also have prefixes to indicate the base 
// (binary, octal, decimal, or hexadecimal), or an exponent suffix.

console.log(0b111110111); // 503
console.log(0o767); // 503
console.log(0x1f7); // 503
console.log(5.03e2); // 503

```

    503
    503
    503
    503


Bigint用来指定数值是整数，它是在数字后面跟一个后缀：`n`


```javascript
console.log(-3n)
```

    -3n



```javascript
// console.log(-3.1n)
```


```javascript
console.log(-3n/2n)
```

    -1n



```javascript
// Bigint与Number不能混合运算
// console.log(-3n + 2); //TypeError: Cannot mix BigInt and other types, use explicit conversions
```

Math是一个提供标准数据运算的Object。


```javascript
Math.sin(3.5);
```




    -0.35078322768961984




```javascript
var r0 = 2;
const circumference0 = 2 * Math.PI * r0;
```


```javascript
circumference0
```




    12.566370614359172



可以使用下面的方式做数字和字符串之间的转换：
* parseInt()：把字符串解析成整数。
* parseFloat()：将字符串解析成一个浮点数。
* Number()：将表示Number数值的字符串解析成Number类型的值。也可以使用`+/-`符号来代替Number()函数。


```javascript
parseInt('123');
```




    123




```javascript
parseInt('123n');
```




    123




```javascript
parseInt('-123');
```




    -123




```javascript
parseInt('123.4'); // 可以解析小数，只是得到其整数部分
```




    123




```javascript
parseFloat('123.4'); 
```




    123.4




```javascript
parseInt('0b111110111'); // 不能正确解析其他进制的数
```




    0




```javascript
Number('0b111110111'); 
```




    503




```javascript
Number('0o767'); 
```




    503




```javascript
+'0o767'
```




    503




```javascript
-'0b111110111'
```




    -503



`NaN`表示Not a Number，比如解析一个非数字表达；传入`NaN`做运算，会返回`NaN`。

`Infinity`表示无穷大，除以0会产生这个值。它有正负之分。


```javascript
parseInt('Not a Number');
```




    NaN




```javascript
NaN + 1
```




    NaN




```javascript
1/0
```




    Infinity




```javascript
-1/0
```




    -Infinity



### String

JavaScript中的字符串就是Unicode（准确说是UTF-16编码）字符的序列。



```javascript
console.log('Hello, world');
console.log('你好，世界！');
```

    Hello, world
    你好，世界！



```javascript
// 单引号和双引号都OK
console.log("Hello, world");
console.log("你好，世界！");
```

    Hello, world
    你好，世界！



```javascript
// 字符和字符串之间也没有差别：字符就是长度为1的字符串
'Hello'[1] === 'e'
```




    true




```javascript
// 字符串长度：length属性
'Hello'.length
```




    5




```javascript
// 字符串相加
const age = 25;
console.log('I am ' + age + ' years old.') // String concatenation
```

    I am 25 years old.



```javascript
// 也可用字符串模板：使用反引号 `` + ${}
console.log(`I am ${age} years old.`)
```

    I am 25 years old.


### 其他类型

在JavaScript中，`null`表示deliberate non-value（故意的空值），`undefined`表示absence of value（没有定义的值）。`null`只能通过null关键字获取，而`undefined`可以通过下面的多种方式获取：
* return语句不带值（return;）
* 访问object一个并不存在的属性（obj.iDontExist）
* 申明一个变量但不赋值(let x;)


```javascript
function returnNothing() {
    return
}

console.log(returnNothing())
```

    undefined



```javascript
var arr = [1, 2, 3]

console.log(arr.iDontExist)
```

    undefined



```javascript
let xxx;
console.log(xxx)
```

    undefined


JavaScript的布尔值`true`和`false`，任何值都可以转换成布尔值，其规则如下：
* `false`, `0`, 空字符串`""`, `NaN`, `null`以及`undefined`被转换成`false`
* 其他值都被转换成`true`


```javascript
Boolean("")
```




    false




```javascript
Boolean(0)
```




    false




```javascript
Boolean(0.0)
```




    false




```javascript
Boolean(undefined)
```




    false




```javascript
Boolean('Hello')
```




    true



## Variables: 变量

JavaScript中，变量可以由下面的三个关键字申明：`let`, `const`, `var`。他们之间的差别可以参考：

[JavaScript 中的 Var、Let 和 Const 有什么区别](https://www.freecodecamp.org/chinese/news/javascript-var-let-and-const/#:~:text=var%20%E5%A3%B0%E6%98%8E%E6%98%AF%E5%85%A8%E5%B1%80%E4%BD%9C%E7%94%A8,%E5%85%B6%E4%BD%9C%E7%94%A8%E5%9F%9F%E7%9A%84%E9%A1%B6%E7%AB%AF%E3%80%82)。

总结其异同点：
* `var`声明是全局作用域或函数作用域，而`let`和`const`是块作用域。
* `var`变量可以在其范围内更新和重新声明； `let`变量可以被更新但不能重新声明； `const`变量既不能更新也不能重新声明。
* 它们都被提升到其作用域的顶端。但是，虽然使用变量undefined初始化了`var`变量，但未初始化`let`和`const`变量。
* 尽管可以在不初始化的情况下声明`var`和`let`，但是在声明期间必须初始化`const`。

现在`let`是主流的申明方式。

下面通过几个例子来说明。


```javascript
// var的作用域是全局或者函数范围

var greeter = 'hi';

function newFunc() {
    var hello = 'hello';
}

console.log(greeter);
// console.log(hello); // ReferenceError: hello is not defined
```

    hi



```javascript
// var变量可以重新申明和修改
var greeter = 'hi';
greeter = 'hello';
console.log(greeter);

var greeter = 'hi hi hi';
console.log(greeter);
```

    hello
    hi hi hi



```javascript
// var 的变量提升
// 变量提升是 JavaScript 的一种机制:在执行代码之前，变量和函数声明会移至其作用域的顶部。这意味着如果我们这样做:

console.log(greeter0);
var greeter0 = 'hi 0';
```

    undefined



```javascript
// 上面的代码会被解释为：
var greeter0;
console.log(greeter0);
greeter0 = 'hi 0';
```

    hi 0





    'hi 0'




```javascript
// var的问题：因为全局作用域造成的不希望发生的赋值和引用

var greeter1 = 'hi';
var times = 4;

if(times > 3) {
    var greeter1 = 'Hello'
}

console.log(greeter1)

```

    Hello


上例中，if block中的greeter1影响了全局greeter1的值。这有可能是我们不希望看到的。这就是使用let的原因：


```javascript
// let的作用域是block {} 级别的

let greeter3 = 'hi';
let times1 = 4;

if(times1 > 3) {
    let greeter3 = 'Hello';
    console.log(greeter3);
}

console.log(greeter3);
```

    Hello
    hi



```javascript
// let变量能被修改，但不能被重新申明
let greeter5 = 'hi';
greeter5 = 'hello';

console.log(greeter5);
```

    hello



```javascript
// let greeter5 = 'hi'; //SyntaxError: Identifier 'greeter5' has already been declared
```

`const`与`let`类似，只不过其申明的变量必须保持常量，不能被修改。


```javascript
const greeter6 = 'hi';
// greeter6 = 'hello'; //TypeError: Assignment to constant variable.
```

不过其申明的object类型的变量，其属性是可以被改变的。


```javascript
const obj_greeter = {
    message: 'hi',
    times: 4
}

obj_greeter.message = 'hello'

console.log(obj_greeter)
```

    { message: 'hello', times: 4 }



```javascript
obj_greeter.receiver = 'Jack'

console.log(obj_greeter)
```

    { message: 'hello', times: 4, receiver: 'Jack' }


JavaScript是动态类型，意味着同一个变量名可以指向不同类型的数据。


```javascript
let a = 1;
a = 'foo';
```




    'foo'



## Operators：运算符

JavaScript支持的运算符包括：
* `+`, `-`, `*`, `/`, `%`, `**`: 数学运算
* `=`, `+=`, `-=`: 赋值
* `++`, `--`: 自加自减
* `+`: 字符串拼接
* `>`, `<`, `>=`, `<=`: 不等比较
* `==`: 双等号，不同类型会强制转换（type coercion）, 对应的不等号是`!=`
* `===`: 三等号，不同类型不会强制转换, 对应的不等号是`!==`
* `&&`, `||`, `!`: 逻辑运算：与或非
* `&`, `|`, `~`: 位运算：与或非

更详尽的运算符说明，见[链接](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators)。

下面是一些示例。


```javascript
// 求余数
123 % (3.5)
```




    0.5




```javascript
// 注意顺序
"3" + 4 + 5;
```




    '345'




```javascript
3 + 4 + '5';
```




    '75'




```javascript
// 双等号 vs 三等号

console.log(123 == '123');
console.log(1 == true);

console.log(123 === '123');
console.log(1 === true);
```

    true
    true
    false
    false



```javascript
// 逻辑运算
const a1 = 0 && 'Hello'; // 0 because 0 is "falsy"
console.log(a1);
```

    0



```javascript
const b1 = "Hello" || "world"; // "Hello" because both "Hello" and "world" are "truthy"
console.log(b1);
```

    Hello


## Grammar: 语法风格

JavaScript的语法风格接近于C语言。有下面几点值得注意：
* 注释：单行用`//`，多行用`/* */`
* 表达式的结尾用';'：但这个分号也是可选的

## Control Structure: 控制结构

和大多数语言一样，JavaScript包括下面的控制结构：
* `if`, `else`: 条件
* `while`, `do...while`: 循环
* `for`, 除了常规的for循环，还衍生出两种特殊的遍历：
    * `for...of`: 遍历数组的各个元素
    * `for...in`: 遍历object的各个属性
* `switch`: 分支
* `try...catch`, `throw`: 异常

## Objects: 对象类型

JavaScript的object类型用来放键值对key-value数据，相当于Python中的dict. object是非常动态的, 它的属性可以随时被添加、删除、重排、突变mutated。object的key总是string或者symbol。


```javascript
const obj = {
    name: 'Carrot',
    for: 'Max',
    details: {
        color: 'orange',
        size: 12
    }
}

obj
```




    { name: 'Carrot', for: 'Max', details: { color: 'orange', size: 12 } }




```javascript
// dot notation: 只能是一个静态的标识符
console.log(obj.name);

// bracket notation：可以是一个动态的变量
console.log(obj['name'])
```

    Carrot
    Carrot



```javascript
// 用变量来标识一个key
let userName1 = 'nick';
obj[userName1] = 'Catty';
obj
```




    {
      name: 'Carrot',
      for: 'Max',
      details: { color: 'orange', size: 12 },
      nick: 'Catty'
    }




```javascript
// 链式访问
console.log(obj.details.color);
console.log(obj['details']['size']);
```

    orange
    12



```javascript
// object是按照引用传值
const obj1 = {}

function doSth(o) {
    o.x = 1;
}

doSth(obj1);

obj1
```




    { x: 1 }




```javascript
// 引用
const obj2 = obj1;
obj1.y = 2;

obj2
```




    { x: 1, y: 2 }



## Arrays: 数组

JavaScript中的数组是一种特殊的object，通过`[index]`来访问元素。数组长度通过`.length`来获取。


```javascript
const arr1 = ['dog', 'cat', 'hen'];
a.length
```




    3




```javascript
// 可以给任意非负整数的下标赋值
arr1[10] = 'fox'

arr1
```




    [ 'dog', 'cat', 'hen', <7 empty items>, 'fox' ]




```javascript
// 数组访问越界是不会报错的，只会放回一个undefined
console.log(arr1[100])
```

    undefined



```javascript
// 数组元素可以是任意类型的
arr1.push
arr1.push(false);
arr1.push(null);
arr1.push(101);
arr1.push({});
arr1.push([]);

arr1
```




    [
      'dog',
      'cat',
      'hen',
      <7 empty items>,
      'fox',
      false,
      null,
      101,
      {},
      []
    ]




```javascript
arr1[14].x = 1;
arr1[15].push('ok');

arr1
```




    [
      'dog',
      'cat',
      'hen',
      <7 empty items>,
      'fox',
      false,
      null,
      101,
      { x: 1 },
      [ 'ok' ]
    ]




```javascript
// 遍历： 通过for

for(let i = 0; i < arr1.length; i++) {
    console.log('#' + i + ' : ' + arr1[i])
}
```

    #0 : dog
    #1 : cat
    #2 : hen
    #3 : undefined
    #4 : undefined
    #5 : undefined
    #6 : undefined
    #7 : undefined
    #8 : undefined
    #9 : undefined
    #10 : fox
    #11 : false
    #12 : null
    #13 : 101
    #14 : [object Object]
    #15 : ok



```javascript
// 遍历： for...of
for(const a of arr1) {
    console.log(a);
}
```

    dog
    cat
    hen
    undefined
    undefined
    undefined
    undefined
    undefined
    undefined
    undefined
    fox
    false
    null
    101
    { x: 1 }
    [ 'ok' ]


Array有一系列的方法，比如cancat, map, filter, slice等，详见[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)。


```javascript
// map
const babies = ['dog', 'cat', 'hen', 'fox'].map((name) => 'baby ' + name);
babies
```




    [ 'baby dog', 'baby cat', 'baby hen', 'baby fox' ]




```javascript
// concat: 数组拼接
const arr2 = ['a', 'b', 'c'];
const arr3 = ['d', 'e', 'f'];
const arr4 = arr2.concat(arr3);

arr4
```




    [ 'a', 'b', 'c', 'd', 'e', 'f' ]



## Functions: 函数

函数在JavaScript中非常重要，是其核心组成部分。下面是一个基础的函数声明：


```javascript
function add(x, y) {
    const total = x + y;
    return total;
}
```


```javascript
// 函数传参个数可以少于定义的参数个数，缺少的参数会被定义成undefined
add(); // Equivalent to add(undefined, undefined)
```




    NaN




```javascript
// 如果传入的参数个数多余定义的，多余的参数被直接忽略。总之，函数不会因为参数的多少而报语法错
add(1, 2, 3, 4);
```




    3



函数的参数可以是rest parameter语法，通过一个数组来存储未明确指定的参数值，类似Python的`*args`（注：在语法层面没有`**kwargs`）。 


```javascript
function avg(...args) {
    let sum = 0;
    for (const item of args) {
        sum += item;
    }
    return sum / args.length;
}

avg(1, 2, 4, 5)
```




    3




```javascript
// 还可以这么调用
const arr5 = [5, 6, 7, 8]
avg(...arr5)
```




    6.5



虽然JavaScript的函数不支持`**kwargs`这样的命名参数，但是可以通过传入object类型参数，通过`object destructuring`来`pack/unpack`。


```javascript
// Note the { } braces: this is destructuring an object
function area({ width, height }) {
  return width * height;
}

// The { } braces here create a new object
console.log(area({ width: 2, height: 3 }));
```

    6



```javascript
console.log(area({ width1: 2, height1: 3 }));
```

    NaN



```javascript
// 与其他语言一样，JavaScript的函数支持参数的默认值
function avg3(v1, v2, v3=0) {
    return (v1 + v2 + v3)/3
}

avg3(1, 2)
```




    1



### Anonymous functions: 匿名函数

匿名函数就是没有名字的函数。在实践中，匿名函数常常会作为参数传递给其他函数，或者立刻传入参数让函数立刻被调用（通常只被调用一次），抑或被另一个函数作为返回值返回。

下面是一个匿名函数的定义方式：function后面不带函数名。


```javascript
const avgFunc = function (...args) {
    let sum = 0;
    for (const item of args) {
        sum += item;
    }
    return sum / args.length;  
};

avgFunc(1,2,4);
```




    2.3333333333333335



另一种定义的方式是通过arrow function expression，也就是`=>`: 


```javascript
const avgFunc2 = (...args) => {
    let sum = 0;
    for (const item of args) {
        sum += item;
    }
    return sum / args.length;  
};

avgFunc2(1,2,4);
```




    2.3333333333333335




```javascript
// 对简单的表达式，可以不用return
const sumFunc = (a, b, c) => a + b + c;
sumFunc(1, 2, 3);
```




    6




```javascript
const sumFunc1 = (a, b, c) => {return a + b + c;};
sumFunc1(1, 2, 3);
```




    6



### Functions are first-class objects: 函数是头等公民

函数像其他类型一样，可以被赋值给其他参数，或者传参给其他函数，以及被其他函数作为返回值返回。


```javascript
const addxy = (x) => (y) => x + y;
```


```javascript
addxy(1)
```




    [Function (anonymous)]




```javascript
addxy(1)(2)
```




    3




```javascript
// Function accepting function
const babies2 = ["dog", "cat", "hen"].map((name) => `baby ${name}`);
babies2
```




    [ 'baby dog', 'baby cat', 'baby hen' ]



### Inner functions: 内部函数

在函数内部定义的函数，内部函数可以访问上层函数作用域内的变量。这个特性可以在内部函数间共享上层函数的变量，从而避免污染全局变量。


```javascript
function parentFunc() {
    const a = 1;
    
    function innerFunc() {
        const b = 4;
        return a + b;
    };
    
    return innerFunc();
};

parentFunc();
```




    5



## Classes: 类

JavaScript的类定义类似Java：


```javascript
class Person {
    constructor(name) {
        this.name = name;
    };
    
    sayHello() {
        return `Hello, I am ${this.name}!`;
    };
};

const p = new Person('Maria');
p.sayHello();
```




    'Hello, I am Maria!'



## Asynchronous programming: 异步编程

JavaScript is single-threaded by nature. There's *no paralleling; only concurrency*. Asynchronous programming is powered by an event loop, which allows a set of tasks to be queued and polled for completion.

There are three idiomatic ways to write asynchronous code in JavaScript:
* Callback-based (such as setTimeout())
* Promise-based
* async/await, which is a syntactic sugar for Promises

For example, here's how a file-read operation may look like in JavaScript:


```javascript
// Callback-based

/*
fs.readFile(filename, (err, content) => {
  // This callback is invoked when the file is read, which could be after a while
  if (err) {
    throw err;
  }
  console.log(content);
});
// Code here will be executed while the file is waiting to be read
*/


// Promise-based
/*
fs.readFile(filename)
  .then((content) => {
    // What to do when the file is read
    console.log(content);
  }).catch((err) => {
    throw err;
  });
// Code here will be executed while the file is waiting to be read
*/

// Async/await

/*
async function readFile(filename) {
  const content = await fs.readFile(filename);
  console.log(content);
}
*/
```

## Modules: 模块

模块通常是一个js文件，可以被一个文件路径或者URL指定。可以通过import或者export在module之间交换数据。


```javascript
/*

import { foo } from "./foo.js";

// Unexported variables are local to the module
const b = 2;

export const a = 1;

*/
```





**这就是JavaScript的基本语法知识。Keep going!**

