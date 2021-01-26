---
title: JavaScript 中的循环
key: 20210126
tags: javascript
---

JavaScript 中有很多循环的方式， 那么他们之间有什么不同呢， 今天就来整它一下子

先总结一下， JavaScript 到底有哪些循环方式呢
<!--more-->
1. 普通的 `for` 循环
2. `while` 和 `do ... while`
3. `for ... in ...`
4. `for ... of ...`
5. `for await ... of ...`
6. `Object.keys()`
7. `Object.values()`
8. `Object.entries()`
9. `Object.getOwnPropertyNames()`

目前想到的就以上 9 种方式， 如果有没提到的， 欢迎评论补充， 6、7、8、9 也算是对对象的循环吧， 遂把它们也归为了循环语法， 那么我们开始一个一个介绍吧



### 1. 普通的 for 循环

基本语法如下

```javascript
for (语句 1; 语句 2; 语句 3) {
  // 要执行的代码块
}
// 语句 1 在循环开始前执行，一般用于初始化
// 语句 2 定义运行循环的条件， 当条件不满足时， 则退出循环
// 语句 3 在每次循环代码执行完以后执行
```

这个在平时用的很多， 很平常， 但有一个点， 是我个人比较推荐的， 虽然不知道能不能起到优化作用🤣

```javascript
// 假设有一个数组， 我们要循环打印每一项
const array = [1, 2, 3, 4, 5]
for (let i = 0; i < array.length; i++) {} // 这是平时使用最多方式
// 优化以后的
for (let i = 0, len = array.length; i < len; i++) {}
```

对比上述两种方式， 以及 `for` 循环的语法， 可以发现第二种循环时不用每次去计算 `array.length`, 算是一个小小的优化点吧



### 2. while 和 do ... while

基本语法

```javascript
while(条件) {
  // 要执行的代码块
}

do {
  // 要执行的代码块
} while (条件)
```

`while`  和 `do ... while`  的唯一区别就是 `do ... while` 至少会执行一次

平时工作中感觉没有 `for` 循环出场率高



### 3. for ... in ...

`for ... in ...` 语句以任意顺序遍历一个对象的除 `Symbol` 以外的**可枚举**属性， 有一个点需要注意一下**， `for ... in ..`循环可以遍历到原型链上的属性**

举个🌰

```javascript
Object.prototype.say="hello"
var person ={ age: 18 };
for (var key in person) {
  console.log(key, person[key]);
}
// age 18
// say hello
```

如果想只循环自身的属性， 那么可以配合 `hasOwnProperty` 方法过滤一下， 或者使用一会儿就会介绍到的 `Object.keys()`

```javascript
Object.prototype.say = "hello"
var person ={ age: 18 }
for (var key in person) {
  if(person.hasOwnProperty(key)) {
    console.log(key, person[key])
  }
}
// age 18
```

还有 `for ... in ...` 循环循环是是对象的 `key` 值， 所以对于关注索引顺序的 数组， 不适合使用 `for ... in ...` 循环



### 4. for ... of ...

`for ... of ...` 循环用于迭代一个 **可迭代** 对象（包括 Array, Map, Set, String, arguments 对象等），这里为什么没有 `Object` 呢， 因为 `Object` 不是一个可迭代的对象， 那有没有什么办法在 `Object` 上应用 `for ... of ...` 呢， 当然是有的了， 一会儿介绍。

基本语法

```javascript
for (variable of iterable) {
    //statements
}
```

variable: 在每次迭代中，将不同属性的值分配给变量， **注意**这里迭代的是 **值**， 而 `for ... in ...`迭代的是 **属性**

iterable: 被迭代枚举其属性的对象



如果是遍历 `Object`的话, 需要自定义一个 `Iterator` 接口, 简单的实现如下

```javascript
Object.prototype[Symbol.iterator] = function() {
    let index = 0
    let arr = Object.values(this)
    let length = arr.length
    return {
      next: () => {
        let value = arr[index]
        let done = index >= length
        index++
        return { value, done }
      }
    };
};
```



### 5. for await ... of ...

`for await ... of...` 是 es2018 新增的特性， 异步迭代器， 用于迭代异步操作， 类似 `for ... of ...`， 被迭代的对象必须实现 `AsyncIterator` 函数， 而 `AsyncIterator` 函数返回的是一个 `Promise`, 类似如下

```javascript
const example = {
  [Symbol.asyncIterator]: () => {
    const values = Object.values(this)
    return {
      next: () => Promise.resolve({
        done: values.length === 0,
        value: values.shift()
      })
    }
  }
}
```

如果 `next` 方法返回的 `Promise` 对象被 `reject`， `for await ... of` 就会报错， 要用 `try ... catch` 捕获

```javascript
async function () {
  try {
    for await (const x of createRejectingIterable()) {
      console.log(x);
    }
  } catch (e) {
    console.error(e);
  }
}
```

注意， `for await ... of ` 循环也可以用于同步遍历器

```javascript
(async function () {
  for await (const x of ['a', 'b']) {
    console.log(x);
  }
})();
// a
// b
```



### 6. Object.keys

`Object.keys()` 方法返回给定对象的**自身可枚举属性**组成的**数组**， 数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致

`Object.keys()` 返回一个所有元素为字符串的数组， 该方法与 `for ...in ...` 最大的不同就是只能遍历**自身可枚举属性**



### 7. Object.values

`Object.values` 方法返回一个给定对象自身的所有**可枚举属性值**的**数组**， 值的顺序与使用 `for ...in ...`循环的顺序相同（区别在于 `for ...in ...`循环枚举原型链中的属性）



### 8. Object.entries

`Object.entries()` 方法返回一个给定对象**自身可枚举属性**的**键值对数组**， 其排列与使用`for ...in ...`循环遍历该对象时返回的顺序一致（区别在于 for-in 循环还会枚举原型链中的属性）。



### 9. Object.getOwnPropertyNames()

`Object.getOwnPropertyNames()` 方法返回一个由执行对象的所有**自身属性的属性名**（**包括可枚举属性和不可枚举属性， 但不包括`symbol`值作为名称的属性**）





以上， 整理的比较简洁， 大部分都是参考 `MDN` 整理， 如果还有没有整理到的循环方法， 欢迎评论补充， 谢谢！