---

title: JavaScript 设计模式之工厂模式
key: 20190528
tags: javascript

---

<center><h1>JavaScript设计模式之工厂模式</h1></center>



#### 介绍

​工厂模式主要用于实例化对象，它抽象了创建具体对象的过程。当你遇到new关键字的时候就可以考虑考虑是否可以使用工厂模式。

#### 举例

当你去KFC买汉堡时，你只需要点餐付钱，之后等着服务员叫号就行。这个过程就可以看做是应用工厂模式的情况，你不用自己去做汉堡吧，做汉堡（new Hamburger()）的方法被封装在了KFC商店里。你只需要点餐付钱(可以看做汉堡店给你提供的方法)，就可以吃到美味的汉堡包。

#### UML类图

![工厂模式的UML类图](https://raw.githubusercontent.com/lele3/markDownImages/master/images/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/%E5%B7%A5%E5%8E%82%E6%A8%A1%E5%BC%8FUML%E7%B1%BB%E5%9B%BE.jpg)

#### 代码

```javascript
class Product {
	constructor(name) {
		this.name = name
		this.createTime = Date.now()
	}
	
	fun1 () {
		console.log('fun1')
	}
	fun2() {
		console.log('fun2')
	}	
}

class Factory {
	factory (name) {
		return new Product(name)
	}
}

// 初始化一个工厂
let factory = new Factory()
// 生产一个产品
let product = factory.factory('产品1')
product.fun1()
product.fun2()
```

#### 使用场景

1. Jquery 中$的可以如下生成

   ```javascript
   class Jquery {
   	constructor (selector) {
   		let dom = document.querySelectAll(selector)
   		// 一些操作
   	}
   }
   
   window.$ = function (selector) {
   	return new Jquery(selector)
   }
   ```

2. React 中createElement函数

   ```javascript
   function VNode (/* 参数 */) {
   	// ...
   }
   
   React.createElement (/* 参数 */) {
   	return new VNode(/* 参数 */)
   }
   ```