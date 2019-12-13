---
title: JavaScript中获取DOM元素宽度和高度的常用API
key: 20191208
tags: javascript
---


<center><h1>JavaScript中获取DOM元素宽度和高度的常用API</h1></center>



#### clientWidth 和 clientHeight

**只读属性**

对于内联元素, 其`clientWidth`和`clientHeight`属性值为 0

`Element.clientWidth` 属性表示元素的内部宽度，以像素计。该属性包括内边距，但不包括垂直滚动条（如果有）、边框和外边距
<!--more-->
`Element.clientHeight` 属性同上

**Note：** 该属性值会被四舍五入为一个整数
  <div align='center'>
       <img src='https://raw.githubusercontent.com/lele3/markDownImages/master/images/javascript/JavaScript%E4%B8%AD%E8%8E%B7%E5%8F%96DOM%E5%85%83%E7%B4%A0%E5%AE%BD%E5%BA%A6%E5%92%8C%E9%AB%98%E5%BA%A6%E5%B8%B8%E7%94%A8API/clientWidth%26clientHeight.png' width = '450' height = '300' alt='clientWidth&clientHeight' align='center' />
  </div>
#### offsetWidth、 offsetHeight、 offsetTop 、 offsetLeft 和 offsetParent

**只读属性**

- offsetWidth 和 offsetHeight

  分别表示元素的布局宽度和布局高度，是由 元素的边框、 元素的padding， 滚动条的宽度（如果存在的话）以及CSS设置的宽度

  **note：** 该属性值会被四舍五入为一个整数

  <div align='center'>
       <img src='https://raw.githubusercontent.com/lele3/markDownImages/master/images/javascript/JavaScript%E4%B8%AD%E8%8E%B7%E5%8F%96DOM%E5%85%83%E7%B4%A0%E5%AE%BD%E5%BA%A6%E5%92%8C%E9%AB%98%E5%BA%A6%E5%B8%B8%E7%94%A8API/offsetWidth%26offsetHeight.png' width = '450' height = '300' alt='offsetWidth&offsetHeight' align='center' />
  </div>

- offsetParent

  `Element.offsetParent`返回一个指向最近的（只包含层级的最近）包含该元素的定位元素或者最近的`table, td, th, body`元素， 当元素的`style.display`设置为`none`时， `offsetParent`返回为 `null`

- offsetTop 和 offsetLeft

  了解了`offsetParent`属性以后，这两个属性就比较好理解了， 分别返回当前元素相对于其`offsetParent`元素的顶部和左侧内边距的距离

  **Note：** 该属性值会被四舍五入为一个整数

  

#### scrollWidth、 scrollHeight、 scrollTop 和 scrollLeft

- scrollWidth 和 scrollHeight

  **只读属性**

  分别返回元素内容的宽度和高度，包括由于`overflow`溢出而在屏幕上不可见的内容， 当内容宽度小于元素宽度的时候，返回值同`clientWidth`和`clientHeight`

  <div align='center'>
       <img src='https://raw.githubusercontent.com/lele3/markDownImages/master/images/javascript/JavaScript%E4%B8%AD%E8%8E%B7%E5%8F%96DOM%E5%85%83%E7%B4%A0%E5%AE%BD%E5%BA%A6%E5%92%8C%E9%AB%98%E5%BA%A6%E5%B8%B8%E7%94%A8API/scrollHeight.png' width = '450' height = '300' alt='scrollHeight' align='center' />
  </div>
- scrollTop 和 scrollLeft

  该属性可以获取和设置一个元素的内容的垂直滚动或水平滚动的像素数， 当没有产生滚动条时，这两个属性的值都为 0

  **Note**: 

  1. 如果一个元素**不能被滚动**，（例如， 它没有被溢出，或者这个元素有一个**non-scrollable**属性），他们将被设置为 0
  2. 如果设置的值 **小于0**，则他们的值会被设置为 0
  3. 如果设置了**超出这个容器可滚动的值**，则他们将被设置为最大值 

  

#### getBoundingClientRect

`Element.getBoundingClientRect()`方法返回`Element`元素的大小及其相对视口的位置

该API返回一个`DOMRect`对象， 如下

  <div align='center'>
       <img src='https://raw.githubusercontent.com/lele3/markDownImages/master/images/javascript/JavaScript%E4%B8%AD%E8%8E%B7%E5%8F%96DOM%E5%85%83%E7%B4%A0%E5%AE%BD%E5%BA%A6%E5%92%8C%E9%AB%98%E5%BA%A6%E5%B8%B8%E7%94%A8API/DOMRect.png' width = '450' height = '200' alt='DOMRect' align='center' />
  </div>

`x, y`是该元素左上角相对于浏览器视口左上角的坐标

`bottom， left，right， top`是该元素相对于浏览器视口左上角的距离

`width, height`是该元素的宽度和高度

  <div align='center'>
       <img src='https://raw.githubusercontent.com/lele3/markDownImages/master/images/javascript/JavaScript%E4%B8%AD%E8%8E%B7%E5%8F%96DOM%E5%85%83%E7%B4%A0%E5%AE%BD%E5%BA%A6%E5%92%8C%E9%AB%98%E5%BA%A6%E5%B8%B8%E7%94%A8API/getBoundingClientRect.png' width = '350' height = '400' alt='getBoundingClientRect' align='center' />
  </div>


参考： MDN web docs