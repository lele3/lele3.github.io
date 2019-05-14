---

title: JavaScript 常用设计模式（一）
key: 20190514
tags: javascript

---

<center><h1>javaScript 常用的设计模式（一）</h1></center>

### 1. 设计模式的历史

​		设计模式起源于建筑行业，一位名叫Alexander的建筑师发现并总结了一些建筑行业的设计模式。在20世纪90年代，准确地说是1995年，由于Erich Gamma、Richard Helm、Ralph Johnson、John Vlissides合著的《设计模式--可复用面向对象软件的基础》一书的出版，正式掀开了软件业设计模式的序幕，这本书的四位作者被世人称为四人组，也有人将其称为"四人帮"，也就是大家常见的GoF（Gang of Four）。



### 2. 什么是模式

​        设计模式是一套被 **反复使用**、**多数人知晓**、**经过分类编目的**、**代码设计经验的总结**。**使用设计模式是为了可重用代码、让代码更容易被他人理解并且保证代码可靠性。**



### 3. 设计模式的分类

- **创建型设计模式**

  **主要用于描述如何创建对象**

  ​        主要包括： Constructor(构造器)、Factory(工厂)、Abstract(抽象)、Prototype(原型)、Singleton(单例)和Builder(生成器)。	

- **结构型设计模式**

  **主要用于描述如何实现类或对象的组合**

​       	   主要包括：Decorator(装饰者)、 Facade(外观)、Flyweight(享元)、Adapter(适配器）和Proxy(代理)。

- **行为型设计模式**

  **主要用于描述类或对象怎样交互以及怎样分配职责**

  ​       主要包括： Iterator(迭代器)、Mediator(中介者)、Observer(观察者)和Visitor(访问者)。

  

### 4. 常用的设计模式

####  1. Constructer(构造器)模式 

1. 对象的创建

   ```javascript
   // 创建新对象的三种方法
   // 1. 对象字面量
   let newObject = {}
   
   // 2. create方法
   let newObject1 = Object.create(null)
   
   // 3. new
   let newObject2 = new Object()
   
   // 给对象绑定属性的四种方法
   // 1. 点语法
   newObject.someKey = 'hello world 1'
   // 获取属性
   let someKey = newObject.someKey
   
   // 2.中括号语法
   newObject['anotherKey'] = 'hello world 2'
   // 获取属性
   let anotherKey = newObject['anotherKey']
   
   // 3. Object.defineProperty
   Object.defineProperty(newObject, 'otherKey', {
     value: 'hello world 3',
     writable: true,
     enumerable: true,
     configurable: true
   })
   
   // 4. Object.defineProperties
   Object.defineProperties(newObject, {
     'key1': {
       value: 'hello',
       writable: true
     },
     'key2': {
       value: 'world',
       writable: true
     }
   })
   ```

2. 基本Constructor

   ```javascript
   // 声明一个构造函数
   function Person(name, age, gender) {
     this.name = name
     this.age = age
     this.gender = gender
   
     this.sayHi = function () {
       console.log(`${this.name} say hi !`)
     }
   }
   
   let person1 = new Person('小明', 20, '男')
   person1.sayHi()
   
   let person2 = new Person('小红', 18, '女')
   person2.sayHi()
   ```

   <details>
   <summary>问题一： 调用new 操作符后， 会执行哪几步来生成对象？？</summary>
    1. 创建一个新对象<br />
    2. 将构造函数的作用域赋给新对象(因此this就指向了这个新对象)<br />
    3. 执行构造函数中的代码<br />
    4. 返回新对象
   </details>

    <details>
       <summary>问题二： person1.sayHi === person2.sayHi 吗？？</summary>
         不等于，person1和person2是两个不同的对象， 每个对象中的sayHi指向不同的地址，所以不一样。
       </details>

3. 带原型的Constructor

   ```javascript
   // 声明一个构造函数
   function Person(name, age, gender) {
     this.name = name
     this.age = age
     this.gender = gender
   }
   
   Person.prototype.sayHi = function () {
       console.log(`${this.name} say hi !`)
   }
   
   let person1 = new Person('小明', 20, '男')
   person1.sayHi()
   
   let person2 = new Person('小红', 18, '女')
   person2.sayHi()
   
   console.log(person1.sayHi === person2.sayHi)
   ```

4. 使用场景

   构造函数模式通常使用在对象的属性较少，相对来说比较简单的地方，比如仅仅需要包装出一个对象，或者想用一个变量存储几个属性。

   

####     2. Module(模块)模式

1. 立即执行函数(Immediately Invoked Function Expression)

   ```javascript
   var a = 'I am in outside';
   (function () {
     var a = 'I am in inside'
     console.log('内部 a', a)
   }())
   console.log('外部 a',  a)
   ```

2. Module 模式

   ```javascript
   var basketModule = (function () {
     var basket = []
     function doSomethingPrivite() {
   		  // ...
     }
     function doSomethingElsePrivite () {
   			// ...
     }
   
     return {
       addItem: function (values) {
         basket.push(values)
       },
       getItemCount: function () {
         return basket.length
       },
       getTotal: function () {
         var itemCount = this.getItemCount(), total = 0
         while(itemCount--) {
           total += basket[itemCount].price
         }
         return total
       }
     }
   }())
   
   basketModule.addItem({
     item: 'a',
     price: 2
   })
   basketModule.addItem({
     item: 'b',
     price: 5
   })
   let total = basketModule.getTotal()
   console.log('total', total)
   ```

3. 使用场景

   为JavaScript提供类似 类 的功能， 使对象拥有公有和私有属性及函数之分。

   

4. Module 模式基本特征

   1. 模块化，可重用
   2. 封装了变量和function，和全局的namaspace不接触，松耦合
   3. 只暴露可用public的方法，其它私有方法全部隐藏

####     3. Revealing Module(揭示模块)模式

1. 说明

   揭示模块模式是一种对模块模式的改进，在私有范围内定义所有的属性和方法，并返回一个匿名对象，它拥有指向私有方法和属性的指针。

2. 代码

   ```javascript
   var myRevealingModule = (function () {
     var _variable = 'hello'
     var variable = 'world'
   
     function _fun () {
       console.log(`我是私有函数${_variable}`)
     }
   
     function fun1 () {
       console.log(`I say ${variable}`)
     }
    
     function fun2 () {
       _fun()
     }
     return {
       say: fun1, 
       introduce: fun2
     }
   }())
   myRevealingModule.say()
   myRevealingModule.introduce()
   ```

   

####     4. Singleton(单例)模式

1. 定义

   保证一个类仅有一个实例，并提供一个访问它的全局访问点。实现的方法为先判断实例存在与否，如果存在则直接返回，如果不存在就创建了再返回，这就确保了一个类只有一个实例对象。

2. 代码

   ```javascript
   var Singleton = (function(){
   		
     var instance;
   
     function Person (name) {
       this.name = name
   
       this.sayName = function () {
         console.log(`My name is ${name}`)
       }
     }
   
     return {
       getInstance: function (name) {
         if (!instance) {
           instance = new Person(name)
         }
         return instance
       }
     }
   }())
   
   var singleton1 = Singleton.getInstance('小明')
   singleton1.sayName()
   var singleton2 = Singleton.getInstance('小红')
   singleton2.sayName()
   
   console.log('singleton1和singleton2是否相等？？', singleton1 === singleton2) // true
   ```



参考：

JavaScript设计模式 / (美) 奥斯马尼 著， 徐涛译. --北京：人民邮电出版社：  https://addyosmani.com/resources/essentialjsdesignpatterns/book/#modulepatternjavascript



  