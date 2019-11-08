---
title: JavaScript 面试题收集    --持续更新
abbrlink: 80c1f29a
date: 2019-08-30 21:52:57
tags:
---


![](/assets/blogImg/201908302202.jpg)

## 送你 43 道 JavaScript 面试题

原文链接：https://juejin.im/post/5d0644976fb9a07ed064b0ca

1. 下面代码的输出是什么？

```
function sayHi() {
  console.log(name);
  console.log(age);
  var name = "Lydia";
  let age = 21;
}

sayHi();
```
A: Lydia 和 undefined
B: Lydia 和 ReferenceError
C: ReferenceError 和 21
D: undefined 和 ReferenceError

答案: D
<!--more-->

在函数中，我们首先使用var关键字声明了name变量。 这意味着变量在创建阶段会被提升（JavaScript会在创建变量创建阶段为其分配内存空间），默认值为undefined，直到我们实际执行到使用该变量的行。 我们还没有为name变量赋值，所以它仍然保持undefined的值。
使用let关键字（和const）声明的变量也会存在变量提升，但与var不同，初始化没有被提升。 在我们声明（初始化）它们之前，它们是不可访问的。 这被称为“暂时死区”。 当我们在声明变量之前尝试访问变量时，JavaScript会抛出一个ReferenceError。
译者注：
关于let的是否存在变量提升，我们何以用下面的例子来验证：
```
let name = 'ConardLi'
{
  console.log(name) // Uncaught ReferenceError: name is not defined
  let name = 'code秘密花园'
}
```
let变量如果不存在变量提升，console.log(name)就会输出ConardLi，结果却抛出了ReferenceError，那么这很好的说明了，let也存在变量提升，但是它存在一个“暂时死区”，在变量未初始化或赋值前不允许访问。
变量的赋值可以分为三个阶段：

创建变量，在内存中开辟空间
初始化变量，将变量初始化为undefined
真正赋值

关于let、var和function：

- let 的「创建」过程被提升了，但是初始化没有提升。
- var 的「创建」和「初始化」都被提升了。
- function 的「创建」「初始化」和「赋值」都被提升了。

----

2. 下面代码的输出是什么?

```
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}
```
A: 0 1 2 and 0 1 2
B: 0 1 2 and 3 3 3
C: 3 3 3 and 0 1 2

答案: C
由于JavaScript中的事件执行机制，setTimeout函数真正被执行时，循环已经走完。 由于第一个循环中的变量i是使用var关键字声明的，因此该值是全局的。 在循环期间，我们每次使用一元运算符++都会将i的值增加1。 因此在第一个例子中，当调用setTimeout函数时，i已经被赋值为3。
在第二个循环中，使用let关键字声明变量i：使用let（和const）关键字声明的变量是具有块作用域的（块是{}之间的任何东西）。 在每次迭代期间，i将被创建为一个新值，并且每个值都会存在于循环内的块级作用域。

----

3. 下面代码的输出是什么?

```
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2;
  },
  perimeter: () => 2 * Math.PI * this.radius
};

shape.diameter();
shape.perimeter();
```
A: 20 and 62.83185307179586
B: 20 and NaN
C: 20 and 63
D: NaN and 63

答案: B
请注意，diameter是普通函数，而perimeter是箭头函数。
对于箭头函数，this关键字指向是它所在上下文（定义时的位置）的环境，与普通函数不同！ 这意味着当我们调用perimeter时，它不是指向shape对象，而是指其定义时的环境（window）。没有值radius属性，返回undefined。

----



## 28 道 JavaScript 面试题，看看能错几道？

原文链接：https://mp.weixin.qq.com/s/o260IGR5FxvN-P8Kgtz1uA

1.  下面代码输出什么?

