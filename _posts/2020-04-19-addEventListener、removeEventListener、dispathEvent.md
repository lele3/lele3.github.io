---
title: addEventListener、removeEventListener、dispathEvent
key: 20200419
tags: javascript
---
<center><h1>addEventListener、removeEventListener、dispathEvent</h1></center>



### addEventListener

**说明**：该方法是在指定dom元素上添加事件，包括浏览器自带事件(例 click)和自定义事件，所有dom节点中都包含这个方法，它接收三个参数： 要处理的事件名、作为事件处理程序的函数以及一个布尔值。布尔值如果为true， 则代表在捕获阶段触发事件，如果为false，则代表在冒泡阶段触发(<font color='red'>这一点老是记反</font>)。
<!--more-->
使用方法如下

```javascript
let btn = document.getElementById('btn')
btn.addEventListener('click', function() {
  console.log('我被点击了')
}, false)
```

上面的代码是在一个id为btn的按钮上添加了一个点击事件，当鼠标点击该按钮时， 会在控制台打印 ‘我被点击了’，且表示在冒泡阶段触发。但是上述代码有个小小的问题，**稍后再说**。

第一种：

```javascript
let btn = document.getElementById('btn')
btn.onclick = function () {
  console.log('我被点击了1')
}
btn.onclick = function () {
  console.log('我被点击了2')
}
```

第二种：

```javascript
let btn = document.getElementById('btn')
btn.addEventListener('click', function() {
  console.log('我被点击了1')
}, false)
btn.addEventListener('click', function() {
  console.log('我被点击了2')
}, false)
```

上述两种在dom上添加事件的方式，当点击按钮时，你认为它们分别会输出几次呢？

。

。

。

第一种只会输出一次，且是后一个添加事件的`console`, 说明后一个添加的事件覆盖掉了前一个，看代码也觉得很合理，`btn.onclick`可以说是一个引用，它只能指向一个函数。第二种答案是两次，也就是说`addEventListener`支持在一个dom上添加多个方法，并且按添加顺序执行，这也是`addEventListener`的特别之处。



### removeEventListener

看函数名就知道它和`addEventListener`是一对儿，它的作用就是移除掉使用`addEventListener`在dom上添加的事件，参数和`addEventListener`必须一致， 否则删除无效。

使用方法如下

```javascript
let btn = document.getElementById('btn')
btn.removeEventListener('click', function() {
  console.log('我被点击了2')
}, false)
```

看一下下面这段代码有几个错误

```javascript
let btn = document.getElementById('btn')
btn.addEventListener('click', function() {
  console.log('我被点击了')
}, false)
btn.removeEventListener('click', function() {
  console.log('我被点击了')
}, true)
```

首先结果是 当你点击按钮时，还是会执行事件，事件并没有移除掉。那么原因究竟是什么呢？此处有两个错误 ① 第二个参数，也就是事件函数，看起来一模一样，但实际上并不是一个函数，在内存中，它们各自占有自己的内存，互不干扰。② 最后一个参数不一致，究其深层原因不是很清楚，但是在浏览器中的表现确确实实是没有移除成功

所以正确的使用方法如下 

```javascript
let btn = document.getElementById('btn')
let handler = function () {
   console.log('我被点击了')
}
btn.addEventListener('click', handler, false)
btn.removeEventListener('click', handler, false)
```

所以，最上面那段代码的问题就是无法移除添加的事件。

### dispatchEvent

作用： 触发自定义事件

举个简单的例子，我们使用`addEventListener`在`document`上绑定了一个自定义事件，等到某个时机我们手动去触发它执行，那么我们就可以使用`dispatchEvent`

```javascript
let myFun = function () {
  console.log('我笑了')
}
// 在document上绑定了一个名为haha的自定义事件
document.addEventListener('haha', myFun)
// 触发过程
// 1. 创建事件
const event = new Event('haha')
// 2. 可以在事件对象上绑定一些参数（看需要）
event.reason = '有好事发生'
// 3. 触发
document.dispatchEvent(event)
```



参考： 《JavaScript 高级程序设计》