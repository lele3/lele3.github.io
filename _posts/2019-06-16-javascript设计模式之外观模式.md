---

title: JavaScript 设计模式之外观模式
key: 20190616
tags: javascript

---

<center><h1>JavaScript设计模式之外观模式</h1></center>

#### 介绍
- 为子系统的一组接口提供一个高层接口
- 使用者使用这个高层接口
#### 举例
当你去医院时， 会有一个接待员，接待员回去给你挂号、门诊、划价、取药等等。
![外观模式详解图](https://raw.githubusercontent.com/lele3/markDownImages/master/images/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/%E5%A4%96%E8%A7%82%E6%A8%A1%E5%BC%8F%E8%AF%A6%E8%A7%A3%E5%9B%BE.jpg)
#### UML类图
![外观模式UML类图](https://raw.githubusercontent.com/lele3/markDownImages/master/images/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/%E5%A4%96%E8%A7%82%E6%A8%A1%E5%BC%8FUML%E7%B1%BB%E5%9B%BE.jpg)

#### 使用场景

```javascript
function bindEvent (elem, type, selector, fn) {
  if (fn === null) {
    fn = selector
    selector = null
  }
  // ...
}

bindEvent(elem, 'click', '#div', fn)
bindEvent(elem, 'click', fn)
```