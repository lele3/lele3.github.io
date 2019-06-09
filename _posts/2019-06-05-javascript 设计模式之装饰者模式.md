---

title: JavaScript 设计模式之装饰者模式
key: 20190605
tags: javascript

---

<center><h1>JavaScript设计模式之装饰者模式</h1></center>

#### 介绍
- 为对象添加新功能
- 不改变其原有的结构和功能

#### 举例
正如这个模式的名字一样，装饰一个原有的对象，装饰的意思就是不改变原有对象的东西，只是新增。
现实生活中的例子有很多，比如手机壳， 手机的所有功能正常使用， 新增手机壳的一些属性， 还有灯罩， 灯的功能完全不影响，新增灯罩的功能。
#### UML类图
![装饰者模式UML类图](https://raw.githubusercontent.com/lele3/markDownImages/master/images/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/%E8%A3%85%E9%A5%B0%E8%80%85%E6%A8%A1%E5%BC%8FUML%E7%B1%BB%E5%9B%BE.jpg)
#### 代码
```javascript
class Cirlce {
  draw () {
    console.log('画圈')
  }
}
class Decorator {
  constructor (circle) {
    this.circle = circle
  }
  draw () {
    this.circle.draw()
    this.setRedBorder(circle)
  }
  setRedBorder(circle) {
    console.log('画红色边框')
  }
}

let circle = new Circle()
circle.draw()
let dec = new Decorator()
dec.draw()
```
#### 使用场景
1. ES7 装饰器
   - http://es6.ruanyifeng.com/#docs/decorator （阮一峰老师的ES6教程）
2. 第三方库 core-decorators
   环境配置
   - 安装插件 babel-plugin-transform-decorators-legacy
   - 配置.babelrc
   ```javascript
   {
     "presets": ["es2015", "latest"],
     "plugins": ["transform-decorators-legacy"]
   }
   ```
